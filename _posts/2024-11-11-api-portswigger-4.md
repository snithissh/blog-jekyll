---
layout: post
title:  "Exploiting a mass assignment vulnerability"
date:   2024-11-11 10:20:54 +0530
categories: [BSCP, API Vulnerabilities]
---

## Objective 

To solve the lab, find and exploit a mass assignment vulnerability to buy a Lightweight l33t Leather Jacket. You can log in to your own account using the following credentials: wiener:peter. 

## Solution 

Once after logging in with credentials provided from the Lab Objective, we were initiated a kind of "how we add a product and checkout". I initiated from a product selecting a product to checking out the product, intercepting every request. We found the 2 requests which are somewhat interesting actually:
- GET /api/checkout
- POST /api/checkout 

This is how the GET request looks like when you give a `GET` request to the `/api/checkout`. This is how the response looks like. 

![]({{ site.baseurl }}/assets/images/api/api-12.png)

Same with the post request, the request kind of looks similar. If you observe the post body's request (i.e. the post body request) and the get request kind of look similar in some way. But one thing is different - when you initiate the post request, since we don't have any sufficient credits, we receive an "insufficient credits" error. 

![]({{ site.baseurl }}/assets/images/api/api-13.png)

Now we can take the `GET` request's response and add it as the Post Request body. However, when we send the request, it looks at the insufficient funds. 

![]({{ site.baseurl }}/assets/images/api/api-14.png)

What we can do is we can just update the discount percentage from 0 to 100 and let's check the response whether it fits some kind of different or not. 

Once after initiating the request, we found that there is an order confirmation location and the request passes and the discount is applied to 100%, and it is 201 created, the location is somewhat different from the previous response.  

![]({{ site.baseurl }}/assets/images/api/api-15.png)

Now we were able to purchase the product at a 100% discount, and that solves the lab. 

![]({{ site.baseurl }}/assets/images/api/api-16.png)