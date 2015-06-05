---
layout: post
title: Change postgresql port number in Galaxy Docker image
date: 2015-06-02T18:06:00
---

Galaxy Docker image default postgresql port is 5432.
If it uses another process, you can change your Galaxy Docker's postgresql port.

Change postgresql port from 5432(default) to 15555.
Pass two Environmental values when docker run.

```
docker run --rm -e PGPORT=15555 -e GALAXY_CONFIG_DATABASE_CONNECTION=postgresql://galaxy:galaxy@localhost:15555/galaxy -i -t -p 19002:9002 -p 18080:80 bgruening/galaxy-stable /bin/bash
```

bash in docker.
type following command to start galaxy.

```
startup
```

After the start galaxy.
port LISTEN status is following.

```
root@7b667f1d7c99:/galaxy-central# netstat -natp | grep LISTEN
tcp 0 0 0.0.0.0:8800 0.0.0.0:* LISTEN -
tcp 0 0 0.0.0.0:6817 0.0.0.0:* LISTEN -
tcp 0 0 127.0.0.1:4001 0.0.0.0:* LISTEN -
tcp 0 0 0.0.0.0:6818 0.0.0.0:* LISTEN 49/slurmd
tcp 0 0 127.0.0.1:15555 0.0.0.0:* LISTEN -
tcp 0 0 127.0.0.1:9191 0.0.0.0:* LISTEN -
tcp 0 0 127.0.0.1:9001 0.0.0.0:* LISTEN -
tcp 0 0 0.0.0.0:9002 0.0.0.0:* LISTEN 43/python
tcp 0 0 0.0.0.0:80 0.0.0.0:* LISTEN 48/nginx
tcp6 0 0 ::1:15555 :::* LISTEN -
tcp6 0 0 :::21 :::* LISTEN -
```
