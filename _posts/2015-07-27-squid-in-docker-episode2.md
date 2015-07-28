---
layout: post
title: Squid in docker episode 2
date: 2015-07-27T14:25:56
---

# Updated docker image

After I wrote  [earlier article - Squid in docker](/squid-in-docker/)

Image is updated

* [sameersbn/squid Repository | Docker Hub Registry - Repositories of Docker Images](https://registry.hub.docker.com/u/sameersbn/squid/)

* [sameersbn/docker-squid](https://github.com/sameersbn/docker-squid)


## Setup some variables

On updated image some environment variables is not used.

You can write those values to your configuration file.

## ORDER IS IMPORTANT (2016-07-28 updated)

***maximum_object_size*** must be put before ***cache_dir***

* [squid - Squid3 not caching larger files - Server Fault](http://serverfault.com/questions/596890/squid3-not-caching-larger-files)

### maximum_object_size

maximum object size set to 100 MB

```
maximum_object_size 100 MB
```

### cache_dir

10GB for cache on directory

```
cache_dir ufs /var/spool/squid3 10000 16 256
```

### If you do not want to cache repomd.xml

If you do not want to cache repomd.xml

```
# add for repomd.xml
acl repomd url_regex /repomd\.xml$
cache deny repomd
```



## Start new image

pull
```
docker pull sameersbn/squid:latest
```

port setting host is 13128, container is 3128

```
docker run --name squid --rm --privileged=true \
  --publish 3128:3128 \
  --volume $PWD/squid.conf:/etc/squid3/squid.conf \
  --volume $PWD/../squid/cache:/var/spool/squid3 \
  --volume $PWD/../squid/log:/var/log/squid3 \
  sameersbn/squid:latest
```
