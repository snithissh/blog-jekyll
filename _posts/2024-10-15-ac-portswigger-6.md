---
layout: post
title:  "Unprotected admin functionality"
date:   2024-10-15 12:20:54 +0530
categories: [BSCP, Access Control]
---

## Introduction 

This lab has an unprotected admin panel and Solve the lab by deleting the user carlos. 

## Solution 

The lab looks like a same ecommerce website just like what we see in other labs as well 

![]({{ site.baseurl }}/assets/images/access-control/access-1.png)

Just like other challenges, I searched for any hint in source code and I didn't find anything else 

One thing I noticed that, we can do a CTF approach here, Where checking for `/robots.txt` where we might have a hint over there 

Checking the `/robots.txt` shows that it disallows the access to a particular subdirectory `/administrator-panel` 

```bash
curl "https://0a2100b404e1bef982fa6006000200ec.web-security-academy.net/robots.txt"

User-agent: *
Disallow: /administrator-panel
```

Accessing the path `/administrator-panel` shows that we got access to the admin panel and we do have access to delete the users

![]({{ site.baseurl }}/assets/images/access-control/access-2.png)

Delete the user called `carlos` and lab will be deleted

![]({{ site.baseurl }}/assets/images/access-control/access-3.png)