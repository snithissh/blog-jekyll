---
layout: post
title:  "Reflected XSS into a JavaScript string with Angle Brackets and HTML encoded"
date:   2024-09-30 09:39:54 +0530
categories: [BSCP, XSS]
---

  

## Intro

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets are encoded. The reflection occurs inside a JavaScript string. To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the alert function.



## Solution

There is search functionality in our lab, where if we search for anything and values will get reflected inside variable called `searchTerms` in javascript

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408115843.png)  

  

With the following payload `'-alert(1)-'` we can actually escape from the **var** value and bypass the need of `>` and `<` And as a result, there will be XSS popup and lab will be solved

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408115835.png)