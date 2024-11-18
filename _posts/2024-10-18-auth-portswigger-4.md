---
layout: post
title:  "Username enumeration via response timing"
date:   2024-10-18 10:20:54 +0530
categories: [BSCP, Authentication]
---

## Objective 

This lab is vulnerable to username enumeration using its response times. To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.

- Credentials: `wiener:peter`
- Candidate usernames: https://portswigger.net/web-security/authentication/auth-lab-usernames
- Candidate passwords: https://portswigger.net/web-security/authentication/auth-lab-passwords

## Solution 

Firstly, we can login as `wiener` and check around for any functionalities. The credentials were provided in the lab objectives. 

![]({{ site.baseurl }}/assets/images/auth/auth-15.png)

Before exploiting further, I generally entered the wrong credentials on the login page for 4-5 times. There is a ban over there, where if you do more than 5, you will be banned for 30 minutes, where you can only login after 30 minutes actually. 

But there is a way to bypass this by passing a custom header called `X-Forwarded-For` where you can specify any IP address and you can able to bypass it for every brute force you are doing. 

Again, what I did was:
- Entered some wrong credentials
- Intercepted the request into BurpSuite
- Passed it to Intruder
Let's check on where the placeholders are that we have to place. 

![]({{ site.baseurl }}/assets/images/auth/auth-16.png)

Now I have added the custom header and I placed the placeholder for the particular IP address at the end of the IP address and then I also gave a placeholder for the username. What happens is, now through the pitchfork attack where we placed two placeholders, right? One for the IP address and one for the username. Now, we have to supply the wordlist for it, where let's check on how the wordlist works. 

I also entered a long password in order to take the login process time effectively. So if you pass a password, let's assume it's greater than 256 characters. When you pass it, the login flow will take a particular amount of time to process it, which is enough time to check whether this username is correct or wrong. 

![]({{ site.baseurl }}/assets/images/auth/auth-17.png)

Coming into the word list, we used the username list where it was mentioned in the lab objective. We just copy-pasted it into the second payload list. For the first set, we need to do a brute force attack on the end of the IP address, from 1 to 100. 

![]({{ site.baseurl }}/assets/images/auth/auth-18.png)

Now, let's start the attack and check for any weird responses. 

Once the attack is done, there is one username which is called 'ar' with a response packet of 908. It's an interesting stuff to look into. Actually, now what we can do is we'll take the same request and push it to Intruder and repeat the same process. One thing we did for the username now we have to brute force the password field actually. 

![]({{ site.baseurl }}/assets/images/auth/auth-19.png)

So, as mentioned earlier, we're going to repeat the same setup again for the password field. If you remember, we did initially for the username field, and to recall it, we found that username called `ar` which is there. Now, we have to take the username as `ar` and brute force the password field with the same setup again and again. 

But there is a slight change in the X-Forwarded-For header, where we used initially 1.1.1.1. Now we are going to use 2.1.1.$1$ and the last digit will be brute-forced. 

![]({{ site.baseurl }}/assets/images/auth/auth-20.png)

And now I just started the attack. Surprisingly, there is a 302, and with the password `sunshine` 

![]({{ site.baseurl }}/assets/images/auth/auth-21.png)

And now finally we can use the username as `ar` and the password as `sunshine` which will solve the lab 

![]({{ site.baseurl }}/assets/images/auth/auth-22.png)