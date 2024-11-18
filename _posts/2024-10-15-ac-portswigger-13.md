---
layout: post
title:  "User role controlled by request parameter"
date:   2024-10-15 12:20:54 +0530
categories: [BSCP, Access Control]
---

## Introduction

This lab has an admin panel at /admin, which identifies administrators using a forgeable cookie.

Solve the lab by accessing the admin panel and using it to delete the user carlos.

You can log in to your own account using the following credentials: wiener:peter

## Solution

As mentioned in the lab description, we were able to login as wiener with the following credentials which is `wiener:peter`

![]({{ site.baseurl }}/assets/images/access-control/access-8.png)

Acccessing the `/admin` path shows that Only admin can access this interface which means we need to find a way to escalate our privilege to admin 

![]({{ site.baseurl }}/assets/images/access-control/access-9.png)

But the checking `Cookie:` Header shows that there is an additional attribute exists inside a cookie which is `Admin` and the value is set to `false`

![]({{ site.baseurl }}/assets/images/access-control/access-10.png)

Changing the `Admin` from `false` to `true` shows a new tab called `Admin panel` which didn't exists previously which means our privilege got escalated 

![]({{ site.baseurl }}/assets/images/access-control/access-11.png)

Access the admin panel tab, delete the user called `carlos` and lab will be solved 

![]({{ site.baseurl }}/assets/images/access-control/access-12.png)