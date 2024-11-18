---
layout: post
title:  "Web shell upload via obfuscated file extension"
date:   2024-10-29 10:20:54 +0530
categories: [BSCP, File Upload Vulnerabilities]
---

## Objective 

This lab contains a vulnerable image upload function. Certain file extensions are blacklisted, but this defense can be bypassed using a classic obfuscation technique.

To solve the lab, upload a basic PHP web shell, then use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter` 

## Solution

Like the previous lab, uploaded the same `exploit.php` through an avatar upload and now we face an error where it will allow only PNG and JPEG files 

![]({{ site.baseurl }}/assets/images/fileupload/fu-12.png)

Adding the nullbyte `%00` at the end of the extension like `.php%00` usually stops the application from further processing and proceeds the request but still it didn't workout 

![]({{ site.baseurl }}/assets/images/fileupload/fu-13.png)

But when we did add an additional extension at the end of it.. worked out and our exploit got uploaded successfully.. meaning it kinda validates the extension at the end 

![]({{ site.baseurl }}/assets/images/fileupload/fu-14.png)

Now open the image in new tab, you will face a `404` but you can remove `%00.jpg` from it and access it as `exploit.php` and you will see a secret code and submit as solution and that solves the lab 

![]({{ site.baseurl }}/assets/images/fileupload/fu-15.png)
