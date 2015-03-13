---
layout: post
title: Install executable files without make install
---

If there is no *install* target in Makefile,
You can copy them in specific directory by using following command.

```
cp `find . -perm 755` /usr/local/bin
```
