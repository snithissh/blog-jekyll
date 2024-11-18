---
layout: post
title:  "SQL injection UNION attack, retrieving data from other tables"
date:   2024-09-07 21:39:54 +0530
categories: [BSCP, SQLi]
---


# Objective 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data.

## Solution 

In the product category filter, added the following `'` in the url and recieved a internal server error 

![]({{ site.baseurl }}/assets/images/sqli6.png)

To check whether with only one column exists with the following payload `'UNION+SELECT+NULL--` but still an internal server error 

![]({{ site.baseurl }}/assets/images/sqli7.png)

Adding another `NULL` to check whether only two column but still we have internal server error meanig there is more than one

![]({{ site.baseurl }}/assets/images/sqli8.png)

Adding third `NULL` and we get to know that there is only 3 columns exists in the database with the following payload `'UNION+SELECT+NULL,NULL,NULL--` responded with `200` status code 

![]({{ site.baseurl }}/assets/images/sqli9.png)

But still it isn't completed yet, we need to find the following string `'f17Ct1'` in any of the three columns by replacing the `NULL` values 

When replacing the first NULL value with `'f17Ct1'` recieves a internal server error meaning that this particular string doesn't exists 

![]({{ site.baseurl }}/assets/images/sqli10.png)

In replacing the second `NULL` value with the string value `'f17Ct1'` recieves a `200` status code meaning that string matches in the second column of the database 

![]({{ site.baseurl }}/assets/images/sqli11.png)

String is found and the lab is solved 

![]({{ site.baseurl }}/assets/images/sqli12.png)