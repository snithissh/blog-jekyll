---
layout: post
title:  "Web shell upload via Content-Type restriction bypass"
date:   2024-11-02 10:20:54 +0530
---

## Objective 

This lab contains a vulnerable image upload function. It attempts to prevent users from uploading unexpected file types, but relies on checking user-controllable input to verify this.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter`

## Solution 

When we upload a PHP file through the avatar upload, it doesn't accept it. It says only the JPEG and PNG files are allowed. It's not validating based on the file type, rather it is validating based on the content type of the file. The server restrictions are based on the content type. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-21.png)

So you get the same request, but you change the content type to `image/jpeg` from `text/php`  The request got accepted and the file exploit.php got uploaded. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-22.png)

As a help, what we generally do is we just open the uploaded avatar and a new tab will have a secret code over there. We submit it as a solution that solves the lab. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-23.png)