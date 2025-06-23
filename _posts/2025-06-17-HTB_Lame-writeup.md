---
title: 'HTB Lame Writeup'
date: 2025-06-17
permalink: /posts/2025/06/HTB_Lame_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Lame" with easy difficulty


HTB_Lame
======

Hi and welcome to this writeup of the easy HTB machine Lame. 

`Nmap scan: nmap -sV -sC 10.10.10.3`

VSFTPd is version 2.3.4 which has a backdoor vulnerability.

Samba has a CVE-2007-2447 vulnerability because the version is 3.0.20

Using Metasploit I was able to exploit the machine and become root instantly.

The `user.txt` is in the `/home/<user>` dir and the root flag in the `/root` dir.

Easy Peasy :)