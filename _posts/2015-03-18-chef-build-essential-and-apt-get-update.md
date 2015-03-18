---
layout: post
title: Chef apt-get update before build-essential
date: 2015-03-17T07:29:00
---

I want to apply build-essential cookbook.

* [opscode-cookbooks/build-essential](https://github.com/opscode-cookbooks/build-essential/ "opscode-cookbooks/build-essential")


CentOS does not have error.
But Ubuntu has error.

I already write to apply apt.
But it's not execute before build-essential execute.


* [opscode-cookbooks/apt](https://github.com/opscode-cookbooks/apt "opscode-cookbooks/apt")

Because of apt-get update do not execute before build-essential packages installed.

* [Not running apt-get update before installing packages · Issue #41 · opscode-cookbooks/build-essential](https://github.com/opscode-cookbooks/build-essential/issues/41 "Not running apt-get update before installing packages · Issue #41 · opscode-cookbooks/build-essential")

I setup following attributes and run.

```
apt:
  compile_time_update: true
build_essential:
  compiletime: true
```

In other words

```
node['apt']['compile_time_update'] = true
node['build_essential']['compiletime'] = true
```

After execute, I get WARNING messages.

```
[2015-03-18T07:24:10+00:00] WARN: node['build-essential']['compiletime'] has been deprecated. Please use
node['build-essential']['compile_time'] instead. I have gracefully converted the
attribute for you, but this warning and converstion will be removed in the next
major release of the build-essential cookbook.
```

Final version

```
apt:
  compile_time_update: true
build_essential:
  compile_time: true
```

In other words

```
node['apt']['compile_time_update'] = true
node['build_essential']['compile_time'] = true
```
