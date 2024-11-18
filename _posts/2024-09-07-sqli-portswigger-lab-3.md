---
layout: post
title:  "SQL injection UNION attack, determining the number of columns returned by the query"
date:   2024-09-07 21:39:54 +0530
categories: [BSCP, SQLi]
---

## Objective 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

## Solution 

After spinning up the lab, this is how the application looks like kind of ecommerce application 

![]({{ site.baseurl }}/assets/images/sqli1.png)

In the page which is shown, I've gone through a product category filter and in this case, I've clicked on the `Corporate Gifts` and added `'` at the end of the URL and recieved a internal server meaning our SQL query got broke and app may not able to process further 

![]({{ site.baseurl }}/assets/images/sqli2.png)

With the following payload `'UNION+SELECT+NULL--` will check if there is only one column exists but still we face `500` meaning there is more than 1

![]({{ site.baseurl }}/assets/images/sqli3.png)

Adding other `NULL` to check for two column but still `500`

![]({{ site.baseurl }}/assets/images/sqli4.png)

But when we add the third null value which responded with `200` status code, we can see that there are three columns exists with the following payload `'UNION+SELECT+NULL,NULL,NULL--` 

![]({{ site.baseurl }}/assets/images/sqli5.png)