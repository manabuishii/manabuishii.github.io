---
layout: post
title: Jenkins and slow job
date: 2015-03-17T02:16:00
---

When job has some problems, it takes too long time.

But it is still running, so you can't notice this problem via API.
Because job is not failed.

If job takes too long time, it want to be fail state.

You can use **Build-timeout plugin** .

* [Build-timeout Plugin - Jenkins - Jenkins Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Build-timeout+Plugin "Build-timeout Plugin - Jenkins - Jenkins Wiki")

And you maybe check how many job in queue via API.
