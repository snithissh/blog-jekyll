---
layout: post
title:  "Reflected XSS in canonical link tag"
date:   2024-09-30 09:39:54 +0530
categories: [BSCP, XSS]
---


## Intro

This lab reflects user input in a canonical link tag and escapes angle brackets.

To solve the lab, perform a [cross-site scripting](https://portswigger.net/web-security/cross-site-scripting) attack on the home page that injects an attribute that calls the `alert` function.

  

To assist with your exploit, you can assume that the simulated user will press the following key combinations:

- `ALT+SHIFT+X`
- `CTRL+ALT+X`
- `Alt+X`

  

Please note that the intended solution to this lab is only possible in Chrome.  

## Solution

In the following lab, we have our lab URL embedded inside the canonical tag and we have to escape it 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408123126.png)  

  

With the following payload `?'accesskey='x'onclick='alert(1)` we where able to escape out of the canonical tag 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408123142.png)  

  

Now In order the payload to work, you need to be in an chrome browser and press `ALT+SHIFT+X` → Payload will execute and lab is solved 

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408123155.png)