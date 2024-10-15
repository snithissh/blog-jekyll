---
layout: post
title:  "Method-based access control can be circumvented"
date:   2024-10-15 12:20:54 +0530
---

## Introduction

This lab implements access controls based partly on the HTTP method of requests. You can familiarize yourself with the admin panel by logging in using the credentials administrator:admin.

To solve the lab, log in using the credentials wiener:peter and exploit the flawed access controls to promote yourself to become an administrator. 

## Solution

As provided in the lab description, I logged into the admin account and found that there is functionality to upgrade or downgrade the user and it calls the `POST` request to `/admin-roles` endpoint

![]({{ site.baseurl }}/assets/images/access-control/access-21.png)

Logged out of the admin credentials which we loggedin earlier and signed in as a wiener using the following credentials `wiener:peter`

Accessed the same endpoint `/admin-roles` and It says `"Missing parameter 'username'"`

![]({{ site.baseurl }}/assets/images/access-control/access-22.png)

Intercepted the request in burpsuite and then added the `username` parameter with the value as `wiener` which actually redirects to the `/admin` and then shows `401` error 

![]({{ site.baseurl }}/assets/images/access-control/access-23.png)

Again loggedin as a admin to observe how the upgrade and downgrade of the user role works and for example, upgraded the user called `carlos` to admin and found it sents a `POST` request and along with that in the Post body we have the username and one another parameter called `action` where the value sets as `upgrade`

![]({{ site.baseurl }}/assets/images/access-control/access-24.png)

Going back to the repeater, where had our `wiener` session and in the `GET` request, added a parameter and value as `&action=upgrade` and In the response, it says a `401` 

![]({{ site.baseurl }}/assets/images/access-control/access-25.png)

The above method didn't workout and again gone back to admin panel 

In the Upgrade and Downgrade the particular user by changing the request method from `POST` -> `GET` , I gave the username as `carlos` with the session cookie of user `wiener` and results in `302` that's a good one 

![]({{ site.baseurl }}/assets/images/access-control/access-26.png)

Now change the username from `carlos` -> `wiener` and the lab will be solved due to the fact the access control is implemented only for the `POST` request and for `GET` method, access control is not implemented and we were able to bypass with that 

![]({{ site.baseurl }}/assets/images/access-control/access-27.png)