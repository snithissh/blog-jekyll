---
layout: post
title:  "Authentication bypass via information disclosure"
date:   2024-10-14 12:20:54 +0530
categories: [BSCP, Information disclosure]
---

## Objective 

This lab's administration interface has an authentication bypass vulnerability, but it is impractical to exploit without knowledge of a custom HTTP header used by the front-end.

To solve the lab, obtain the header name then use it to bypass the lab's authentication. Access the admin interface and delete the user `carlos`.

You can log in to your own account using the following credentials: `wiener:peter` 

  

## Solution

  

With the credentials I’ve which is `wiener:peter` and after login, I’ve noticed the URL path `/my-account?id=wiener` and changed it to admin redirected to the login page 

  

![]({{ site.baseurl }}/assets/images/image%203.png)  

  

Accessing the `/admin` shows that it is only accessible to the users available locally 

  

![]({{ site.baseurl }}/assets/images/image%204.png)  

  

When changing it from GET to TRACE reveals a new header `X-Custom-IP-Authorization` which will help us in bypassing IP based Authorization since we have this header 

  

![]({{ site.baseurl }}/assets/images/image%205.png)  

  

Now change the request to GET from TRACE and replace the new header called `X-Custom-IP-Authorization` with value as `127.0.0.1` why we are replacing the address as local address where the previous header told right **“Admin is accessible only the users”** and sent the request and responds with `200` means we have access to the admin panel now 

  

![]({{ site.baseurl }}/assets/images/image%206.png)  

  

Opening the response in browser and delete the user carlos to solve the lab 

  

![]({{ site.baseurl }}/assets/images/image%207.png)