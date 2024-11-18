---
layout: post
title:  "Reflected XSS with Some SVG markup Allowed"
date:   2024-09-30 09:39:54 +0530
categories: [BSCP, XSS]
---

## Intro

In order to solve this lab has a simple reflected XSS vulnerability. The site is blocking common tags but misses some SVG tags and events.



## Solution

In the application, we have a search functionality where we are able to search for something and searched for the following payload `test”><h2>xss` and unfortunately the tag is blocked 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408122827.png)  

  

Now we know that, `<h2>` tag is blocked but we already learnt that only `<svg>` tags and related events is allowed.. With that in mind, we can use the following payload `<svg onload=alert(1)>` in the search functionality and now the tags are bypassed but now we have recieved error related to events called `Event is not allowed` 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408122844.png)  

  

  

Intercepted the traffic in burp → Sent to the intruder and payload placeholder looks like as follows 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408122858.png)  

  

  

Used all the tags from portswigger cheatsheet and then started the bruteforcing, found that `<animatetransform>` is allowed 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408122917.png)  

  

With the following payload `"><svg><animatetransform onbegin=alert(1) attributeName=transform>` we were able to solve this the lab and how the found his manually looking into the payload associated with `svg → animatetransform` tag 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408122939.png)