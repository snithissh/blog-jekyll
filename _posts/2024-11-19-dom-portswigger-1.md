---
layout: post
title:  "DOM XSS using web messages"
date:   2024-11-19 10:20:54 +0530
categories: [BSCP, DOM Based]
---

## Objective 

This lab demonstrates a simple web message vulnerability. To solve this lab, use the exploit server to post a message to the target site that causes the `print()` function to be called. 

## Solution 

Looking into the source of the homepage, we have the following javascript code 

```js
<script>
window.addEventListener('message', function(e) {
document.getElementById('ads').innerHTML = e.data;
})
</script>
```

Where it listens for messages sent to the window (e.g., via postMessage) and dynamically updates the inner HTML of an element with the ID ads based on the received message.. which can be dangerous if `e.data` isn’t validated or sanitized

Now with the following payload, 

```html
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
```

where we are it initally starts with embed an external website into our page where in our case we are utilising the burp collaborator and then, we are sending the message through `postMessage` to iframe content window and since origin is set to wildcard `*` and XSS payload triggers through an broken image as usual 

Now we can just copy the payload and paste it on to the exploit server and `store -> View Exploit` and you can see that our payload executed and triggers `print()` function 

![]({{ site.baseurl }}/assets/images/dom/dom.png)

Now when we deliver the exploit to victim which solves the lab..