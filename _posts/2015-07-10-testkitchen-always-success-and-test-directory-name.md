---
layout: post
title: test kitchen always success because wrong test directory name
date: 2015-07-13T16:25:56
---

All causes are that I change suite name.

# kitchen verify suites name and test directory name

I change suites name.
```
kitchen test
```
finish successfully.

I put new test case to fail.

but kitchen test still successfully finished.

I execute

```
kitchen converge
```
and
```
kitchen verify
```
result
```
-----> Starting Kitchen (v1.4.1)
-----> Setting up <ucsc-genomebrowserbbbb-ubuntu-1404>...
       Finished setting up <ucsc-genomebrowserbbbb-ubuntu-1404> (0m0.00s).
-----> Verifying <ucsc-genomebrowserbbbb-ubuntu-1404>...
       Preparing files for transfer
       Transferring files to <ucsc-genomebrowserbbbb-ubuntu-1404>
       Finished verifying <ucsc-genomebrowserbbbb-ubuntu-1404> (0m0.00s).
-----> Kitchen is finished. (0m1.09s)
```
It seems to finish successful.

```
$ echo $?
0
```


# Why I changed the suites name ?

According to my memory, when I executed test kitchen, virtualbox name looks like same.
And I set aside those environment to check what happens.
I did not destroy immediately
Result in I could not recognized virtualbox name and environment.

## Changed CI style

After that, I change two things
* Always destroy kitchen environment. Jenkins remember the test result. And I rerun from scratch.
* Change suites name

But latter one is not good for me.


# I change suites name to default again

So I rewrite suites name to default.
Everything goes finished

## Another solution Change test directory name same as suites name

Another solution, renamed test directory name from default to new suites name.
verify process successfully executed.

## Check result output after verify process.

I want to see and certainly if verify process had been executed.

### Using Publish JUnit test result plugin on Jenkins.

* [JUnit Plugin - Jenkins - Jenkins Wiki](https://wiki.jenkins-ci.org/display/JENKINS/JUnit+Plugin)

This plugin creates graph of JUnit results.

If there is no test file, this plugin aborted.

#### Concept to check result output

1. delete result files
2. exec kitchen test

#### Clean before checkout git plugin

* [Git Plugin - Jenkins - Jenkins Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin)

This plugin offer various options.

I add ***Clean before checkout*** behavior.

After setup this, always clean the directory.

#### Check truly clean

I use free style job.

Before I execute kitchen test.

I check there is no xml file under the test result directory.
If there is xml file.
Job is fail.

To avoid reuse previous test result.

# Using rspec_junit_formatter to output JUnit xml file.

To output result as XML(JUnit), I use

* [sj26/rspec_junit_formatter · GitHub](https://github.com/sj26/rspec_junit_formatter)

### get serverspec output result file from virtualbox

Sync virtual machine and host machine using synced_folder.

* [How to copy files from box to host machine? · Issue #716 · test-kitchen/test-kitchen · GitHub](https://github.com/test-kitchen/test-kitchen/issues/716)


# Serverspec2 and resource

* [Serverspec - Changes of Version 2](http://serverspec.org/changes-of-v2.html)

* [Serverspec - Resource Types](http://serverspec.org/resource_types.html)

## serverspec-init

Following command creates sample files.
It's useful for people who has some old knowledge.

```
serverspec-init
```
