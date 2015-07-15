---
layout: post
title: Squid in docker
date: 2015-07-15T11:25:56
---

# Squid in docker

* [sameersbn/squid Repository | Docker Hub Registry - Repositories of Docker Images](https://registry.hub.docker.com/u/sameersbn/squid/)

* [sameersbn/docker-squid · GitHub](https://github.com/sameersbn/docker-squid)

## Squid

* [squid : Optimising Web Delivery](http://www.squid-cache.org/)



# Pull docker image

```
docker pull sameersbn/squid
```

## Setting file and volume

docker squid's git repository has example ***squid.conf***

```
git clone https://github.com/sameersbn/docker-squid.git
mkdir squiddata
mkdir squidsetting
cp docker-squid/squid.conf squidsetting
cd squidsetting
```

### Add your network

squid.conf has following entry in default settings.

```
# Example rule allowing access from your local networks.
# Adapt to list your (internal) IP networks from where browsing
# should be allowed
acl localnet src 10.0.0.0/8     # RFC1918 possible internal network
acl localnet src 172.16.0.0/12  # RFC1918 possible internal network
acl localnet src 192.168.0.0/16 # RFC1918 possible internal network
acl localnet src fc00::/7       # RFC 4193 local private network range
acl localnet src fe80::/10      # RFC 4291 link-local (directly plugged) machines
```

Add your network and customize if needed.

# Run docker squid

[sameersbn/docker-squid · GitHub](https://github.com/sameersbn/docker-squid)

 sample

run as daemon with name ***squid***

```
docker run --name=squid -d [OPTIONS] sameersbn/squid:latest
```

To specify own squid.conf.
This container run start script and it check whether there is /etc/squid3/squid.user.conf ,
if it exists , start script replace /etc/squid3/squid.conf with /etc/squid3/squid.user.conf

```
docker run --name=squid -d -v $PWD/squid.conf:/etc/squid3/squid.user.conf sameersbn/squid:latest
```

To use host volume for squid cache

```
docker run --name=squid -d -v $PWD/../squiddata:/var/spool/squid3 -v $PWD/squid.conf:/etc/squid3/squid.user.conf sameersbn/squid:latest
```

port setting host is 13128, container is 3128

```
docker run --name=squid -d -p 13128:3128 -v $PWD/../squiddata:/var/spool/squid3 -v $PWD/squid.conf:/etc/squid3/squid.user.conf sameersbn/squid:latest
```

remove after stop
but this option can not use with -d.

```
docker run --name=squid --rm -p 13128:3128 -v $PWD/../squiddata:/var/spool/squid3 -v $PWD/squid.conf:/etc/squid3/squid.user.conf sameersbn/squid:latest
```


if need privileged

```
docker run --name=squid --privileged=true --rm -p 13128:3128 -v $PWD/../squiddata:/var/spool/squid3 -v $PWD/squid.conf:/etc/squid3/squid.user.conf sameersbn/squid:latest
```

#### if you try to mount your squid.conf as /etc/squid3/squid.conf

It will ends with error.

```
$ docker run --name=squid --privileged=true  -
-rm -p 13128:3128 -v $PWD/../squiddata:/var/spool/squid3 -v $PWD/squid.conf:/etc
/squid3/squid.conf  sameersbn/squid:latest
sed: cannot rename /etc/squid3/sedMVOi5z: Device or resource busy
```

### Stop container

```
docker stop squid
```

### Login to debug squid container

```
docker -ti squid bash
```

# Squid does not cache

Sometimes squid do not cache the contents.
There are some reason.


## header

Almost case squid does not cache is host does not want to cache there contents.

In my case
After start squid try that squid works well.

Execute twice following command.
```
http_proxy=http://yourhost:yourport wget http://www.yahoo.co.jp
```

check /var/log/squid3/access.log

```
1436933211.370     94 xxx.xxx.xxx.xxx TCP_MISS/200 19463 GET http://www.yahoo.co.jp/ - HIER_DIRECT/183.79.23.158 text/html
1436933228.976     76 xxx.xxx.xxx.xxx TCP_MISS/200 19463 GET http://www.yahoo.co.jp/ - HIER_DIRECT/183.79.23.158 text/html
```

First line is ok.Because it is first time.
Second line is not goog. I want squid3 to cache.

Check header of http://www.yahoo.co.jp/

```
$ wget -S --spider http://www.yahoo.co.jp
Spider mode enabled. Check if remote file exists.
--2015-07-15 13:17:49--  http://www.yahoo.co.jp/
Resolving www.yahoo.co.jp (www.yahoo.co.jp)... 183.79.235.148, 183.79.227.90, 183.79.197.250, ...
Connecting to www.yahoo.co.jp (www.yahoo.co.jp)|183.79.235.148|:80... connected.
HTTP request sent, awaiting response...
  HTTP/1.1 200 OK
  Server: nginx
  Date: Wed, 15 Jul 2015 04:17:49 GMT
  Content-Type: text/html; charset=UTF-8
  Connection: close
  P3P: policyref="http://privacy.yahoo.co.jp/w3c/p3p_jp.xml", CP="CAO DSP COR CUR ADM DEV TAI PSA PSD IVAi IVDi CONi TELo OTPi OUR DELi SAMi OTRi UNRi PUBi IND PHY ONL UNI PUR FIN COM NAV INT DEM CNT STA POL HEA PRE GOV"
  Cache-Control: private, no-cache, no-store, must-revalidate
  Expires: -1
  Pragma: no-cache
  X-XRDS-Location: https://open.login.yahooapis.jp/openid20/www.yahoo.co.jp/xrds
  Vary: Accept-Encoding
  X-Frame-Options: SAMEORIGIN
Length: unspecified [text/html]
Remote file exists and could contain further links,
but recursion is disabled -- not retrieving.
```

[Yahoo! JAPAN](http://www.yahoo.co.jp/) do not want to cache contents.

So I try another site.
First check header.

```
$ wget -S --spider http://ftp.jaist.ac.jp/pub/Linux/debian/pool/main/x/x11-utils/x11-utils_7.7+3.tar.gz
Spider mode enabled. Check if remote file exists.
--2015-07-15 13:18:15--  http://ftp.jaist.ac.jp/pub/Linux/debian/pool/main/x/x11-utils/x11-utils_7.7+3.tar.gz
Resolving ftp.jaist.ac.jp (ftp.jaist.ac.jp)... 150.65.7.130, 2001:df0:2ed:feed::feed
Connecting to ftp.jaist.ac.jp (ftp.jaist.ac.jp)|150.65.7.130|:80... connected.
HTTP request sent, awaiting response...
  HTTP/1.1 200 OK
  Date: Wed, 15 Jul 2015 04:18:15 GMT
  Server: Apache/2.2.22 (Unix)
  Last-Modified: Sat, 02 May 2015 15:19:38 GMT
  ETag: "242654-291250-5151adb695680"
  Accept-Ranges: bytes
  Content-Length: 2691664
  Keep-Alive: timeout=2, max=100
  Connection: Keep-Alive
  Content-Type: application/x-gzip
Length: 2691664 (2.6M) [application/x-gzip]
Remote file exists.
```

There is no Cache-Control header.

And execute twice.

```
http_proxy=http://yourhost:yourport wget http://ftp.jaist.ac.jp/pub/Linux/debian/pool/main/x/x11-utils/x11-utils_7.7+3.tar.gz
```

First line is ok.
Second line is also good.

```
1436933590.366   1001 xxx.xxx.xxx.xxx TCP_MISS/200 2692053 GET http://ftp.jaist.ac.jp/pub/Linux/debian/pool/main/x/x11-utils/x11-utils_7.7+3.tar.gz - HIER_DIRECT/150.65.7.130 application/x-gzip
1436933597.721     57 xxx.xxx.xxx.xxx TCP_HIT/200 2692059 GET http://ftp.jaist.ac.jp/pub/Linux/debian/pool/main/x/x11-utils/x11-utils_7.7+3.tar.gz - HIER_NONE/- application/x-gzip
```

## size

### min size

minimum object size is set by minimum_object_size

* [squid : minimum_object_size configuration directive](http://www.squid-cache.org/Doc/config/minimum_object_size/)

```
Objects smaller than this size will NOT be saved on disk.  The
	value is specified in bytes, and the default is 0 KB, which
	means all responses can be stored.
```

### max size

max size is also problem.

This docker container can set max size with environment variables.

* [sameersbn/docker-squid · GitHub](https://github.com/sameersbn/docker-squid#configuration)
