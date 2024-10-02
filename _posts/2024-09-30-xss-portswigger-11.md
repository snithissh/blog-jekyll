---
layout: post
title:  "Reflected XSS into a JavaScript string with single quote and backslash escaped"
date:   2024-09-30 09:39:54 +0530
---

  

## Intro

This lab contains a [reflected cross-site scripting](https://portswigger.net/web-security/cross-site-scripting/reflected) vulnerability in the search query tracking functionality. The reflection occurs inside a JavaScript string with single quotes and backslashes escaped.

  

To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the `alert` function

  

## Solution

In the search functionality as usual searched for the sample payload `nithissh”><h2>XSS` and without execution of `<h2>` it is getting reflected inside a javascript code 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408123317.png)  

  

What I did was, Again in the search functionality searched for `</script>nithissh` why we did this because we our input value getting reflected inside `<script></script>` and while passing the value, we have closed our `</script>` and what ever after the closed tag, will execute in a newline 

  

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408123332.png)  

  

  

Now entering the following payload `</script><img src=x onerror=alert(1)>` and lab will be solved 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408123346.png) 