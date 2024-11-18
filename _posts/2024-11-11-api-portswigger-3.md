---
layout: post
title:  "Exploiting server-side parameter pollution in a query string"
date:   2024-11-11 10:20:54 +0530
categories: [BSCP, API Vulnerabilities]
---

## Objective 

To solve the lab, log in as the administrator and delete carlos. 


## Solution

Since we don't have any credentials provided in the lab, we need to play around with the forgot password functionality. 

The reset password flow looks like this: When you click on "Forgot password," it asks for the username (in this case, we're giving it as administrator). Once after giving it, it shows the response that we've sent a password reset email to the following email ID, but the email is obfuscated. 

![]({{ site.baseurl }}/assets/images/api/api-7.png)

Adding the `#` in an url encoded format shows a field error but when we add `%26` which is urlencoded format of `&` symbol and combining both reveals the same response like previous but you think its the same not really.. It's user controllable now 

here you can see I asked `username` inside the field parameter where it shown the `type` and an `Actual result` 

![]({{ site.baseurl }}/assets/images/api/api-8.png)

In the forgot password functionality, go through the javascript code we found that there is another type and result combination

```js
forgotPwdReady(() => {
    const queryString = window.location.search;
    const urlParams = new URLSearchParams(queryString);
    const resetToken = urlParams.get('reset-token');
    if (resetToken)
    {
        window.location.href = `/forgot-password?reset_token=${resetToken}`;
    }
    else
    {
        const forgotPasswordBtn = document.getElementById("forgot-password-btn");
        forgotPasswordBtn.addEventListener("click", displayMsg);
    }
});
```

Where you add `field=` with `field=reset_token` and in the response you will get the reset token of `administrator` user

![]({{ site.baseurl }}/assets/images/api/api-9.png)

This is how the forgot password URL look like `https://0a8100ec03bd833c81c870930045009a.web-security-academy.net/forgot-password?reset_token=ugb0g5n5v51blwi5xotq0emie148ocvy` and update the new password here 

![]({{ site.baseurl }}/assets/images/api/api-10.png)

After resetting the administrator password, we can login to the admin panel and delete the user called Carlos, and the lab will be solved. 

![]({{ site.baseurl }}/assets/images/api/api-11.png)