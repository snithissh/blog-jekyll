---
layout: post
title:  "OverTheWire: Bandit Walkthrough (Levels 0-14)"
date:   2020-02-01 21:39:54 +0530
categories: linux
---

The Bandit wargame on OverTheWire is aimed at absolute beginners and teaches the basics of remote server access and Linux command line skills. This post will walk through the solutions for levels 0-34 of the game.

## Some Helpful Commands

- To view the manual page for a command, use `man <command>` 
- If there is no man page, the command might be a shell built-in. In that case use `help <command>`
- To exit an SSH session normally, use `exit` or `Ctrl-d`
- If the session is unresponsive, hit `Enter`, then type `~.` to force close the SSH connection

## Level 0

The goal of this level is to log into the game using SSH.

```bash
$ ssh bandit0@bandit.labs.overthewire.org -p 2220
```

This will connect to the bandit server on port 2220 as user `bandit0`. The password is also `bandit0`.

## Level 0 → Level 1

After logging in, list the files in the home directory:

```bash
bandit0@bandit:~$ ls 
readme
```

There is a file called `readme`. Print its contents:

```bash
bandit0@bandit:~$ cat readme  
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

The password for the next level is `boJ9jbbUNNfktd78OOpsqOltutMc3MY1`.

## Level 1 → Level 2 

The password file is named `-`.

```bash
bandit1@bandit:~$ ls
-
```

To open a file whose name starts with a dash, prefix the filename with `./`:

```bash
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9  
```

The password for the next level is `CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9`.

## Level 2 → Level 3

The filename contains spaces. To open files with spaces in their names, enclose the name in quotes or escape the spaces with backslashes:

```bash
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat "spaces in this filename"
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

The password is `UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK`.

## Level 3 → Level 4

The password file is hidden. Use `ls -a` to show hidden files:

```bash  
bandit3@bandit:~$ ls -a
.  ..  .hidden 
bandit3@bandit:~$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

The password is `pIwrPrtPN36QITSp3EQaw936yaFoFgAB`.

## Level 4 → Level 5

There are several files in the `inhere` directory. We need to find the one that is human-readable and contains the password. The `file` command can identify file types:

```bash
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data 
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text  
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$ cat ./-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh  
```

The password is in the ASCII text file `-file07`: `koReBOKuIDDepwhWk7jZC0RTdopnAYKh`.

## Level 5 → Level 6

The password file has the following properties:
- human-readable
- 1033 bytes in size
- not executable

We can use `find` with these criteria:

```bash  
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ find . -type f -size 1033c ! -executable
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

The password is `DXjZPULLxYr17uwoI01bNLQbtFemEgo7`.

## Level 6 → Level 7

The password file is somewhere on the server and has the following properties:
- owned by user bandit7
- owned by group bandit6
- 33 bytes in size
 
```bash
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```

`2>/dev/null` redirects error messages to `/dev/null` to keep the output clean. The password is `HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs`.

## Level 7 → Level 8

The password is in `data.txt` next to the word `millionth`:

```bash  
bandit7@bandit:~$ grep millionth data.txt
millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

The `grep` command searches files for a given pattern. The password is `cvX2JJa4CFALtqS87jk27qwqGhBM9plV`.

## Level 8 → Level 9

The password is the only line of text that occurs only once in `data.txt`:

```bash
bandit8@bandit:~$ sort data.txt | uniq -u  
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

`sort` sorts the lines alphabetically and `uniq -u` prints only unique lines. The password is `UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR`.

## Level 9 → Level 10

The password is one of the few human-readable strings in `data.txt`, preceded by several `=` characters:

```bash
bandit9@bandit:~$ strings data.txt | grep ===  
========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk  
```

`strings` extracts printable character sequences from binary files. The password is `truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk`.

## Level 10 → Level 11

The `data.txt` file contains base64 encoded data. Decode it to get the password:

```bash  
bandit10@bandit:~$ base64 -d data.txt
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```

The password is `IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR`.

## Level 11 → Level 12

The password has been ROT13 encrypted:

```bash
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```

ROT13 rotates each letter 13 places in the alphabet. The `tr` command performs the decryption. The password is `5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu`.

## Level 12 → Level 13

The `data.txt` file is a hexdump of a file that has been repeatedly compressed. We need to reverse this process:

```bash
bandit12@bandit:~$ mkdir /tmp/myname123
bandit12@bandit:~$ cp data.txt /tmp/myname123
bandit12@bandit:~$ cd /tmp/myname123

bandit12@bandit:/tmp/myname123$ xxd -r data.txt > data
bandit12@bandit:/tmp/myname123$ file data
data: gzip compressed data, was "data2.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
bandit12@bandit:/tmp/myname123$ mv data data.gz
bandit12@bandit:/tmp/myname123$ gunzip data.gz

bandit12@bandit:/tmp/myname123$ file data
data: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/myname123$ mv data data.bz2
bandit12@bandit:/tmp/myname123$ bunzip2 data.bz2

bandit12@bandit:/tmp/myname123$ file data
data: gzip compressed data, was "data4.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
bandit12@bandit:/tmp/myname123$ mv data data.gz
bandit12@bandit:/tmp/myname123$ gunzip data.gz

bandit12@bandit:/tmp/myname123$ file data
data: POSIX tar archive (GNU)
bandit12@bandit:/tmp/myname123$ mv data data.tar
bandit12@bandit:/tmp/myname123$ tar xf data.tar
bandit12@bandit:/tmp/myname123$ file data5.bin 
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/myname123$ mv data5.bin data5.tar
bandit12@bandit:/tmp/myname123$ tar xf data5.tar
bandit12@bandit:/tmp/myname123$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/myname123$ mv data6.bin data6.bz2
bandit12@bandit:/tmp/myname123$ bunzip2 data6.bz2

bandit12@bandit:/tmp/myname123$ file data6
data6: POSIX tar archive (GNU)
bandit12@bandit:/tmp/myname123$ tar xf data6
bandit12@bandit:/tmp/myname123$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
bandit12@bandit:/tmp/myname123$ mv data8.bin data8.gz
bandit12@bandit:/tmp/myname123$ gunzip data8.gz

bandit12@bandit:/tmp/myname123$ file data8
data8: ASCII text
bandit12@bandit:/tmp/myname123$ cat data8
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```

We copy the file to a temporary directory, convert the hexdump back to binary with `xxd -r`, then repeatedly decompress and unarchive it based on the file type hints from the `file` command. The final ASCII text file contains the password `8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL`.

---

*To be continued in Part 2...*

This walkthrough covered levels 0-13 of the Bandit wargame. The remaining levels will be covered in the next part. Stay tuned and keep hacking!