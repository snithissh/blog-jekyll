---
layout: post
title:  "DOM Based XSS in document.write sink using source location.search inside a select element"
date:   2024-09-28 09:39:54 +0530
---


## Intro

This lab contains a DOM-based cross-site scripting vulnerability in the stock checker functionality. It uses the JavaScript document.write function, which writes data out to the page. The document.write function is called with data from location.search which you can control using the website URL. The data is enclosed within a select element.

To solve this lab, perform a cross-site scripting attack that breaks out of the select element and calls the alert function.
 

## Solution

In this lab, Just like amazon and flipkart where we can look out for the product and checkout the stocks but in our case, we found the following lines of JS code

  

```js
var stores = ["London","Paris","Milan"];  
var store = (new URLSearchParams(window.location.search)).get('storeId');  
document.write('<select name="storeId">');  
if(store) {  
 document.write('<option selected>'+store+'</option>');  
}  
for(var i=0;i<stores.length;i++) {  
    if(stores[i] === store) {  
            continue;  
    }  
    document.write('<option>'+stores[i]+'</option>');  
}  
document.write('</select>');
```

  

This code snippet creates a dropdown menu (a `<select>` element) dynamically based on a query parameter called "storeId" in the URL. It first defines an array of city names. Then, it retrieves the value of the "storeId" parameter from the URL. If this parameter exists, it sets the corresponding city as the pre-selected option in the dropdown. Next, it iterates through the array of city names, adding each as an option in the dropdown, except for the one already pre-selected if it matches. Finally, it completes the dropdown element. In essence, this code allows users to select a city from a dropdown menu, with the default selection based on the "storeId" parameter in the URL, if provided.

  

Adding a parameter `&storeId=nithissh` with a random value and In the response, shows us that it reflected inside `<select>` statement and created a new option called `nithissh`⁠

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408120324.png)  

  

With the `</select>` we have closed our `<select>` tag and followed given payload with a `<h2>XSS</h2>` where it will write a `<h2>` in a newline and payload will execute and confirms that there is HTML injection

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408120307.png)  

  

Great right, let's remove the `<h2>` tag payload and will enter the following `<img src=x onerror=alert(1)>` where it will execute our payload and lab is solved

  

![]({{ site.baseurl }}/assets/images/Pasted%20image%2020240408120256.png)