---
layout: post
title:  "Basic server-side template injection (code context)"
date:   2024-12-03 10:20:54 +0530
categories: [BSCP, SSTI]
---

## Objective 

This lab is vulnerable to server-side template injection due to the way it unsafely uses a Tornado template. To solve the lab, review the Tornado documentation to discover how to execute arbitrary code, then delete the morale.txt file from Carlos's home directory.

You can log in to your own account using the following credentials: `wiener:peter` 

## Solution 

Once after login into `wiener` account where now we have a display my name in format like `Wiener Peter` or `Peter wiener` or Some random nicknames for public visibility 

![]({{ site.baseurl }}/assets/images/ssti/ssti-4.png)

Let's say if I leave a comment like `test` in a blog post to check what username is being displayed after selecting it as `Name` and well it says `Peter wiener`

![]({{ site.baseurl }}/assets/images/ssti/ssti-5.png)

When we select it as `Nickname` it will say some random username like `HotD0g`

![]({{ site.baseurl }}/assets/images/ssti/ssti-6.png)

Now we can intercept the request in burp and this is how the request looks like whenever you change the preferred name 

![]({{ site.baseurl }}/assets/images/ssti/ssti-7.png)

Noow through `blog-post-author-display` parameter we can pass the following payload like `user.first_name}}{{7*7}}` which will actually display first name and then closes the function.. later it will executed value which is `49` 

![]({{ site.baseurl }}/assets/images/ssti/ssti-7.png)

And that confirms, that's an SSTI right 

Ok, in order to solve the lab.. we need to delete a file which is `/home/carlos/morale.txt` and we can construct a payload like 

```liquid
{% raw %}{% import os %}{{ os.system('rm /home/carlos/morale.txt') }}{% endraw %}
```

which will use os library and executes the system command here to delete the file and send the request..

Refresh the blog post and that solves the lab 

![]({{ site.baseurl }}/assets/images/ssti/ssti-8.png)