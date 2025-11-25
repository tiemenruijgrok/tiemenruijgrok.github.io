---
title: 'HTB Flag Command Writeup (challenge)'
date: 2025-07-21
permalink: /posts/2025/07/HTB_FlagCommand_Writeup/
tags:
  - HTB
  - writeup
  - very-easy
  - challenge
---

A writeup of the Hack The Box challenge "Flag Command" with very easy difficulty

# Flag Command (very easy)

It’s some kind of game in the browser.

![image.png](/images/challenges/HTB_FlagCommand/image.png)

We have to find a way out of the game. You are given multiple options at a time. You have to choose 1 to progress further in the game. 

But you can’t escape the game this way. 

The game is written in JavaScript looking at the source code of the web app. 

Somewhere in the code I see a line `playerWon();` which contains `HTB{`

This is the start of a flag.

I see also this in the `main.js`

![image.png](/images/challenges/HTB_FlagCommand/image%201.png)

When I type [http://94.237.54.192:52066/api/options](http://94.237.54.192:52066/api/options)

![image.png](/images/challenges/HTB_FlagCommand/image%202.png)

`Blip-blop, in a pickle with a hiccup! Shmiggity-shmack`

![image.png](/images/challenges/HTB_FlagCommand/image%203.png)

![image.png](/images/challenges/HTB_FlagCommand/image%204.png)