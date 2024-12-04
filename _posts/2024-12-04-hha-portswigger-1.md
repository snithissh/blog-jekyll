---
layout: post
title:  "Web cache poisoning via ambiguous requests"
date:   2024-12-04 10:20:54 +0530
categories: [BSCP, HTTP Host Header Attacks]
---

## Objective 

This lab is vulnerable to web cache poisoning due to discrepancies in how the cache and the back-end application handle ambiguous requests. An unsuspecting user regularly visits the site's home page.

To solve the lab, poison the cache so the home page executes `alert(document.cookie)` in the victim's browser. 

## Solution 

Well, in the home page of our lab.. we have a javascript file called `tracking.js` being loaded on the same site and what happens if we poison that `tracking.js` file and which helps us in executing the xss right 

![]({{ site.baseurl }}/assets/images/hha/hha-1.png)

When we add a cachebusters like `?a=2` and observing the response.. well it says, `X-Cache: Miss`

![]({{ site.baseurl }}/assets/images/hha/hha-2.png)

Sending the request and In the response, it changed from `X-Cache: Miss` to `X-Cache: HIT`

![]({{ site.baseurl }}/assets/images/hha/hha-3.png)

Now let's update the cachebusters from `?a=2` to something new let's say `?a=5` and update the additional host header like kinda duplication one but value with our exploit server like `Host: exploit-0a9300eb0415490a8054cac201990032.exploit-server.net`

Send the request twice and now you can observe that the tracking script which loads was changed from the application server to our exploit server which kinda be poisoned 

![]({{ site.baseurl }}/assets/images/hha/hha-4.png)

Once we refresh the page, we can see that the alert is triggered which we put inside the exploit server

![]({{ site.baseurl }}/assets/images/hha/hha-5.png)

That solves the lab actually but the portswigger lab isn't changing to be solved state