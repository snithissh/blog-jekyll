---
layout: post
title:  "SameSite Strict bypass via client-side redirect"
date:   2024-10-06 12:20:54 +0530
categories: [BSCP, CSRF]
---

## Objective 

This lab's change email function is vulnerable to CSRF. To solve the lab, perform a CSRF attack that changes the victim's email address. You should use the provided exploit server to host your attack.

You can log in to your own account using the following credentials: `wiener:peter` 

## Solution 

In this lab, we do have the same kind of functionality `Email Update` just like the other labs.. But there is a twist here is that it does actually support `GET` request meaning you can update the email, intercept the traffic and change it from `POST` -> `GET` and request passes and updates the email to whatever email we want 

![]({{ site.baseurl }}/assets/images/csrf-33.png)

Before exploring further we need to understand on how the cookies will work if the same strict works, If a cookie is set with the SameSite=Strict attribute, browsers will not send it in any cross-site requests. In simple terms, this means that if the target site for the request does not match the site currently shown in the browser's address bar, it will not include the cookie. 

Previously, we found that through the `GET` request we can able to update the email.. Then what ?

We need a open redirect vulnerability to chain it with the `GET` request based email update.. let's go further 

In the home page, below the post we can able to post the comment and once after posting the comment, there will be redirect to a particular newly posted comment 

![]({{ site.baseurl }}/assets/images/csrf-34.png)

Ok, basically the url looks like this `https://0a0d009a039da07881de251e009500df.web-security-academy.net/post/comment/confirmation?postId=3` and redirect to `https://0a0d009a039da07881de251e009500df.web-security-academy.net/post/comment/3` and what I'm gonna do his edit the url like `https://0a0d009a039da07881de251e009500df.web-security-academy.net/post/comment/confirmation?postId=3../../my-account` kind of client-side path traversal if the confirmation is successful for postId of 3 and it will later redirect to `/my-account`

Then, let's build a CSRF POC like this 

```html
<script>
    document.location = "https://0a0d009a039da07881de251e009500df.web-security-academy.net/post/comment/confirmation?postId=3../../../my-account/change-email?email=tuv%40attcker.com%26submit=1"
</script>
```

Now copy the CSRF POC, Paste it to exploit server and then click on `Store -> Deliver the exploit to victim` will actually solve the lab 