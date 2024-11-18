---
layout: post
title:  "Blind SSRF with out-of-band detection"
date:   2024-10-16 11:20:54 +0530
categories: [BSCP, SSRF]
---

## Introduction 

This site uses analytics software which fetches the URL specified in the Referer header when a product page is loaded.

To solve the lab, use this functionality to cause an HTTP request to the public Burp Collaborator server. 

## Solution 

Once after spinning the lab, you can go, visit any products available on this website and intercept the request 

![]({{ site.baseurl }}/assets/images/ssrf-24.png)

Replace the actual value in `Referrer:` header with the collaborator URL and send the request 

Well, the request passes and responds with a status code of 200 

![]({{ site.baseurl }}/assets/images/ssrf-25.png)

Looking into the collaborator, we have received HTTP pingback which is our endgoal to make it 

![]({{ site.baseurl }}/assets/images/ssrf-26.png)


Once we got HTTP pingback and our lab is solved as well 

![]({{ site.baseurl }}/assets/images/ssrf-27.png)