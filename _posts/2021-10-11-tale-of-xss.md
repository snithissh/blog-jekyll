---
layout: post
title:  "A Tale of Weird XSS into $100"
date:   2021-10-11 21:39:54 +0530
---

## Introduction 

Hey Guys , How are you all ? . I hope so your doing good and healthy . So, Lets get started . So , I started searching for bugbounty programme through Google dorking process . I found a program with small scope with `app.reducted.com` and `api.reducted.com`. So , I started doing some google dorking as manually as well as in a automated way using a tool called `pagodo` and using `pentest-tools`

## Tool Reference 

- [Pagado](https://github.com/opsdisk/pagodo)
- [Google dorking using pentest-tools](https://pentest-tools.com/information-gathering/google-hacking)

## Further more 

Above tools are some awesome tools . It gave me some awesome results in my recon process . So , I started playing with login functionality testing that includes I created two accounts and tried session hijacking , Password reset poisoning and similiar to that did every possible ones But unfortunately , Nothing works

<iframe src="https://giphy.com/embed/lPY5d1vUYBreWtF1I0" width="480" height="268" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/snowfallfx-franklin-whats-next-so-lPY5d1vUYBreWtF1I0">via GIPHY</a></p>

I’m so confused , What to do ? . I looked into other functionality that the target has . So , When I looked into settings . We can edit the following such as Bios ,Instagram Handle , Twitter Handle and We can also set the linkedin account . But there is a twist out there we don’t have import the link . we can only import the name of the account only . I can idea of XSS here . But I tried the XSS by simply injecting payloads into Instagram page name field . But Unfortunately It doesn’t works . So , You can ask why can’t we try payloads into fields . But nothing works .

So , I just took break for few hours and I again started working on the target . But this time , I had the idea of why I can’t change by Instagram name to the basic xss payload and Input the payload into the **Instagram field** ( `“><script>alert(document.domain)</script>` ) . So , What ? After I entered , The XSS Payload got Triggered

![]({{ site.baseurl }}/assets/images/xss1-insta.png)

## Conclusion 

In conclusion, always be proactive in seeking out vulnerabilities. I hope this report has provided valuable insights into the importance of vulnerability discovery and remediation.

**Timeline:**

**Reported:** 14/09/2021
**Triaged:** 16/09/2021
**Rewarded $100:** 20/09/2021