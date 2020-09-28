---
title: ringzer0ctf - SysAdmin Linux
description: My writeup on ringzer0ctf SysAdmin Linux challenges.
categories:
 - ringzer0ctf
tags: ringzer0ctf linux
---

![](https://miro.medium.com/max/1472/1*5M1nixCj5FzRwBMVrU-EkQ.png)

You can find the challenges there > [ringzer0ctf](https://ringzer0ctf.com/challenges){:target="_blank"}

## Summary

Something different from vulnhub boxes, these challenges require command line skills! Great for beginners if they want to learn linux CLI. Let’s pwn them! :sunglasses:

## SysAdmin Part 1

![](https://i.imgur.com/VqYGrG6.png)

Let's connect to server as `morpheus:VNZDDLq2x9qXCzVdABbR1HOtz` and our goal is to find `Trinity` password!

We can see that trinity is a system user:

```
morpheus@lxc-sysadmin:~$ cat /etc/passwd | grep -i trinity
trinity:x:1001:1002::/home/trinity:/bin/bash
```

Let's use `grep` to search recursive:

```
morpheus@lxc-sysadmin:~$ grep -Ri "trinity" / 2>/dev/null
/etc/rc.local:/bin/sh /root/files/backup.sh -u trinity -p Flag-7e0cfcf090a2fe53c97ea3edd3883d0d &                                                                                                                             97ea3edd3883d0d &
```

We have the first flag! `Flag-7e0cfcf090a2fe53c97ea3edd3883d0d`

## SysAdmin Part 2

![](https://i.imgur.com/DmuTk1b.png)

We don't need to change user, our goal now is to find architect password.

Using the same `grep` technique we can detect a base64 password:

```
morpheus@lxc-sysadmin:~$ grep -R "architect" / 2>/dev/null
/etc/fstab:#//TheMAtrix/phone  /media/Matrix  cifs  username=architect,password=$(base64 -d "RkxBRy0yMzJmOTliNDE3OGJkYzdmZWY3ZWIxZjBmNzg4MzFmOQ=="),iocharset=utf8,sec=ntlm  0  0
```

Let's decode it:

```
morpheus@lxc-sysadmin:~$ echo RkxBRy0yMzJmOTliNDE3OGJkYzdmZWY3ZWIxZjBmNzg4MzFmOQ== | base64 -d
FLAG-232f99b4178bdc7fef7eb1f0f78831f9
```

We got the flag! `FLAG-232f99b4178bdc7fef7eb1f0f78831f9`

## SysAdmin Part 3
