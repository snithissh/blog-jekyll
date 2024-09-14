---
layout: post
title:  "SQL injection UNION attack, retrieving data from other tables"
date:   2024-09-08 21:39:54 +0530
---
 
## Objective 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user. 

## Solution

Added the `'` in the url parameter value for product category filter and recieved a `500` status code 

![]({{ site.baseurl }}/assets/images/sqli13.png)

There are two columns exists and recieved a `200` status code with the following payload `'UNION+SELECT+NULL,NULL--` 

![]({{ site.baseurl }}/assets/images/sqli14.png)

Since there are two columns exists and our endgoal is to exfil the usernames and passwords from users database 

With the following payload `'UNION+SELECT+username,+password+FROM+users--` we are retrieving the columns **username** and **password** from the **users** database and passed this payload into parameter and as a result, analysing the response we found that we have successfully dumped out all the available username and passwords inside users database 

![]({{ site.baseurl }}/assets/images/sqli15.png)

Now with those credentials, we will login as an administrator to solve the lab 

![]({{ site.baseurl }}/assets/images/sqli16.png)