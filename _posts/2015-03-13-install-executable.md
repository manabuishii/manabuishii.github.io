---
layout: post
title: Install executable files without make install
---

If there is no *install* target in Makefile,
You can copy execute files under current directory into specific directory by using following command.

```
cp `find . -perm 755` /usr/local/bin
```
