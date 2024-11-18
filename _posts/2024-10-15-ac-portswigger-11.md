---
layout: post
title:  "User ID controlled by request parameter"
date:   2024-10-15 12:20:54 +0530
categories: [BSCP, Access Control]
---

## Introduction

This lab has a horizontal privilege escalation vulnerability on the user account page.
To solve the lab, obtain the API key for the user carlos and submit it as the solution.
You can log in to your own account using the following credentials: wiener:peter 

## Solution

With the following credentials `wiener:peter`, we loggedin as a user called `wiener` and once after login, we can see our own API key 

![]({{ site.baseurl }}/assets/images/access-control/access-28.png)

If you observe the URL, where have a `id` pointing out to loggedin user which is `wiener` and changing the value of `id` parameter from `wiener` to `carlos`

Shows the complete API key of the `carlos` and this type of vulnerability is called `IDOR`

![]({{ site.baseurl }}/assets/images/access-control/access-29.png)

Submit the API key as solution and lab will be solved 

![]({{ site.baseurl }}/assets/images/access-control/access-30.png)