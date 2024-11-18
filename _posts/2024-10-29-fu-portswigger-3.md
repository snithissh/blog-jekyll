---
layout: post
title:  "Web shell upload via extension blacklist bypass"
date:   2024-10-29 10:20:54 +0530
categories: [BSCP, File Upload Vulnerabilities]
---

## Objective 

This lab contains a vulnerable image upload function. Certain file extensions are blacklisted, but this defense can be bypassed due to a fundamental flaw in the configuration of this blacklist.

To solve the lab, upload a basic PHP web shell, then use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter` 

## Solution 

When we upload a PHP file just like the other labs, but here uploading PHP files are strictly restricted and as it doesn't allow 

![]({{ site.baseurl }}/assets/images/fileupload/fu-8.png)

But while we actually learning through the topic, they discussed about the write access to `.htaccess` and through that we can update the certain configurations which actually limits us from uploading the PHP file 

Now, we can update the filename to `.htaccess` and content-type as `text/plain` and contents of `.htaccess` with the following `AddType application/x-httpd-php .nithissh` where it will rewrite the configuration file and helps us in uploading a PHP file but instead of `.php` extension we will use `.nithissh`

Updated those and sent the request, it worked and we did update the configuration on `.htaccess`

![]({{ site.baseurl }}/assets/images/fileupload/fu-9.png)

Now redo the same, upload the php file.. But when you intercept the request.. Change it from `.php` to `.nithissh` and sent the request.. Now it got uploaded successfully 

![]({{ site.baseurl }}/assets/images/fileupload/fu-10.png)

Now open the avatar in new tab, will show the contents of `/home/carlos/secret` and submit it as solution and that solves the lab 

![]({{ site.baseurl }}/assets/images/fileupload/fu-11.png)