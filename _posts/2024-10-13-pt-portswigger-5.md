---
layout: post
title:  "File path traversal, validation of file extension with null byte bypass"
date:   2024-10-13 12:20:54 +0530
---

# Objective

This lab contains a path traversal vulnerability in the display of product images.

The application validates that the supplied filename ends with the expected file extension.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

  

## Solution

  

Opening the image in new tab shows that just through a `filename`  parameter getting the image 

  

![]({{ site.baseurl }}/assets/images/image.png)  

  

When I keep on traversing through the filename parameter like gone from `../../`  to `../../../` and I still face the error **“No file found”**

![]({{ site.baseurl }}/assets/images/image%202.png)  

  

But we remember right, In the lab description, It should end with suitable file extension in our case `jpeg`⁠ but still we face the same error 

  

![]({{ site.baseurl }}/assets/images/image%203.png)  

  

But the lab header tells that it is null byte bypass ( `%00` ) and now when we place nullbyte between the filename and the extension just like `../../../etc/passwd%00.jpeg` still the same error 

  

![]({{ site.baseurl }}/assets/images/image%204.png)  

  

Yuck, think off like what the other filename extension and placing `png` instead off `jpeg`  extension we got the contents of `/etc/passwd` 

  

![]({{ site.baseurl }}/assets/images/image%205.png)