---
layout: post
title:  "SameSite Lax bypass via method override"
date:   2024-10-03 09:39:54 +0530
categories: [BSCP, CSRF]
---

## Objective 

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: `wiener:peter` 

## Solution

After logging with the provided credentials in the lab objective, we have functionality to update the email and this is how the intercepted request looks like and once after updating it responds with a `302`

![]({{ site.baseurl }}/assets/images/csrf-17.png)

**Some Important points to remember:**

- Samesite lax cookie can be a cross-site request and if these conditions are met 
   - Request should be a `GET` method 
   - Results are from a kind of top level navigation.. Kind of like going and click a download button 
- If you have a `POST` request for a crossite requests, Cookies won't be passed 

Ok keeping that in mind.. What I did was.. Just changed the request from `POST` to `GET` and sent the request, Resulted in `405 method not allowed` meaning this method is eithier blocked or not allowed 

![]({{ site.baseurl }}/assets/images/csrf-19.png)

We can use method override by utilising `_method` parameter by adding `&_method=POST` where it will override the `GET` method and perform a `POST` method operation 

![]({{ site.baseurl }}/assets/images/csrf-20.png)

Awesome we bypassed it, Let's build a CSRF POC

```html
<script>
    document.location = "https://0a1d002604874436807bc12b001000a6.web-security-academy.net/my-account/change-email?email=attackeryo%40gmail.com&_method=POST"
</script>
```

Paste the CSRF POC onto the exploit server.. Then hit store and deliver the exploit to victim should solve the lab but portswigger didn't mark it solved

