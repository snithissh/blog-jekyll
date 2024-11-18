---
layout: post
title:  "Username enumeration via subtly different responses"
date:   2024-10-17 10:20:54 +0530
categories: [BSCP, Authentication]
---

## Objective 

This lab is subtly vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:

- Candidate usernames: https://portswigger.net/web-security/authentication/auth-lab-usernames
- Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page. 

## Solution 

We have login page where it takes the `/login` as a `POST` request and then takes `username` and `password` as body.. I entered the wrong username and password and responded with the 200 response (status code 200) and if you see the below response, we have an invalid username or password. 

![]({{ site.baseurl }}/assets/images/auth/auth-10.png)

so think of it like what we generally do if you face a login page in ncdf right so we'll just do a brute force attack and so what we're gonna do is we're gonna brute force the username and password 

but initially if you observe the particular title which mentioned the lab objective subtile responses right what we can generally do is we'll first brute force username and then password we'll just brute force the username understand what the response is and again brute force the password

I just sent the request to Intruder and placed a brute force placeholder for the username field only, not for the password, for the initial stage. 

![]({{ site.baseurl }}/assets/images/auth/auth-11.png)

Once I start the BruteForce, it is completed within a second. But when I look into the 7th request, which is the payload called as adm, have the lowest response packet actually `(158)`. It is kind of sus, so we just look into further. 

![]({{ site.baseurl }}/assets/images/auth/auth-12.png)

Once after finding the username (ADM), I started brute-forcing on the password. On the `53rd` request, it was a `ranger` with a status code of `302`, which is obviously a redirect. So now let's use the username and password and login into it to solve the lab.  

![]({{ site.baseurl }}/assets/images/auth/auth-13.png)

Now, with the username as `adm` and the password as `ranger`, we were able to log in and that solves the lab. 

![]({{ site.baseurl }}/assets/images/auth/auth-14.png)