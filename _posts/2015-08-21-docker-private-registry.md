---
layout: post
title: Setup Docker Private Registry
date: 2015-08-21T14:25:56
---
# Software and container Version
Update 2015-08-25

| Software(or container)| Version |
| ----- |:----:|
| docker | 1.8.1 |
| docker registry | 2 (2.1.1)|



# Run registry container

This command start docker registry v2.

```
docker run -d -p 5000:5000 --name registry registry:2
```

***-v*** or something to registry image permanent.

## Check which version regisry is running


After start registry, check which version of registry is running.

```
$ curl http://registryMachine-IP:5000/v0/
404 page not found
$ curl http://registryMachine-IP:5000/v1/
404 page not found
$ curl http://registryMachine-IP:5000/v2/
{}
$
```

Confirm v2 registry is running.

# Build image and push

## Dockerfile

install git and curl

Dockerfile

```
FROM ubuntu:trusty
RUN apt-get update ; apt-get install -y git curl ; apt-get clean
```

## Build

```
$ docker build -t registryMachine-IP:5000/test/testgit:0.1.0 .
$
```

## Push to registry

```
$ docker push registryMachine-IP:5000/test/testgit:0.1.0
The push refers to a repository [registryMachine-IP:5000/test/testgit] (len: 1)
c741cd2cc314: Image successfully pushed
8251da35e7a7: Image successfully pushed
e5855facec0b: Image successfully pushed
5bff21ba5409: Image successfully pushed
6071b4945dcf: Image successfully pushed
0.1.0: digest: sha256:e12c2898ef539e15cb24544065a37d1451bd6d938931a3f5113ab377236499ae size: 9499
$
```

## Pull from other host

```
$ docker pull registryMachine-IP:5000/test/testgit:0.1.0
Trying to pull repository registryMachine-IP:5000/test/testgit ... 0.1.0: Pulling from test/testgit
c741cd2cc314: Pull complete
6071b4945dcf: Already exists
5bff21ba5409: Already exists
e5855facec0b: Already exists
8251da35e7a7: Already exists
Digest: sha256:e12c2898ef539e15cb24544065a37d1451bd6d938931a3f5113ab377236499ae
Status: Downloaded newer image for registryMachine-IP:5000/test/testgit:0.1.0
$
```


## Pull but Fail

without insecure registry, pull will be fail

```
$ docker pull registryMachine-IP:5000/test/testgit:0.1.0
Error response from daemon: unable to ping registry endpoint https://registryMachine-IP:5000/v0/
v2 ping attempt failed with error: Get https://registryMachine-IP:5000/v2/: tls: oversized record received with length 20527
 v1 ping attempt failed with error: Get https://registryMachine-IP:5000/v1/_ping: tls: oversized record received with length 20527
```

## Setup --insecure-registry

I can't find official document about insecure regisrty format yet.

### CIDR
With CIDR setting, this style can work without port number.

```
--insecure-registry 192.168.0.0/16
```

### CIDR and port

Update: 2015-08-25 following is not work

Of course can setup port number

```
--insecure-registry 192.168.0.0/16:5000
```

This is working no port number

```
--insecure-registry 192.168.0.0/16
```



### host and port

```
--insecure-registry registryMachine-IP:5000
```

## After setup, Pull will be Success

```
$ docker pull registryMachine-IP:5000/test/testgit:0.1.0
0.1.0: Pulling from test/testgit
6071b4945dcf: Pull complete
5bff21ba5409: Pull complete
e5855facec0b: Pull complete
8251da35e7a7: Pull complete
c741cd2cc314: Pull complete
Digest: sha256:e12c2898ef539e15cb24544065a37d1451bd6d938931a3f5113ab377236499ae
Status: Downloaded newer image for registryMachine-IP:5000/test/testgit:0.1.0
```
