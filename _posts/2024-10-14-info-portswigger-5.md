---
layout: post
title:  "Source code disclosure via backup files"
date:   2024-10-14 12:20:54 +0530
---

## Objective

  

This lab leaks its source code via backup files in a hidden directory. To solve the lab, identify and submit the database password, which is hard-coded in the leaked source code.

  

## Solution

  

Looking into page source doesn’t reveal anything but checking `/robots.txt` revealed that it disallows `/backup` folder

  

![]({{ site.baseurl }}/assets/images/image%208.png)  

  

Accessing the `/backup` folder shows there is file exists in index of directory called `ProductTemplate.java.bak`⁠ 

  

![]({{ site.baseurl }}/assets/images/image%209.png)  

  

  

Clicking the file and reveals full source code

  

![]({{ site.baseurl }}/assets/images/image%2010.png)  

  

Just copy and paste the database password as a solution and submit to solve the lab 

  

![]({{ site.baseurl }}/assets/images/image%2011.png)