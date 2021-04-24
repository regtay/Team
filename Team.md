# Team


```
nmap -sC -sV 10.10.171.128
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-23 17:09 CDT
Nmap scan report for 10.10.171.128
Host is up (0.13s latency).
Not shown: 997 filtered ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 79:5f:11:6a:85:c2:08:24:30:6c:d4:88:74:1b:79:4d (RSA)
|   256 af:7e:3f:7e:b4:86:58:83:f1:f6:a2:54:a6:9b:ba:ad (ECDSA)
|_  256 26:25:b0:7b:dc:3f:b2:94:37:12:5d:cd:06:98:c7:9f (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works! If you see this add 'te...
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.58 seconds
```

Navigated to port 80

viewed the source page

```
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Apache2 Ubuntu Default Page: It works! If you see this add 'team.thm' to your hosts!</title>
    <style type="text/css" media="screen">
```
* this looks interesting

```
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://team.thm -x php,html,txt,zip,doc
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://team.thm
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              zip,doc,php,html,txt
[+] Timeout:                 10s
===============================================================
2021/04/23 19:08:15 Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 305] [--> http://team.thm/images/]
/index.html           (Status: 200) [Size: 2966]                             
/scripts              (Status: 301) [Size: 306] [--> http://team.thm/scripts/]
/assets               (Status: 301) [Size: 305] [--> http://team.thm/assets/]
/robots.txt           (Status: 200) [Size: 5]
```

On Workstation Machine

```
nano /etc/hosts

Add <ipaddress>   team.thm
```

```
wfuzz -c --hw 977 -u http://team.thm -H "Host: FUZZ.team.thm" -w ~/Documents/tryhackme/SecLists/Discovery/DNS/subdomains-top1million-5000.txt

********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://team.thm/
Total requests: 4989

=====================================================================
ID           Response   Lines    Word       Chars    Payload  
=====================================================================

000000001:   200        89 L     220 W      2966 Ch  "www"
000000019:   200        9 L      20 W       187 Ch   "dev"
000000085:   200        9 L      20 W       187 Ch   "www.dev"
000000689:   400        12 L     53 W       422 Ch   "gc._msdcs"
```

went back to the host file

added dev.team.thm

* once added to the I navigated to http://dev.team.thm

Interesting I see a link "Place holder link to team share"

Well lets see what Burpsuite has to say about this
