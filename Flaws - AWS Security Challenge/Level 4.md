#AWS
***

- Get the account ID
```
aws --profile flaws sts get-caller-identity
```

- To list the buckets that an account can view we use the following command:
```
aws --profile flaws s3 ls
```
*Note*: In AWS we cannot give a permission to a user to only list 1 bucket. If they need to list 1 bucket => they can list all buckets

- To read a file in a given bucket we use the following command:
```
aws s3 --profile flaws ls s3://level3-9afd3927f195e10225021a578e6f78df.flaws.cloud/{filename}
```

- To list the EC2 snapshots in a specific region, we use the following command:
```
aws --profile flaws ec2 describe-snapshots --owner-ids 975426262029 --region us-west-2
```

- To create a volume using a snapshot, we use the following command:
```
aws --profile YOUR_ACCOUNT ec2 create-volume --availability-zone us-west-2a --region us-west-2  --snapshot-id  snap-0b49342abd1bdcb89
```

- To attach a volume to an EC2 instance we use the following command:
```
aws ec2 attach-volume --volume-id your-volume-id --instance-id your-instance-id --device /dev/sdf
```

- To create a profile we need to use the following command:
```
aws configure --profile {profile-name}
```
Then we need to provide the `AccessKeyId` and `SecretAccessKey`in the wizard