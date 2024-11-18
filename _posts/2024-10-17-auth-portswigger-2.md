---
layout: post
title:  "2FA Bypass"
date:   2024-10-17 10:20:54 +0530
categories: [BSCP, Authentication]
---

## Introduction

This lab's two-factor authentication can be bypassed. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, access Carlos's account page.

- `Your credentials:` wiener:peter
- `Victim Credentials:` carlos:montoya

# Solution 

They have already provided with the credentials right which is wiener and peter as both the username and password and Once after login we need to enter 2FA code and same applies with the carlos account but we need to bypass it 

![]({{ site.baseurl }}/assets/images/auth/auth-5.png)

Checking the email client of our own account, shows that 2FA code of only four digits 

![]({{ site.baseurl }}/assets/images/auth/auth-6.png)

Awesome, we get to know some sort of information about the 2FA code right and now what we can do his login into the `carlos` account and entered some wrong 2FA or even random.. It redirects me back to the login with an error says `Incorrect security code`

![]({{ site.baseurl }}/assets/images/auth/auth-7.png)

Actually i got into rabbit hole here by started bruteforcing the pins actually but here it didn't workout and Now for the reason, We need to understand what happens after login into wiener account, entering the 2FA code it goes into account page with `/my-account` page 

![]({{ site.baseurl }}/assets/images/auth/auth-8.png)

Now we logged into `carlos` account and now when you are asked for `2FA code` and change the path from `/login2` to `/my-account` gets 2FA bypassed and lab is solved 

![]({{ site.baseurl }}/assets/images/auth/auth-9.png)