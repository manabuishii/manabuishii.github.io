---
layout: post
title: Berksfile specify GitHub tag
date: 2015-07-16T15:25:56
---

# Berksfile

I want to specify the version which cookbook is from github.

* [Berkshelf](http://berkshelf.com/)

This site is about Berkshelf.

A lot of information is there.

## Cookbook from GitHub specify version

I want to write following

* cookbook from github
* cookbook name is r-compiledfromsource
* tag 0.3.0

Berksfile line is below.

```
cookbook 'r-compiledfromsource', github: "rikenbit/r-compiledfromsource-cookbook", tag: "0.3.0"
```
# Berksfile.lock

It's useful for cookbook dependencies management among developers.

But forget to remove or update this file during development.

Sometimes I confused that behavior is different from my expectation.

Berksfile.lock is update by

```
berks update
```

or

```
berks update COOKBOOKS
```

Start from clean.

```
rm -rf Berksfile.lock cookbooks
```
