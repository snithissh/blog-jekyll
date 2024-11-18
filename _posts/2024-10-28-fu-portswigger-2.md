---
layout: post
title:  "Web shell upload via path traversal"
date:   2024-10-28 10:20:54 +0530
categories: [BSCP, File Upload Vulnerabilities]
---

## Objective 

This lab contains a vulnerable image upload function. The server is configured to prevent execution of user-supplied files, but this restriction can be bypassed by exploiting a secondary vulnerability.

To solve the lab, upload a basic PHP web shell and use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter` 

## Solution 

So just like the last lab, we have the same functionality to upload an avatar. Once after uploading, viewing the avatar image in new tab (usually in the last lab) shows the payload execution, but in this lab, it shows a blank page app. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-4.png)

Intercept the request when you upload the avatar and change the filename from `exploit.php` to `..%2fexploit.php` And now once we change it and send the request, it will upload the files inside `/files` directory rather than `/files/avatars`

![]({{ site.baseurl }}/assets/images/fileupload/fu-5.png)

Now open the uploaded file which is inside the `/files` directory through the browser where it will show the contents of `/home/carlos/secret` due to fact that our php code got successfully executed 

![]({{ site.baseurl }}/assets/images/fileupload/fu-6.png)

Just copy the code and submit it as solution, and that will solve the lab. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-7.png)