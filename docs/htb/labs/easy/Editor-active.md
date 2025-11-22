# Editor

## IP Address = 10.10.11.80

```bash
echo "10.10.11.80 editor.htb" | sudo tee -a /etc/hosts
```

## Enumeration
```bash
nmap 10.10.11.80 -sCV    
Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-15 15:58 EDT
Nmap scan report for editor.htb (10.10.11.80)
Host is up (0.037s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 3e:ea:45:4b:c5:d1:6d:6f:e2:d4:d1:3b:0a:3d:a9:4f (ECDSA)
|_  256 64:cc:75:de:4a:e6:a5:b4:73:eb:3f:1b:cf:b4:e3:94 (ED25519)
80/tcp   open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Editor - SimplistCode Pro
8080/tcp open  http    Jetty 10.0.20
| http-methods: 
|_  Potentially risky methods: PROPFIND LOCK UNLOCK
|_http-open-proxy: Proxy might be redirecting requests
| http-cookie-flags: 
|   /: 
|     JSESSIONID: 
|_      httponly flag not set
|_http-server-header: Jetty(10.0.20)
| http-webdav-scan: 
|   WebDAV type: Unknown
|   Server Type: Jetty(10.0.20)
|_  Allowed Methods: OPTIONS, GET, HEAD, PROPFIND, LOCK, UNLOCK
| http-robots.txt: 50 disallowed entries (15 shown)
| /xwiki/bin/viewattachrev/ /xwiki/bin/viewrev/ 
| /xwiki/bin/pdf/ /xwiki/bin/edit/ /xwiki/bin/create/ 
| /xwiki/bin/inline/ /xwiki/bin/preview/ /xwiki/bin/save/ 
| /xwiki/bin/saveandcontinue/ /xwiki/bin/rollback/ /xwiki/bin/deleteversions/ 
| /xwiki/bin/cancel/ /xwiki/bin/delete/ /xwiki/bin/deletespace/ 
|_/xwiki/bin/undelete/
| http-title: XWiki - Main - Intro
|_Requested resource was http://editor.htb:8080/xwiki/bin/view/Main/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.36 seconds
```

Discovered two web services, one on port 80 and another on port 8080. The one on port 8080 seems to be running XWiki.
### Port 80
Navigating to the web page, we see a landing page for "Editor - SimplistCode Pro".
![image](images/editor-1.png)

Tried some common paths, but nothing interesting was found.
### Port 8080
Navigating to the web page, we see an XWiki instance.

![image](images/editor-1.png)

#### Subdomain Enumeration
```bash
gobuster dir -u http://editor.htb:8080 -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt --exclude-length 0
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://editor.htb:8080
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] Exclude Length:          0
[+] User Agent:              gobuster/3.8
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/robots.txt           (Status: 200) [Size: 1842]
Progress: 4746 / 4746 (100.00%)
===============================================================
Finished
===============================================================
```
Found a robots.txt file.
```bash
curl http://editor.htb:8080/robots.txt
curl http://editor.htb:8080/robots.txt

User-agent: *
# Prevent bots from executing all actions except "view" and
# "download" since:
# 1) we don't want bots to execute stuff in the wiki by
#    following links! (for example delete pages, add comments,
#    etc)
# 2) we don't want bots to consume CPU and memory
#   (for example to perform exports)
Disallow: /xwiki/bin/viewattachrev/
Disallow: /xwiki/bin/viewrev/
Disallow: /xwiki/bin/pdf/
Disallow: /xwiki/bin/edit/
Disallow: /xwiki/bin/create/
Disallow: /xwiki/bin/inline/
Disallow: /xwiki/bin/preview/
Disallow: /xwiki/bin/save/
Disallow: /xwiki/bin/saveandcontinue/
Disallow: /xwiki/bin/rollback/
Disallow: /xwiki/bin/deleteversions/
Disallow: /xwiki/bin/cancel/
Disallow: /xwiki/bin/delete/
Disallow: /xwiki/bin/deletespace/
Disallow: /xwiki/bin/undelete/
Disallow: /xwiki/bin/reset/
Disallow: /xwiki/bin/register/
Disallow: /xwiki/bin/propupdate/
Disallow: /xwiki/bin/propadd/
Disallow: /xwiki/bin/propdisable/
Disallow: /xwiki/bin/propenable/
Disallow: /xwiki/bin/propdelete/
Disallow: /xwiki/bin/objectadd/
Disallow: /xwiki/bin/commentadd/
Disallow: /xwiki/bin/commentsave/
Disallow: /xwiki/bin/objectsync/
Disallow: /xwiki/bin/objectremove/
Disallow: /xwiki/bin/attach/
Disallow: /xwiki/bin/upload/
Disallow: /xwiki/bin/temp/
Disallow: /xwiki/bin/downloadrev/
Disallow: /xwiki/bin/dot/
Disallow: /xwiki/bin/delattachment/
Disallow: /xwiki/bin/skin/
Disallow: /xwiki/bin/jsx/
Disallow: /xwiki/bin/ssx/
Disallow: /xwiki/bin/login/
Disallow: /xwiki/bin/loginsubmit/
Disallow: /xwiki/bin/loginerror/
Disallow: /xwiki/bin/logout/
Disallow: /xwiki/bin/lock/
Disallow: /xwiki/bin/redirect/
Disallow: /xwiki/bin/admin/
Disallow: /xwiki/bin/export/
Disallow: /xwiki/bin/import/
Disallow: /xwiki/bin/get/
Disallow: /xwiki/bin/distribution/
Disallow: /xwiki/bin/jcaptcha/
Disallow: /xwiki/bin/unknown/
Disallow: /xwiki/bin/webjars/
```

