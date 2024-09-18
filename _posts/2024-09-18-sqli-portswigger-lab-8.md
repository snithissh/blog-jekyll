---
layout: post
title:  "SQL injection attack, listing the database contents on Oracle"
date:   2024-09-18 09:39:54 +0530
---

## Objective 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user. 

## Solution

Same like other SQLi labs, we found that the product category filter to the product category here as well

Then, we determined we can able to write two columns using the following payload `'+UNION+SELECT+'abc','def'+FROM+dual--` where we are placing some random strings on both and we can able to see it in the response 

> Here we have mentioned dual right ? what does it mean ? DUAL is a table automatically created by Oracle Database along with the data dictionary

![]({{ site.baseurl }}/assets/images/sqli38.png)

Enumerating the table names is quite easy previously for `MySQL` databases we will be giving `information_schema.tables` and here we will use `all_tables` instead off that.. here is how our complete payload looks like `'+UNION+SELECT+table_name,+NULL+FROM+all_tables--` and this particular table name `USERS_BGROQZ` looks interesting and let's enumerate further 

![]({{ site.baseurl }}/assets/images/sqli39.png)

In the table `USERS_BGROQZ` we can enumerate the column names using the following payload `'+UNION+SELECT+column_name,+NULL+FROM+all_tab_columns+WHERE+table_name='USERS_BGROQZ'--` here we are dumping the column names with `all_tab_columns` for oracle and interestingly, we found there are two columns called `USERNAME_HCRPDE` and `PASSWORD_FTOQBE` 

![]({{ site.baseurl }}/assets/images/sqli40.png)

Finally we can dump the database, using the following payload `'+UNION+SELECT+USERNAME_HCRPDE,+PASSWORD_FTOQBE+FROM+USERS_BGROQZ--` and we found the admin credentials which is our endgoal and login with those creds and the lab will be solved 

![]({{ site.baseurl }}/assets/images/sqli41.png)