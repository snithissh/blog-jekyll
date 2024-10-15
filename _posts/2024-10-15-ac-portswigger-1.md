---
layout: post
title:  "Insecure direct object references"
date:   2024-10-15 12:20:54 +0530
---

# Introduction

This lab stores user chat logs directly on the server's file system, and retrieves them using static URLs.

Solve the lab by finding the password for the user carlos, and logging into their account. 

## Solution

Once after login, we have a functionality to do a `live chat` and Clicking on `View transcript` it will actually download the transcript 

![]({{ site.baseurl }}/assets/images/access-control/access-46.png)

Now change it from `4.txt` to `1.txt` actually reveals other users transcript and observing the transcript carefully we have password of `carlos` 

![]({{ site.baseurl }}/assets/images/access-control/access-48.png)

Login in with the found password and solves the lab 

![]({{ site.baseurl }}/assets/images/access-control/access-49.png)