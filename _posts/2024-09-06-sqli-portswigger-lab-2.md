---
layout: post
title:  "SQL injection attack, querying the database type and version on Oracle"
date:   2024-09-06 21:39:54 +0530
---


## Objective 

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string. 

## Solution 

As same like previous SQLi labs which we saw and this is also potentially vulnerable to SQL injection attack 

![]({{ site.baseurl }}/assets/images/sqli23.png)

Just like what we did in MySQL for determining the columns and I did the same here as well but it didn't workout and unfortunately we are missing something 

![]({{ site.baseurl }}/assets/images/sqli24.png)

Looking into the Portswigger cheatsheet, for oracle based database we need to add `+FROM+dual--` at the end because dual is special table that exists by default in all oracle databases 

Adding it and here is how the complete payload looks like `'+UNION+SELECT+'abc','def'+FROM+dual--` and responded with a status code of `200` 

![]({{ site.baseurl }}/assets/images/sqli25.png)

OK then what's next, our endgoal is to grab the banner version and we can able to do identify the version with the following payload `'+UNION+SELECT+BANNER,NULL+FROM+v$version--` from the following [cheatsheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) and sent the request with following server version `11.2.0.2.0 - Production`

![]({{ site.baseurl }}/assets/images/sqli26.png)