---
title: 'HTB Titanic Writeup'
date: 2025-07-03
permalink: /posts/2025/07/HTB_Titanic_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Titanic" with easy difficulty

# HTB_Titanic (not done)

Hi, welcome to this writeup of the Hack the Box machine Titanic with easy difficulty. 

![image.png](/images/HTB_Titanic/image.png)

It’s a retired Linux machine. 

`nmap -sV -sC 10.10.11.55` 

![image.png](/images/HTB_Titanic/image%201.png)

There is a website: `http://titanic.htb/`

Add this to the hosts: `echo "10.10.11.55 titanic.htb" | sudo tee -a /etc/hosts`

It looks like a booking website where you can book a trip on the Titanic. 

![image.png](/images/HTB_Titanic/image%202.png)

When clicking on “Book Your Trip” you can enter a few details in a modal. Maybe some injection is possible here.

This is the only actions we can do on the site it seems. 

In the source code of the page can be seen that Popperjs is being used, version 2.5.3. Some kind of JavaScript framework. 

Let’s see what happens when we fill in the form and submit: a json file downloads with the information we filled in.  

![image.png](/images/HTB_Titanic/image%203.png)

We can run a scan on the domain to see if there are any subdomains: