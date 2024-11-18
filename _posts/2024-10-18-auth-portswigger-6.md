---
layout: post
title:  "Broken brute-force protection, IP block"
date:   2024-10-18 10:20:54 +0530
categories: [BSCP, Authentication]
---

## Objective 

This lab is vulnerable due to a logic flaw in its password brute-force protection. To solve the lab, brute-force the victim's password, then log in and access their account page.

- Your credentials: `wiener:peter`
- Victim's username: `carlos`
- Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords

## Solution 

Last lab on the user enumeration via response timing. We saw we were brute forcing the IP address via a custom header called 'X-Forwarded-For'. This time, it won't work out (I tried it, and it didn't work out). Instead, what we can do is 

So we have two different wordlists:
- One is for the wiener, which will be repeated 20 times
- For the password wordlist, we have a custom wordlist where every word starts with 'peter' and every 3rd word will start with 'peter'

When we are brute forcing it, if the 1st time the wiener and 'peter' are in the same position, it will be a match (it's 302) and the second time the brute force starts, so we were able to bypass the IP block production right? 

Utilising the following custom list for usernames and passwords from somewhere on github 

Username - https://raw.githubusercontent.com/s4orii/PortSwigger-Lab-Wordlist/refs/heads/main/usernames.txt
Passwords - https://raw.githubusercontent.com/s4orii/PortSwigger-Lab-Wordlist/refs/heads/main/passwords.txt

So, with the username/password we have, we started a brute-force attack using the Intruder. The resource pool has 1 concurrent request per second, so as a result, we found that our username is `carlos` and the password is `starwars` 

![]({{ site.baseurl }}/assets/images/auth/auth-28.png)

Now we will login with the found password and the username as Carlos to solve the lab. 

![]({{ site.baseurl }}/assets/images/auth/auth-29.png)