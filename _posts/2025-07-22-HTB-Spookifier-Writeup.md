---
title: 'HTB Spookifier Writeup (challenge)'
date: 2025-07-22
permalink: /posts/2025/07/HTB_Spookifier_Writeup/
tags:
  - HTB
  - writeup
  - very-easy
  - challenge
---

A writeup of the Hack The Box challenge "Spookifier" with very easy difficulty

# Spookifier (very easy)

Challenge discription: There's a new trend of an application that generates a spooky name for you. Users of that application later discovered that their real names were also magically changed, causing havoc in their life. Could you help bring down this application?

There is a web application and a zip file `spookifier.zip`.

![image.png](/images/challenges/HTB_Spookifier/image.png)

Inside this zip file are some files

![image.png](/images/challenges/HTB_Spookifier/image%201.png)

The `flag.txt` is a fake flag: `HTB{f4k3_fl4g_f0r_t3st1ng}`

The rest gives away that this is a application run with docker.

The app converts a Halloween name from default text to a spookier font.

We see in the `Dockerfile` that the server is exposed on port 1337.

The code also exposes that the app runs a vulnerable flask version, but more interesting: the URL expects the parameter `text`  .

The input text is displayed is a mako-template without formatting. The app uses Mako’s : `${...}` syntax to display the execution. This creates a SSTI-vulnerability. To test this:

`http://<ip>:<port>/?text=${7*7}`

If you see 49, then it works.

This URL should get us the flag:

`http://localhost:1337/?text=${**import**('os').popen('cat /flag.txt').read()}` 

We know that there is a flag.txt.

This is the result:

![image.png](/images/challenges/HTB_Spookifier/image%202.png)

![image.png](/images/challenges/HTB_Spookifier/image%203.png)