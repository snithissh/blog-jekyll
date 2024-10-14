# Information disclosure in error messages

  

This lab's verbose error messages reveal that it is using a vulnerable version of a third-party framework. To solve the lab, obtain and submit the version number of this framework.

  

## Solution

  

This is just regular ecommerce website where buy and checkout the products actually 

  

![](../Files/image%2015.png)  

  

  

  

Now click on “View details” on any product and check the URI path which is `/product?productId=1`  

  

  

![](../Files/image%2016.png)  

  

  

Pass `’`  at the end of the URI like `/product?productId=1’` and analyse the error response you will get framework details like `Apache Struts 2 2.3.31`⁠ in our case

  

![](../Files/image%2017.png)