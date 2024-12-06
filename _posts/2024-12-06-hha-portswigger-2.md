---
layout: post
title:  "Routing-based SSRF"
date:   2024-12-06 10:20:54 +0530
categories: [BSCP, HTTP Host Header Attacks]
---

## Objective 

This lab is vulnerable to routing-based SSRF via the Host header. You can exploit this to access an insecure intranet admin panel located on an internal IP address.

To solve the lab, access the internal admin panel located in the `192.168.0.0/24` range, then delete the user `carlos`. 

## Solution 

Through the `Host` header, I just added an internal IP like `192.168.0.1` and observed an `504 Gateway error` meaning that there is no such IP active on this range and these are called checks based on `Virtual Host` or `VHost`

![]({{ site.baseurl }}/assets/images/hha/hha-6.png)

Our goal is to find an internal IP which is active in the range of `192.168.0.1 - 255` and for that we can use burp intruder, where we can set the placeholder for `192.168.0.$1$` and payload values from `1 to 255` that's the range we are going to check and mainly you need to uncheck `Update Host header to target` and also set the resource pool to `1` request per second 

![]({{ site.baseurl }}/assets/images/hha/hha-7.png)

While intruder is still running and on exactly `77th` request, we got a hit that there is different status code of `302` 

![]({{ site.baseurl }}/assets/images/hha/hha-8.png)

We got the admin panel here on `/admin` through virtual host 

![]({{ site.baseurl }}/assets/images/hha/hha-9.png)

Now in order to solve the lab, we need to delete the user called `carlos` and for that, we can initiate a `GET` request to `/admin/delete?username=carlos` and sending the request.. triggers a `csrf error` and we can add the parameter and value from the source page.. and then send request.. which leads to deletion of `carlos` user and that solves the lab 

![]({{ site.baseurl }}/assets/images/hha/hha-10.png)