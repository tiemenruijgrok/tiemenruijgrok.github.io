---
title: 'HTB Editorial Writeup'
date: 2024-06-26
permalink: /posts/2024/06/HTB-Editorial-Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Editorial" with easy difficulty

<img src='/images/HTB_Editorial/Screenshot 2024-06-26 191746.png'>

**Welcome to this writeup of the easy HTB machine Editorial. I will take you through the process and explain how I captured the flags of this machine.**

Enumeration
======
As always, I run an Nmap scan on the target IP to scan for open ports.

<img src='/images/HTB_Editorial/Screenshot 2024-06-26 192124.png'>

We see that ports 22 and 80 are open, but sometimes there could be unusual open ports that Nmap can’t detect with a default scan. 

If there is a website running on the machine, I check the IP in the browser. 

<img src='/images/HTB_Editorial/Screenshot 2024-06-26 192218.png'>

Yes, there is. We need to add the site to our hostname file to access the site. This can be done with: `echo "10.10.11.20  editorial.htb" | sudo tee -a /etc/hosts`

<img src='/images/HTB_Editorial/Screenshot 2024-06-26 220450.png'>

Now we can access the site.

<img src='/images/HTB_Editorial/Screenshot 2024-06-26 221415.png'>

It looks like a book website, Editorial Tiempo Arriba means Editorial Time Up in English.

There is a search field which changes the URL directory to a question mark if you search for something. 

You can also subscribe to their newsletter with an email address. 

In the “Publish with us” page, you can fill in details about your (imaginary) book which they will publish for you. 

Oeh you can upload a file to this page. This could be an interesting entry point. 

<img src='/images/HTB_Editorial/Screenshot 2024-06-26 223031.png'>

On the last page of the website, there is a contact email address: submissions@tiempoarriba.htb

Next, I will run a directory scan with dirsearch:

```python
dirsearch -u http://editorial.htb -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt
```
<img src='/images/HTB_Editorial/Screenshot 2024-06-26 230303.png'>

Nothing new to be found here. 

I will continue trying to exploit the file upload and input forms. 

User flag
======

I will try to upload a malicious file in the book information section. 

This must be an image.

<img src='/images/HTB_Editorial/Screenshot 2024-06-26 231021.png'>

I uploaded the image and intercepted the submit request with Burp Suite.  

Add the .php extension to the image.  

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231142.png'>

However, I sent the request with a listener on, but no response. 

I think we are on the right track with, because this is a sign of SSRF. 

Then I filled in the localhost in the bookurl field. I got an image URL in response: 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231255.png'>

It downloaded the image with no useful data. 

Next, I will try to brute force the “bookurl” with all possible ports. Maby there is an image behind a specific port on the server. 

Send the request to the Intruder in Burp Suite.

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231341.png'>

Set the Payload type to Numbers and set the Payload settings to “From: 1 to: 65535”. 

After running the attack, we have loads of jpeg images. After filtering out the usual jpeg endpoint, the port 5000 has a different endpoint in the response. 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231423.png'>

I tried this endpoint in the browser on the site and it downloaded the file. 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231513.png'>

The file gives us some kind of API endpoints. Let’s try them all. 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231623.png'>

With this endpoint the downloaded file gives us very valuable information.

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231702.png'>

There is a username and password present in the file.  

Now is the question, where can we use these credentials? 

Remember in the beginning where we found that port 22 is open? Let’s try the credentials here.  

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231805.png'>

And we are in! 

The user flag is in the current directory. 

Root flag
======
Next is the user flag by escalating privileges, now it becomes a bit harder. 

I searched in the directories for clues and found a .git in apps: 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231845.png'>

After some more searching, I found something interesting. 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231918.png'>

A git log says ”downgrading prod to dev”. Dev is the current user, so prod could be another user we can connect to with SSH. 

Let’s see what is in the commit: 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 231956.png'>

Ohh we can see another user with a password. 

Let’s try to connect:

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 232053.png'>

And we are in again!! But there is nowhere a root flag to be seen. 

There are no obvious leads here unfortunately, but with `sudo –l` I can see in what directory sudo can be run. 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 232133.png'>

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 232156.png'>

After searching more directories and listing the apps, I found a vulnerable pip package with pip3 list. 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 232227.png'>

GitPython 3.1.29 contains an RCE vulnerability found here: [https://github.com/gitpython-developers/GitPython/issues/1515](https://github.com/gitpython-developers/GitPython/issues/1515)

We must add `'ext::sh -c touch% /tmp/pwned'` behind the `sudo –l` details I gained earlier. 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 232411.png'>

The error describes that I don't have the correct access right, so it executes as the root user. The created /tmp/pwned dir is now present but it is empty.  

We need to define the root directory to gain the root.txt file. 

<img src='/images\HTB_Editorial\Screenshot 2024-06-26 232515.png'>

The command executed a shell command to cat the root.txt file and put it in the /tmp/root directory. This way, we obtained the root flag!! 

**Thanks for reading this far and enjoy the rest of your day!**