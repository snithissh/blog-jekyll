---
layout: post
title:  "File path traversal, traversal sequences blocked with absolute path bypass"
date:   2024-10-13 12:20:54 +0530
categories: [BSCP, Path Traversal]
---

  

This lab contains a path traversal vulnerability in the display of product images.

The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory.

To solve the lab, retrieve the contents of the `/etc/passwd` file.

  

## Solution

  

Same like the previous lab [File path traversal, simple case](File path traversal, simple case.md) where we saw that `images` being grabbed through the subdirectory via `filename`⁠ parameter 

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2014.png)  

  

When we keep on traverse, we didn’t receive anything because the lab blocks traversal sequence 

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2015.png)  

  

But it allows absolute sequence meaning rather than traversing like `../../`  to get the contents of `/etc/passwd`  we can specify the payload which is `/etc/passwd`  directly into our filename parameter and results in disclosing the contents

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2016.png)