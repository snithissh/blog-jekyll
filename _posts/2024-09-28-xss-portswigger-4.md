---
layout: post
title:  "Stored DOM XSS"
date:   2024-09-28 09:39:54 +0530
categories: [BSCP, XSS]
---

## Objective 

This lab demonstrates a stored DOM vulnerability in the blog comment functionality. To solve this lab, exploit this vulnerability to call the alert() function. 

## Solution

This is the blog comment section where XSS will exists according to lab objective actually 

![]({{ site.baseurl }}/assets/images/xss-1.png) 

Enter the following XSS payload `<img src=x onerror=confirm(1)>` reflects as plain text rather than executing it.. Checking the inspect element of the payload reflects inside `<p>` tag 

![]({{ site.baseurl }}/assets/images/xss-2.png) 

Looking the JS file, It says that it will escape the HTML for the first time occurance and not happens every time and that's my assumption 

```js
function escapeHTML(html) {
 return html.replace('<', '&lt;').replace('>', '&gt;');
}
```

Now let's say in the comment section, enter the following payload `<><img src=x onerror=confirm(1)>` even if the sanitization starts with first two character `<>` and rest others won't get escaped 

Submitted the payload as comment and opening the blog again.. XSS payload got triggered 

![]({{ site.baseurl }}/assets/images/xss-3.png) 