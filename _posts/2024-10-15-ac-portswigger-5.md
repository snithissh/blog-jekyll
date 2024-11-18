---
layout: post
title:  "Unprotected admin functionality with unpredictable URL"
date:   2024-10-15 12:20:54 +0530
categories: [BSCP, Access Control]
---

## Introduction 

This lab has an unprotected admin panel. It's located at an unpredictable location, but the location is disclosed somewhere in the application.

Solve the lab by accessing the admin panel, and using it to delete the user carlos. 

## Solution

Just looks like the previous lab, but accessing the same admin panel under `/administrator-panel` shows a `404` here means it is `Not Found`

![]({{ site.baseurl }}/assets/images/access-control/access-4.png)

Looking into the page source, we found a different endpoint called `/admin-8dkh40` inside a `isAdmin` javascript function 

![]({{ site.baseurl }}/assets/images/access-control/access-5.png)

Now accessing the following path `/admin-8dkh40` shows the complete admin panel 

![]({{ site.baseurl }}/assets/images/access-control/access-6.png)

Deleting the user called `carlos` will solve the lab 

![]({{ site.baseurl }}/assets/images/access-control/access-7.png)