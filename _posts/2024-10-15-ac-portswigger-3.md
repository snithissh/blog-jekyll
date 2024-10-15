---
layout: post
title:  "Multi-step process with no access control on one step"
date:   2024-10-15 12:20:54 +0530
---

## Introduction 

This lab has an admin panel with a flawed multi-step process for changing a user's role. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.

To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator. 

## Solution 

First, we will login into our lower level account using the following credentials `wiener:peter` where you get loggedin as a `wiener` and copy the cookie / session value for later use 

```bash
session:"OILOAH12g5LW6FQgf9diuxNBx23LaEbb"
```

Login out and login as admin with the following credentials `administrator:admin` and through the `Admin panel` let's upgrade or downgrade where we secondary verification we need to confirm on `yes` or `no`

![]({{ site.baseurl }}/assets/images/access-control/access-50.png)

Click on yes, Intercept the request, send it to repeater and Now manipulate with the wiener cookie we copied earlier replace it here and sent the request.. Later as a result it says access denied 

![]({{ site.baseurl }}/assets/images/access-control/access-51.png)

Did the same step, logged in into non admin user account which is `wiener` and copied the cookie again 

From the admin account, upgrade the user called `carlos` and In the confirmation page, intercept the request and replace it with the copied cookies and sent the request, it accepts the request and responds with `302` means it upgrades the user 

Replace the username with `wiener` which is our own user and send the request, responds with `302` which means a low level user can upgrade admin kind of privilege escalation 

![]({{ site.baseurl }}/assets/images/access-control/access-52.png)

Looking into lab UI since we have upgraded the `wiener` user to admin.. Lab is solved 

![]({{ site.baseurl }}/assets/images/access-control/access-53.png)