---
layout: post
title:  "Referer-based access control"
date:   2024-10-15 12:20:54 +0530
categories: [BSCP, Access Control]
---

## Introduction

This lab controls access to certain admin functionality based on the Referer header. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.

To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator. 

## Solution

Once after logging with the admin, the upgrade and downgrade are handled via `GET` request and If you observe the `Referrer:` it tells us from where the request came from and in our case it is `/admin`

![]({{ site.baseurl }}/assets/images/access-control/access-54.png)

Before moving further, we will backwards and copy the session token of `wiener` who is low level user with no privilges and Paste the token into above existing request and we set the action to upgrade and username as carlos 

Response is 302 and carlos is upgraded from Normal user to Admin User with a normal user credentials due to some lack of access control here 

![]({{ site.baseurl }}/assets/images/access-control/access-55.png)

Now change the username to `wiener` and sent the request, responded with a `302` meaning the user gets upgraded from `NORMAL` to `ADMIN`

![]({{ site.baseurl }}/assets/images/access-control/access-56.png)

Opening and refreshing the page, lab is solved and you can `Admin panel` tab as well 

![]({{ site.baseurl }}/assets/images/access-control/access-57.png)
