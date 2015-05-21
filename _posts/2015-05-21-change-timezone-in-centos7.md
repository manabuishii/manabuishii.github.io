---
layout: post
title: Change TimeZone in centos7
date: 2015-05-21T10:10:00+09:00
---

Change TimeZone in CentOS7
Before setting TimeZone to JST

***date*** command output looks like this.

```
$ date
Thu May 21 01:01:03 UTC 2015
```

Change TimeZone

```
timedatectl set-timezone Asia/Tokyo
```

***date*** command output after set to JST

```
$ date
Thu May 21 10:05:30 JST 2015
```
