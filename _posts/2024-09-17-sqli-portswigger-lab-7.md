---
layout: post
title:  "SQL injection attack, listing the database contents on non-Oracle databases"
date:   2024-09-17 21:39:54 +0530
---

## Objective 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user. 

## Solution 

Same like the other labs we saw in SQL injection, The product category filter is vulnerable to Possible SQL injection attack

![]({{ site.baseurl }}/assets/images/sqli31.png)

There are only two columns like the previous lab 

![]({{ site.baseurl }}/assets/images/sqli32.png)

We were also able to write to both the columns as shown in the image where in the first column we replaced it `NULL` with `abc` and second column `NULL` with `def`

![]({{ site.baseurl }}/assets/images/sqli33.png)

With the following payload `'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--` we were able the extract the table names from `information.schema` database through the first column and the second column we set it to `NULL` 

![]({{ site.baseurl }}/assets/images/sqli34.png)

In order to extract the column names from the table `users_mvtqls` we can use the following payload `'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_mvtqls'--` which will actually tells us the column names which is `username_nfnlsh` and `password_vfdfze`

![]({{ site.baseurl }}/assets/images/sqli35.png)

So, Let's extract the username and password from a particular table using the following payload `'+UNION+SELECT+username_nfnlsh,password_vfdfze+FROM+users_mvtqls--`

As you see in the response, we were able to dump the credentials from the table 

![]({{ site.baseurl }}/assets/images/sqli36.png)

Logged in as admin and lab is solved 

![]({{ site.baseurl }}/assets/images/sqli37.png)