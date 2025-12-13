# Slayer
![slayer](../assets/images/slayer.png)

??? info
    Author: Dcyberguy

    Status: Published

    Last edited time: November 20, 2025 10:14 PM

    Created time: November 13, 2025 2:57 PM

    Tags: Windows

**`This is a writeup for the Hacksmarter Lab machine: Slayer`**

## Host information
```abap
- IP Address: ``10.1.246.7``
- Difficulty: Easy
- Operating System: Windows
- Assumed Breach Creds: tyler.ramsey:P@ssw0rd!
```
## Enumeration

```jsx
nmap -A 10.1.246.7 --unprivilege -Pn
Starting Nmap 7.98 ( https://nmap.org ) at 2025-11-13 12:03 -0800
Nmap scan report for 10.1.246.7
Host is up (0.027s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
445/tcp  open  microsoft-ds?
3389/tcp open  ms-wbt-server
|_ssl-date: TLS randomness does not represent time
| rdp-ntlm-info:
|   Target_Name: EC2AMAZ-M1LFCNO
|   NetBIOS_Domain_Name: EC2AMAZ-M1LFCNO
|   NetBIOS_Computer_Name: EC2AMAZ-M1LFCNO
|   DNS_Domain_Name: EC2AMAZ-M1LFCNO
|   DNS_Computer_Name: EC2AMAZ-M1LFCNO
|   Product_Version: 10.0.26100
|_  System_Time: 2025-11-13T20:04:31+00:00
| ssl-cert: Subject: commonName=EC2AMAZ-M1LFCNO
| Not valid before: 2025-09-29T10:51:47
|_Not valid after:  2026-03-31T10:51:47
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3389-TCP:V=7.98%I=7%D=11/13%Time=691639CE%P=i686-pc-windows-windows
SF:%r(TerminalServerCookie,13,"\x03\0\0\x13\x0e\xd0\0\0\x124\0\x02/\x08\0\
SF:x02\0\0\0");
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time:
|   date: 2025-11-13T20:04:33
|_  start_date: N/A
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 89.19 seconds
Commando VM 11/13/2025 12:05:12
```

Three (3) open ports (SMB, MSRPC, RDP)

## SMB Enumeration

Read on IPC$ Share is not possible

```jsx
nxc smb 10.1.246.7 -u tyler.ramsey -p 'P@ssw0rd!' --shares
SMB         10.1.246.7      445    EC2AMAZ-M1LFCNO  [*] Windows 11 / Server 2025 Build 26100 x64 (name:EC2AMAZ-M1LFCNO) (domain:EC2AMAZ-M1LFCNO) (signing:False) (SMBv1:None)
SMB         10.1.246.7      445    EC2AMAZ-M1LFCNO  [+] EC2AMAZ-M1LFCNO\tyler.ramsey:P@ssw0rd!
SMB         10.1.246.7      445    EC2AMAZ-M1LFCNO  [*] Enumerated shares
SMB         10.1.246.7      445    EC2AMAZ-M1LFCNO  Share           Permissions     Remark
SMB         10.1.246.7      445    EC2AMAZ-M1LFCNO  -----           -----------     ------
SMB         10.1.246.7      445    EC2AMAZ-M1LFCNO  ADMIN$                          Remote Admin
SMB         10.1.246.7      445    EC2AMAZ-M1LFCNO  C$                              Default share
SMB         10.1.246.7      445    EC2AMAZ-M1LFCNO  IPC$            READ            Remote IPC
```

But we can see the user can rdp onto the affected system

```jsx
nxc rdp 10.1.246.7 -u tyler.ramsey -p 'P@ssw0rd!'
RDP         10.1.246.7      3389   EC2AMAZ-M1LFCNO  [*] Windows 10 or Windows Server 2016 Build 26100 (name:EC2AMAZ-M1LFCNO) (domain:EC2AMAZ-M1LFCNO) (nla:True)
RDP         10.1.246.7      3389   EC2AMAZ-M1LFCNO  [+] EC2AMAZ-M1LFCNO\tyler.ramsey:P@ssw0rd! (Pwn3d!)
 dcyberguy on  ~
```

I will use the Remote Desktop Client

## Privilege Escalation

Looking at `Powershell history`, which is the first place attacker look, you will notice the Administrator ran a command in the Powershell terminal and included his password.

![slayer1](../assets/images/slayer1.png)


You can now access the Administrator’s Desktop and get the root flag

![slayer2](../assets/images/slayer2.png)