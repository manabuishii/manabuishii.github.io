---
layout: post
title: Setup chef client and network interface
date: 2015-08-19T11:25:56
---

# Chef


## Download and install chef client

If you need to install specific version of chef client,

* [Chef omnibusインストーラーで特定のバージョンのchef-clientをインストールする - Qiita](http://qiita.com/tatsuya6502/items/39d7e23393c9f99571de)


```
$ CHEF_SOLO_VERSION=11.18.12
$ curl -L "https://www.getchef.com/chef/install.sh" | sudo bash -s -- -v $CHEF_SOLO_VERSION
```

in /etc/rc.local

```
if [ ! -e /usr/bin/chef-client ]; then
  curl -L "https://www.getchef.com/chef/install.sh" | sudo bash -s -- -v 11.18.12
fi
```


## Setup files in /etc/chef
create /etc/chef if missing.

```
mkdir /etc/chef
chmod 755 /etc/chef
```

put

```
validation.pem
client.rb
```

chmod 2 files

```
chmod 644 /etc/chef/validation.pem /etc/chef/client.rb
```

client.pem is not needed because it is coming from chef server.

client.pem will be put with mode 0600.


# Network
## Network setting eth1

eth0 setting is ok.
if eth1 is not prepare, you should prepare eth1.

## GSSAPI related

If ssh starts very slow, there are some reasons.

When your ssh server can't resolve client name with DNS, ssh starts very slow.

If you don't use GSSAPI ssh, you can disable it.
To disable GSSAPI, Update setting file,
```
/etc/ssh/sshd_config
```

Comment out (sample is Ubuntu 14.04 file)
```
# GSSAPI options
GSSAPIAuthentication yes
GSSAPICleanupCredentials yes
```
to
```
# GSSAPI options
#GSSAPIAuthentication yes
#GSSAPICleanupCredentials yes
```
* [GSSAPI を使用したユーザ認証](https://support.ssh.com/manuals/client-user/60J/userauth-gssapi.html)

# Clean

## Packages
clean package
```
yum -y clean all
```

or

```
apt-get clean
```
## History

```
export HISTSIZE=0
history -c
truncate -s 0 /root/.bash_history
```
