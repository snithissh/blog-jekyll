---
layout: post
title:  "Reflected XSS into a template literal with angle brackets, single, double quotes"
date:   2024-09-30 09:39:54 +0530
---

## Intro

This lab contains a [reflected cross-site scripting](https://portswigger.net/web-security/cross-site-scripting/reflected) vulnerability in the search blog functionality. The reflection occurs inside a template string with angle brackets, single, and double quotes HTML encoded, and backticks escaped. To solve this lab, perform a cross-site scripting attack that calls the `alert` function inside the template string.

  

  

## Solution

In the Blog search functionality, searched for my name and where it is reflecting inside a javascript code 

  

![]({{ site.baseurl }}/assets/images/image%206.png)  

  

Some Interesting thing about his javascript template literal where in our case the value which we passed through the search functionality displays a kind of variable we define right like in linux we do right `$line` but let’s not go that way in javascript, we do that as well using `${...}`⁠ 

  

  

> - JavaScript template literals: String literals allowing embedded JavaScript expressions.
> - Encapsulation: Template literals enclosed in backticks, not quotation marks.
> - Embedded expressions: Identified by `${...}` syntax.
> - Example: Printing a welcome message with user's display name.
> - XSS context: Exploiting template literals for XSS attacks.
> - No need to terminate literal; ${...} syntax executes embedded JavaScript.
> - Example XSS context: Injecting malicious code within ${...} expression.

  

Now, In the search functionality, we have entered the following payload `${alert(1)}` we had a alert popup and the lab is solved 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408123742.png)