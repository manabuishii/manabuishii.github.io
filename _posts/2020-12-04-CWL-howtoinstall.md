---
layout: post
title: cwltoolをインストールする2020
date: 2020-12-04T13:43:56
---

# cwltoolをインストールする2020

この記事は、[Common Workflow Language \(CWL\) Advent Calendar 2020 \- Adventar](https://adventar.org/calendars/5340) の4日目と、
[Common Workflow Language \(CWL\) Advent Calendar 2019 \- Qiita](https://qiita.com/advent-calendar/2019/cwl) の12日目の記事になります。

2020年、とりあえずCWLを動かしたいので、`cwltool` をいれたいとしたときの入れ方について

python3 で
[venv](https://docs.python.org/ja/3/library/venv.html)
を使って仮想環境を作りそこに、インストールするのが簡単

```
python3 -m venv venv
source venv/bin/activate
pip install cwltool
```

## その他の方法

### debian

aptで入る。
stableだと少し古いかもしれないが、ちょっと試したいならそれでもよいかもしれない

2020年12月4日のパッケージの状況[Debian \-\- Package Search Results \-\- cwltool](https://packages.debian.org/search?keywords=cwltool&searchon=names&suite=all&section=all)

```
Package cwltool
stretch (oldstable) (science): Common workflow language reference implementation
1.0.20170114120503-1: all
buster (stable) (science): Common Workflow Language reference implementation
1.0.20181217162649+dfsg-10: all
bullseye (testing) (science): Common Workflow Language reference implementation
3.0.20200807132242-2: all
sid (unstable) (science): Common Workflow Language reference implementation
3.0.20200807132242-2: all
```

### ubuntu

aptで入る。
[Ubuntu – Package Search Results \-\- cwltool](https://packages.ubuntu.com/search?keywords=cwltool&searchon=names&suite=all&section=all)

```
Package cwltool
bionic (18.04LTS) (science): Common Workflow Language reference implementation [universe]
1.0.20180302231433-1: all
focal (20.04LTS) (science): Common Workflow Language reference implementation [universe]
2.0.20200224214940+dfsg-1: all
groovy (20.10) (science): Common Workflow Language reference implementation [universe]
3.0.20200807132242-2: all
hirsute (science): Common Workflow Language reference implementation [universe]
3.0.20200807132242-2: all
```

## コンテナに関して

cwltoolから、docker を呼び出したいとすると、docker in dockerの状況になる。
これについては、別記事を書く予定

