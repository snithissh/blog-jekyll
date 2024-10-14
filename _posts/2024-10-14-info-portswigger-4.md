---
layout: post
title:  "Information disclosure on debug page"
date:   2024-10-14 12:20:54 +0530
---

## Objective

  

This lab contains a debug page that discloses sensitive information about the application. To solve the lab, obtain and submit the `SECRET_KEY` environment variable.  

  

## Solution

  

After spinning the lab instance, you can view-source of the page and come really really bottom of the page where you will find the following `<!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->`⁠

  

![]({{ site.baseurl }}/assets/images/image%2012.png)  

  

Phpinfo pages some discloses huge amount informations like session cookies, environment variables and alot more 

  

Opening the path which is `/cgi-bin/phpinfo.php` discloses the PHPinfo pages and now our endgoal is to find `SECRET_KEY` right which is also available under Environment variable section 

  

![]({{ site.baseurl }}/assets/images/image%2013.png)  

  

Copy and pasted the value of `SECRET_KEY`  into the solution and lab is solved 

  

![]({{ site.baseurl }}/assets/images/image%2014.png)