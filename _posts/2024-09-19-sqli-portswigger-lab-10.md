---
layout: post
title:  "SQL injection vulnerability allowing login bypass"
date:   2024-09-19 09:39:54 +0530
---

## Objective 

This lab contains a SQL injection vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the administrator user. 

## Solution

In the login functionality, enter the username as `administrator'` and password as some random words lets say `nithi0070800` which resulted in internal server error 

![]({{ site.baseurl }}/assets/images/sqli45.png)

Ok Now, In the username field enter the following payload like `administrator' OR 1=1--` just like the previous lab where it will break out of the SQL query and checks for the true statement and it will ignore `password` but you can enter some random string.. if the statement is true we will get logged in as administrator 

Once the request is sent, we have logged in as an `administrator` here as expect and lab is solved

![]({{ site.baseurl }}/assets/images/sqli46.png)