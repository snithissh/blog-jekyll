---
layout: post
title:  "File path traversal, simple case"
date:   2024-10-13 12:20:54 +0530
---

  
## Objective 

This lab description says that there is a path traversal exists in the product display images and we need to identity it.. Also this is our first lab so things might be easier 

  

## Solution

The lab looks like older ecommmerce website and that’s how the old flipkart and amazon looks like 

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2017.png)  

  

Now open that particular image in new tab and intercept the request in caido 

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2018.png)  

  

As you can see in the above image, There is an `image` subdirectory where a particular image fetched through the `filename` parameter which means it is pulling off directly from local machine 

  

In the request, we have to keep on traversing until we have a kind of identifier that there is possibility to get our endgoal which is to get the contents of `/etc/passwd` 

  

For a single traverse, we did get an error that “No such file”

  

![]({{ site.baseurl }}/assets/images/path-traversal/image%2019.png)  

  

When I keep traversing like `../etc/passwd` to all the way upto `../../../etc/passwd`  to get the contents of `/etc/passwd` 

  

![]({{ site.baseurl }}/assets/images/image%2020.png)