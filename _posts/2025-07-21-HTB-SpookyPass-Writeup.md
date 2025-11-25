---
title: 'HTB SpookyPass Writeup (challenge)'
date: 2025-07-21
permalink: /posts/2025/07/HTB_SpookyPass_Writeup/
tags:
  - HTB
  - writeup
  - very-easy
  - challenge
---

A writeup of the Hack The Box challenge "SpookyPass" with very easy difficulty

# SpookyPass (very easy)

This isn’t a machine but a zip file with a password that is given to us.

When extracted there is a folder called `rev_spookypass` 

Inside this folder there is an executable file called `pass`

Opening the file in VScode: well, unreadable.

Looking at the file via the terminal:

![image.png](/images/challenges/HTB_SpookyPass/image.png)

It is a ELF 64-bit LSB pie executable.

Executing the file `./pass`

![image.png](/images/challenges/HTB_SpookyPass/image%201.png)

Oh and I have the password with this command `strings pass | less`
which gives the readable text to us in the file.

![image.png](/images/challenges/HTB_SpookyPass/image%202.png)

![image.png](/images/challenges/HTB_SpookyPass/image%203.png)

![image.png](/images/challenges/HTB_SpookyPass/image%204.png)