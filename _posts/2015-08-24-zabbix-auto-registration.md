---
layout: post
title: Zabbix Auto Registration
date: 2015-08-24T13:23:56
---
# Zabbix

When you install zabbix agent, auto registration on zabbix server.

## zabbix-server

### Setting at the zabbix server web
Configuration -> Action
"Event source" select choose "Auto registration"

```
Host name like foo
```

If zabbix agentd hostname starts with 'foo', after zabbix agentd connect to
 zabbix server it will be auto registration.

## zabbix-agentd

```
ServerActive=zabbix server
```
