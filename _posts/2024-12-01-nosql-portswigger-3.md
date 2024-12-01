---
layout: post
title:  "Exploiting NoSQL injection to extract data"
date:   2024-12-01 10:20:54 +0530
categories: [BSCP, NoSQL Injection]
---

## Objective 

The user lookup functionality for this lab is powered by a MongoDB NoSQL database. It is vulnerable to NoSQL injection.

To solve the lab, extract the password for the administrator user, then log in to their account.

You can log in to your own account using the following credentials: `wiener:peter`. 

## Solution 

While you login, we found this particular request on user lookup... which is our main objective towards extracting data from it.. Here is how it works, whenever you gave `username` which is valid and if it is found inside the database it will list the info in `JSON` format 

Here in this case, we have valid user called `wiener` and shares information about it 

![]({{ site.baseurl }}/assets/images/nosql/nosql-8.png)

Thought of checking whether we have user called `administrator` and yeah it is there 

![]({{ site.baseurl }}/assets/images/nosql/nosql-9.png)

In order to confirm whether the SQL injection is working out, we can try like `' && '1' == '1` this payload through the user lookup 

Let's say, we have administrator user lookup request.. show us the valid response but we pass the following payload `' && '1' == '2` even though the administrator is valid user but it will say it is a invalid user because it's not about the user.. it passes through a true or false statement and `1 = 2` is `FALSE` statement doesn't disclose the data 

![]({{ site.baseurl }}/assets/images/nosql/nosql-10.png)

But you change the payload to `' && '1' == '1` which is actually `TRUE` statement and it will disclose the data 

![]({{ site.baseurl }}/assets/images/nosql/nosql-11.png)

We can also enumerate length of the password in order to move further.. like with the following payload `' && this.password.length < 30 || '1' == '2` where it will check if the password is `30` characters.. it will actually disclose the data meaning the length of the password is less than `30` 

![]({{ site.baseurl }}/assets/images/nosql/nosql-12.png)

Ok, `30` character is too much.. how about less than `10` characters.. 

Less than `10` characters.. Also works out 

![]({{ site.baseurl }}/assets/images/nosql/nosql-13.png)

I just kept reducing all the way from `10` to `1` and found out, exact length of the password is `9` characters 

![]({{ site.baseurl }}/assets/images/nosql/nosql-14.png)

Now with the following payload `' && this.password[0] == 'a' || 'a'=='b` it will check for the password characters one by one like let's we are taking like an array and matches with alphabetic word like `a` or `b` or `c` if not statement fails, because `'a' == 'b` is actually 

Now through burp intruder, we can bruteforce two value using cluster bomb attack 

![]({{ site.baseurl }}/assets/images/nosql/nosql-15.png)

After bruteforcing, It returned absolute `9` characters with different length then others 

![]({{ site.baseurl }}/assets/images/nosql/nosql-16.png)

The password is `xoikhdas` and login with the username as `administrator` and that solves the lab 

![]({{ site.baseurl }}/assets/images/nosql/nosql-17.png)