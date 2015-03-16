---
layout: post
title: Compare md5sum and md5 command of mac
---

MD5 command of mac *md5* default output format is different from *md5sum* command.


```
touch md5test.txt
md5 md5test.txt > md5test.txt.md5
cat md5test.txt.md5
```
output is
```
MD5 (md5test.txt) = d41d8cd98f00b204e9800998ecf8427e
```

Of cource mac's md5 command has option to output md5sum format.

```
md5 -r md5test.txt
```

But there are already exist md5 that output format is mac style.
And you want to compare them on linux.
First mac format convert to md5sum format.
```
cat md5test.txt.md5 | awk  '{print $4"  "$2};' | awk '{gsub(/[\(\)]/, ""); print $0}'
```
result is
```
d41d8cd98f00b204e9800998ecf8427e  md5test.txt
```
