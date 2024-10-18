---
layout: post
title:  "Password reset broken logic"
date:   2024-10-18 10:20:54 +0530
---

## Objective 

This lab's password reset functionality is vulnerable. To solve the lab, reset Carlos's password then log in and access his "My account" page.

- Your credentials: `wiener:peter`
- Victim's username: `carlos`

## Solution 

Firstly, I logged in as a wiener (it's a user) With the credentials from the lab objective. 

![]({{ site.baseurl }}/assets/images/auth/auth-23.png)

In order to understand how the forgot password works, what we can do is, we can open a new window (it may be incognito) and I use the Container extension that Firefox uses, or you can just download it from the Firefox extension store, and then I just gave a forgot password for the user called `wiener`. 

![]({{ site.baseurl }}/assets/images/auth/auth-24.png)

Once after entering the new password, the password interceptor intercepted the request and passed it to the repeater. This is how the request and response look like. If the password reset is successful, there will be a 302 redirect.  

![]({{ site.baseurl }}/assets/images/auth/auth-25.png)

What we did is in the POST request, there is a certain parameter (like a forgot password token) that I removed from the POST URL parameter and as well as in the POST parameter and sent the request. The request is also passed - actually, it doesn't matter if the token value isn't being validated properly, so without even the token value, the request still works (because the temp token is not being used) but when you remove the complete value (i.e. the temp token with the parameter and the value), the request won't work 

To complete this practitioner lab much easier, I don't want to drag it much further. So I just updated the username to Carlos and sent the request. It passed. 

![]({{ site.baseurl }}/assets/images/auth/auth-26.png)

Since we updated the password of Carlos user, now in order to solve the lab we can login with the username as `carlos` and password as `peter` which will solve the lab. 

![]({{ site.baseurl }}/assets/images/auth/auth-27.png)