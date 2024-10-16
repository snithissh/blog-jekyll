---
layout: post
title:  "Basic SSRF against the local server"
date:   2024-10-16 11:20:54 +0530
---

## Introduction 

This lab has a stock check feature which fetches data from an internal system.

To solve the lab, change the stock check URL to access the admin interface at http://localhost/admin and delete the user carlos. 

## Solution

Once after spinning the lab, we are presented with the following website where it kinda looks like an ecommerce website 

![]({{ site.baseurl }}/assets/images/ssrf-1.png)

As mentioned in the intro, where each products will have a specific functionality to check the stocks and you can view that by clicking on the products and come all the way you will see it 

![]({{ site.baseurl }}/assets/images/ssrf-2.png)

These **Check Stock** functionality where we can check for how many stock units available in each location 

Also, Intercepting the request using **burpsuite** and found that it is calling an external website to check for **Stock Units** through **POST** request via **stockApi** parameter 

![]({{ site.baseurl }}/assets/images/ssrf-3.png)

Sent the Intercepted request to repeater and let's play around with that like in `stockApi` parameter will replace the value like a localhost for an instance and followed by `/admin` which they have mentioned it as lab objective.. Sending the request resulted in status code of **200** and then rendering the page shows that we gotta access to admin panel 

![]({{ site.baseurl }}/assets/images/ssrf-4.png)

In the same response, there are two different calls to delete an user under `/admin` with a parameter called `username`

![]({{ site.baseurl }}/assets/images/ssrf-5.png)


In order to delete the user called `carlos` we have to send the following the url `https://127.0.0.1/admin/delete?username=carlos` through `stockApi` parameter and sent the request which responded with **302** 

![]({{ site.baseurl }}/assets/images/ssrf-6.png)


Now open our lab and lab is solved which means the user named `carlos` deleted successfully and hence we succeeded our objective 


![]({{ site.baseurl }}/assets/images/ssrf-7.png)