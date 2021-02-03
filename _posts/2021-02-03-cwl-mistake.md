---
layout: post
title: この間まで動いていたCWLが動かないときに確認したいこと。rawのURLかどうか
date: 2021-02-03T09:43:56
---

# この間まで動いていたCWLが動かないときに確認したいこと。rawのURLかどうか

この記事は、
[Common Workflow Language \(CWL\) Advent Calendar 2019 \- Qiita](https://qiita.com/advent-calendar/2019/cwl)の13日目と、
[Common Workflow Language \(CWL\) Advent Calendar 2020 \- Adventar](https://adventar.org/calendars/5340)の23日目の記事です。


CWLワークフローとして
githubのアドレスを書いたときなどに、rawではないデータのURLを渡してしまう。

このような減少になったときは、だいたい以下のようなことを思う、口に出すので、rawかどうかを確認

- この前まで動いていたCWLが動かない。
- 昨日までvalidateがとおっていたのに、通らない
- うーん、 `ブラウザでgithub` のコードや履歴見ても問題がない

だいたいこのような場合　CWLとして指定しているURLがgithubの普通のHTMLページで
rawではないところを指定している。

そして最近rawのアドレスが変わっている。
ということに気づくようになります。

## 違いを目で見るけることは難しい

以下に例をだしておきますが、ぱっとみ、まちがっているかどうか判定が難しい気がします。

## validateが通らない例

rawのURL渡していない
HTMLが返るURLを渡している

```
cwltool --validate \
https://github.com/pitagora-network/DAT2-cwl/blob/develop/workflow/meta16s-seq/meta16s-seq.demo.cwl
```

## うまくvalidateが通る例

rawのURL渡している

```
cwltool --validate \
https://github.com/pitagora-network/DAT2-cwl/raw/develop/workflow/meta16s-seq/meta16s-seq.demo.cwl
```
