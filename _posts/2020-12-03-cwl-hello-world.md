---
layout: post
title: CWL basecommand でサブコマンドがあるツールで起こりがちなこと。--validateが通っているのにエラー
date: 2020-12-02T13:43:56
---
# CWL basecommand でサブコマンドがあるツールで起こりがちなこと。--validateが通っているのにエラー

この記事は、[Common Workflow Language \(CWL\) Advent Calendar 2020 \- Adventar](https://adventar.org/calendars/5340) の3日目と、
[Common Workflow Language \(CWL\) Advent Calendar 2019 \- Qiita](https://qiita.com/advent-calendar/2019/cwl) の11日目の記事になります。

`bamtools sort` とか、 `picard BamIndexStats`

などを、CWLで作ったときに、

`cwltool --validate`　でチェックしてもエラーがない。

が、実行するとエラーがでるときがある。

結論から先に書くこと `basecommand` の中でダブルクォートで、サブコマンドまでを１つでくくっていないかを確認する。

```
basecommand: ["bamtools sort"]
```

とか

```
basecommand: ["picard BamIndexStats`"]
```

のようになっていないかを確認する。

意図通りに動作させるには

```
basecommand: ["bamtools", "sort"]
```

とか

```
basecommand: ["picard", "BamIndexStats`"]
```

のように、ダブルクォートで括る部分に空白を含まないようにするということである。

## 例 echo hello world

### エラーになる例

```
#!/usr/bin/env cwl-runner
class: CommandLineTool
cwlVersion: v1.0
baseCommand: ["echo hello"]
inputs: []
outputs:
  - id: out
    type: stdout
stdout: hello.txt
```

これで `cwltool --validate` 実行する

```
$ cwltool --validate echo_hello_valid_but_error.cwl
(省略)
echo_hello_valid_but_error.cwl is valid CWL.
```

実行する

```console
$ cwltool echo_hello_valid_but_error.cwl
(省略)
ERROR 'echo hello' not found: [Errno 2] No such file or directory: 'echo hello'
WARNING [job echo_hello_valid_but_error.cwl] completed permanentFail
{}
WARNING Final process status is permanentFail
$ echo $?
1
$ cat hello.txt
cat: hello.txt: No such file or directory
```



### エラーにならない例

```
#!/usr/bin/env cwl-runner
class: CommandLineTool
cwlVersion: v1.0
baseCommand: [echo, hello]
inputs: []
outputs:
  - id: out
    type: stdout
stdout: hello.txt
```

これで `cwltool --validate` 実行する

```
$ cwltool --validate echo_hello.cwl
(省略)
echo_hello.cwl is valid CWL.
```

実行する

```console
$ cwltool echo_hello.cwl
(省略)
INFO Final process status is success
$ echo $?
0
$ cat hello.txt
hello
```