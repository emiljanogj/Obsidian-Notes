### On-Premises

- To proxy out traffic from our local machine to a remote machine via a proxy we use the following command:
```
ssh -L {local-port}:{REMOTE-IP}:{REMOTE-PORT} {username}@{PROXY-IP}
```

- If we can cat file always make sure to also `cat .bash_history`

**Flag 4**

- For the 4th flag we exploit the following line:
```
cfg.vm.synced_folder "/home/ubuntu/", "/tmp/datacopy"
```
This means that `/home/ubuntu/` folder, where we have no access to as user `entry` is mapped to `/tmp/datacopy` and vice versa => If we modify `authorized_keys` file with a public key we can then login as user `ubuntu`. 

- We than run the following command:
```
ubuntu@tryhackme:~/.ssh$ sudo -l
Matching Defaults entries for ubuntu on tryhackme:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User ubuntu may run the following commands on tryhackme:
    (ALL : ALL) ALL
    (ALL) NOPASSWD: ALL
    (ALL) NOPASSWD: ALL
    (ALL) NOPASSWD: ALL
    (ALL) NOPASSWD: ALL
    (ALL) NOPASSWD: ALL

```
And we see that it can run sudo commands with no passwords needed.


### Cloud-based IaC

```javascript
  // Main.tf
	provider "aws" {
	   region = "EU-Central-1"
   }
  
  // Create a VPC with necessary settings
   resource "aws_vpc" "main" {
	 cidr_block = "10.0.0.0/16"
	 enable_dns_support = true
	 enable_dns_hostnames = true
	 tags = {
	   Name = "main-vpc"
	 }
   }
  
  // Create a public subnet
   resource "aws_subnet" "public" {
	 vpc_id = aws_vpc.main.id
	 cidr_block = "10.0.0.0/24"
	 availability_zone = "eu-central-1a"
	 map_public_ip_on_launch = true
	 tags = {
	   Name = "public-subnet"
	 }
   }
   // Add a security group

// ***Change_1
// Create a ______________ for filtering inbound SSH and outbound traffic
	resource "aws_security_group" "allow_ssh" {
	vpc_id = aws_vpc.main.id
	
		egress {
			from_port = 0
			to_port = 0
			protocol = "-1"
			cidr_blocks = ["0.0.0.0/16"]
		}
		ingress {
			from_port = 22
			to_port = 22
			protocol = "tcp"
			cidr_blocks = ["0.0.0.0/16"]
		}
		tags = {
			Name = "allow-ssh-sg"
		}
	}
  
  // Create an IAM role for EC2 instances 
  // in the Auto Scaling Group
   resource "aws_iam_role" "instance_role" {
	 name = "ec2-instance-role"
	 assume_role_policy = <<EOF
	 {
	   "Version": "2012-10-17",
	   "Statement": [
		 {
		   "Action": "sts:AssumeRole",
		   "Effect": "Allow",
		   "Principal": {
			 "Service": "ec2.amazonaws.com"
		   }
		 }
	   ]
	 }
	 EOF
   }

  // ***Change_2
  // Attach an IAM policy to the role for basic EC2 permissions
   resource "aws_iam_role_policy_attachment" 
   "instance_role_attachment" {
	 policy_arn = "arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess"
	 role = aws_iam_role.instance_role.name
   }
  
  // Create an Auto Scaling Group
   resource "aws_autoscaling_group" "T800" {
	 desired_capacity = 2
	 max_size = 5
	 min_size = 1
	 vpc_zone_identifier = [aws_subnet.public.id]
	 health_check_type = "EC2"
	 health_check_grace_period = 300
	 force_delete = true
	 launch_configuration = aws_launch_configuration.example.id
   }
  
  // Create a launch configuration 
  // (these are the config settings for the AG)
   resource "aws_launch_configuration" "example" {
	 name = "example-config"
	 image_id = "ami-xxxxxx"
	 instance_type = "t2.micro"
	 key_name = "your_key_pair_name"
	 security_groups = [aws_security_group.allow_ssh.id]
	 iam_instance_profile = aws_iam_instance_profile.example.name
	 user_data = <<-EOF
	 EOF
   }

  // ***Change_3
  // Create an IAM instance profile for the EC2 instances
   resource "aws_iam_instance_profile" "T800" {
	 name = "service_account_profile"
	 roles = [aws_iam_role.instance_role.name]
   }

// ***Change_4
// Enable VPC Flow Logs for network traffic logging
	resource "aws_flow_log" "trylink_trail" {
		depends_on = [aws_subnet.public]
		iam_role_arn = aws_iam_role.instance_role.arn
		log_destination = "arn:aws:logs:your_aws_region:T800:log-group:/resistance-logs"
		traffic_type = "ALL"
		log_format = "PLAINTEXT"
		max_aggregation_interval = 60
		subnet_id = aws_subnet.public.id
	}

// ***Change_5

//Enable CloudTrail for AWS API call logging 
	resource "aws_cloudtrail" "trylink" {
		name = "trylink-cloudtrail"
	    s3_bucket_name = "s3_trylink"
	}
```

**Note**: The resource keyword is followed by the resource type(standard as defined by Terraform) + the resource name(custom)