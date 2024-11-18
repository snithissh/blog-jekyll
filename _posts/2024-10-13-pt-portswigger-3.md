---
layout: post
title:  "File path traversal, traversal sequences stripped non-recursively"
date:   2024-10-13 12:20:54 +0530
categories: [BSCP, Path Traversal]
---
  

This lab contains a path traversal vulnerability in the display of product images.

The application strips path traversal sequences from the user-supplied filename before using it.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

  

## Solution 

  

Same as those previous labs which we saw in [File path traversal, traversal sequences blocked with absolute path bypass](File path traversal, traversal sequences blocked with absolute path bypass.md) and [File path traversal, simple case](File path traversal, simple case.md) we have the same `filename`  parameter 

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2011.png)  

  

When you keep traversing just like other labs where we used `../../etc/passwd` or even an absolute path like directly specifying the payload `/etc/passwd`  didn’t worked out 

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2012.png)  

  

But In the lab description says that like traversing the path ( `../../etc/passwd` ) will often be stripped so that we can bypass like ( `....//etc/passwd` ) when an application process or normalizes the path where it becomes from ( `....// to ../` ) regular traverse 

  

So the complete payload looks like `....//....//....//etc/passwd` and we got the contents of `/etc/passwd` 

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2013.png)