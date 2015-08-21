---
layout: post
title: Docker Swarm setting without docker machine
date: 2015-08-21T13:25:56
---


# Prerequisite

## Hosts

Prepare 2 hosts run docker daemon both host.
* swarmNode
* swarmManager

## Docker Host Port

Listen port 2375 by your docker daemon.

Add option to docker daemon configuration file.

```
-H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
```

After add option, restart docker daemon.

# Swarm

## Pull swarm container image

pull swarm image both hosts.

```
docker pull swarm:latest
```

## Generate token

On the swarmManager machine

It generate token

```
$ docker run --rm swarm create
xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
## execute swarm join

login swarmNode and execute following command

```
docker run -d swarm join --addr=swarmNode-IP:2375 token://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

login swarmManager and execute following command

```
docker run -d swarm join --addr=swarmManager-IP:2375 token://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

## execute swarm manage

After execute swarm join on 2 hosts.

login swarmManager and execute following command

```
$ docker run -d -p 12375:2375 swarm manage token://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
4b168532eb18f1f58b8d1e7231960f4a7a099d16645f077acf028fa8d6ceb8c3
```

### Success Case

check manager log.

If all goes well, log looks like this.

```
$ docker logs 4b168532eb18f1f58b8d1e7231960f4a7a099d16645f077acf028fa8d6ceb8c3
time="2015-08-11T02:19:00Z" level=info msg="Listening for HTTP" addr=":2375" proto=tcp
time="2015-08-11T02:19:01Z" level=info msg="Registered Engine swarmNode.yourDomain at swarmNode-IP:2375"
time="2015-08-11T02:19:02Z" level=info msg="Registered Engine swarmManager.yourDomain at swarmManager-IP:2375"
```

### Failure Case

If you do not open docker host port, log looks like this.

```
$ docker logs 4b168532eb18f1f58b8d1e7231960f4a7a099d16645f077acf028fa8d6ceb8c3
time="2015-08-10T06:26:26Z" level=info msg="Listening for HTTP" addr=":2375" pro
to=tcp
time="2015-08-10T06:30:08Z" level=error msg="Get http://swarmNode-IP:2375/v1.15
/info: dial tcp swarmNode-IP:2375: no route to host. Are you trying to connect
to a TLS-enabled daemon without TLS?"
time="2015-08-10T06:31:47Z" level=error msg="Get http://swarmManager-IP:2375/v1.15
/info: dial tcp swarmManager-IP:2375: no route to host. Are you trying to connect
to a TLS-enabled daemon without TLS?"
```
