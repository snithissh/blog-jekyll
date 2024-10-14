# Source code disclosure via backup files

  

This lab leaks its source code via backup files in a hidden directory. To solve the lab, identify and submit the database password, which is hard-coded in the leaked source code.

  

## Solution

  

Looking into page source doesn’t reveal anything but checking `/robots.txt` revealed that it disallows `/backup` folder

  

![](../Files/image%208.png)  

  

Accessing the `/backup` folder shows there is file exists in index of directory called `ProductTemplate.java.bak`⁠ 

  

![](../Files/image%209.png)  

  

  

Clicking the file and reveals full source code

  

![](../Files/image%2010.png)  

  

Just copy and paste the database password as a solution and submit to solve the lab 

  

![](../Files/image%2011.png)