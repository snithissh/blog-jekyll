---
layout: post
title:  "Detecting NoSQL injection"
date:   2024-11-27 10:20:54 +0530
categories: [BSCP, NoSQL Injection]
---

## Objective 

The product category filter for this lab is powered by a MongoDB NoSQL database. It is vulnerable to NoSQL injection.

To solve the lab, perform a NoSQL injection attack that causes the application to display unreleased products.

## Solution

Intercepted the product category filter on to burpsuite and this is how the request looks like 

![]({{ site.baseurl }}/assets/images/nosql/nosql-1.png)

Next up we can check with condition statements like false and true condition.. Now with the following payload `' && 0 && 'x` it will create false condition and no product will be displayed 

![]({{ site.baseurl }}/assets/images/nosql/nosql-2.png)

With a true condition like `' && 1 && 'x` and the products will be displayed 

![]({{ site.baseurl }}/assets/images/nosql/nosql-3.png)

Now in order to solve the lab, we need to disclose all the unreleased products from the server and with the following payload `'||'1'=='1` or we can also use `'%00` where nullbyte breaks the query and discloses the unreleased products as well

`'` which will actually break query to crash an error and then concatenate to check against the true or false statement here where `1 == 1` is true and that will actually disclose the unreleased products and lab will be solved 

![]({{ site.baseurl }}/assets/images/nosql/nosql-4.png)