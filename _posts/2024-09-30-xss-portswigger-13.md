---
layout: post
title:  "Stored XSS into onclick event with angle brackets and double quotes HTML-encoded"
date:   2024-09-30 09:39:54 +0530
---

## Intro

This lab contains a [stored cross-site scripting](https://portswigger.net/web-security/cross-site-scripting/stored) vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the `alert` function when the comment author name is clicked.

  

## Solution

This is a Blog forum where you can view the blog and comment on it

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408144241.png)  

  

After visiting the blog, In each blog we have a functionality to comment it and lab description referenced that as well where it is vulnerable to XSS

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408144653.png)  

  

  

Sprayed out some random payloads across all the fields and found the `Name` field input is getting reflected inside `onclick` event

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408145533.png)  

  

In the website field, Enter the following string `https://jyfvy.com/nithissh` and reflected inside `onclick` event

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408150922.png)  

  

###   

## According to Burp Documentation

When the XSS context is some existing JavaScript within a quoted tag attribute, such as an event handler, it is possible to make use of HTML-encoding to work around some input filters.

  

When the browser has parsed out the HTML tags and attributes within a response, it will perform HTML-decoding of tag attribute values before they are processed any further. If the server-side application blocks or sanitizes certain characters that are needed for a successful XSS exploit, you can often bypass the input validation by HTML-encoding those characters.

  

For example, if the XSS context is as follows:

`<a href="#" onclick="... var input='controllable data here'; ...">`

and the application blocks or escapes single quote characters, you can use the following payload to break out of the JavaScript string and execute your own script:

`&apos;-alert(document.domain)-&apos;`

The `&apos;` sequence is an HTML entity representing an apostrophe or single quote. Because the browser HTML-decodes the value of the `onclick` attribute before the JavaScript is interpreted, the entities are decoded as quotes, which become string delimiters, and so the attack succeeds.

  

## Challenge Continues

Injected the following payload into `?;'-alert(document.domain)-'` into the website name field along with `website URL`

  

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408151941.png)  

  

Unfortunately, still we didn't escape and stuck inside the shit `\`⁠

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408152135.png)  

  

What else, we can do right ?.. Let's take the payload and HTML encode and send it again and payload looks like this after HTML encoding usig burp decoder

  

```
http://nithissh.com&#x3f;&#x3b;&#x27;&#x2d;&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;&#x64;&#x6f;&#x63;&#x75;&#x6d;&#x65;&#x6e;&#x74;&#x2e;&#x64;&#x6f;&#x6d;&#x61;&#x69;&#x6e;&#x29;&#x2d;&#x27;
```

  

Once after submitting it and click on the hyperclick of the website.. XSS pops up and lab is solved and XSS needs an user interaction here because you need go forward and click on the hyperlink to trigger it

  

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408152751.png)  

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408152721.png)