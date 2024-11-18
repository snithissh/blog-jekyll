---
layout: post
title:  "Basic SSRF against another back-end system"
date:   2024-10-16 11:20:54 +0530
categories: [BSCP, SSRF]
---

## Introduction

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, use the stock check functionality to scan the internal `192.168.0.X` range for an admin interface on port 8080, then use it to delete the user `carlos`. 

## Solution

Same like the previous lab, where we saw that there was external call to an external website to check for stocks but in this lab, to check stock, it is calling an internal IP address `192.168.0.1` looks like some sort of backend systems 

![]({{ site.baseurl }}/assets/images/ssrf-8.png)

Sent this to repeater and removed every subdirectories and replace the `stockApi` parameter value with `https://192.168.0.1:8080/admin` which obviously results in weird response which **"Missing parameter"** 

![]({{ site.baseurl }}/assets/images/ssrf-9.png)

Ok, what we can do his let's try the same payload which is `/admin/delete?username=carlos` which we used in the previous lab and curate the request like this and send it

```sh
POST /product/stock HTTP/2
Host: 0af0008b038860ca84294f0c007e00b2.web-security-academy.net
Cookie: session=eqDVJz7S79pd1lZu57GgbI5rfVPMyIoX
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:126.0) Gecko/20100101 Firefox/126.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0af0008b038860ca84294f0c007e00b2.web-security-academy.net/product?productId=1
Content-Type: application/x-www-form-urlencoded
Content-Length: 38
Origin: https://0af0008b038860ca84294f0c007e00b2.web-security-academy.net
Dnt: 1
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=1
Te: trailers

stockApi=http://192.168.0.1:8080/admin/delete?username=carlos
```

But still we have recieved the same weird response which is **"Missing parameter"** 

![]({{ site.baseurl }}/assets/images/ssrf-10.png)

What else, we can pass this request to intruder and will bruteforce the IP block from 0 to 100 and placeholder actually looks like and let's find out are there any IP being alive other than `192.168.0.1` on port `8080`

![]({{ site.baseurl }}/assets/images/ssrf-11.png)

And found to be nothing interesting IP available on port `8080` and what about will remove that as well and bruteforce it 

![]({{ site.baseurl }}/assets/images/ssrf-12.png)

Well, that's a wrong move will have to have o stick with the port 8080 but increase the bruteforce range from 1 to 100 to 255 

But the result is quite interesting because on bruteforce we found a new IP being available `192.168.0.207` which resulted in `200` status code 

![]({{ site.baseurl }}/assets/images/ssrf-13.png)

Now we found a new valid IP and now send the following value `http://192.168.0.207:8080/admin/delete?username=carlos` through `stockApi` parameter which will actually delete the carlos and the lab will be solved 

![]({{ site.baseurl }}/assets/images/ssrf-14.png)