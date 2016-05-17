---
layout: post
title: Speed up test-kitchen
date: 2015-03-18T04:06:00
---

To speed up test-kitchen process, using vagrant-cachier plugin.

* [fgrehm/vagrant-cachier](https://github.com/fgrehm/vagrant-cachier "fgrehm/vagrant-cachier")

vagrant-omnibus plugin also support vagrant-cachier plugin.

* [chef/vagrant-omnibus](https://github.com/chef/vagrant-omnibus "chef/vagrant-omnibus")

Read the articles

* [Speed-up Your Chef Test Kitchen Testing Cycle - I'm Ryan Bowlby](http://ryanbowlby.com/blog/2014/11/26/speed-up-your-chef-test-kitchen-testing-cycle/ "Speed-up Your Chef Test Kitchen Testing Cycle - I'm Ryan Bowlby")

* [Speedup Test Kitchen Vagrant Infrastructure Code Testing - ovais.tariq — ovais.tariq](http://www.ovaistariq.net/833/speedup-test-kitchen-vagrant-testing/)


Update(2015-03-19T14:48:00)

To cache vagrant-omnibus plugin's chef package.

```
provisioner:
  name: chef_solo
  chef_omnibus_install_options: ' -d /tmp/vagrant-cache/vagrant_omnibus'
```

And default all cache is created under **~/.vagrant.d/cache/**.

If you use cache per box setting, directories looks like following.

```
ls  ~/.vagrant.d/cache
opscode-centos-6.5  opscode-ubuntu-12.04
opscode-centos-7.0  opscode-ubuntu-14.04
```

and under the box directory

```
ls  ~/.vagrant.d/cache/opscode-centos-6.5
vagrant_omnibus  yum
```

**vagrant_omnibus** has chef packages.

**yum** has yum packages.


# Update 2016-05-17

* [Setting Up Your Private Supermarket Server | Chef Blog](https://www.chef.io/blog/2015/04/21/setting-up-your-private-supermarket-server/)

See ***Test kitchen is slow because it has to download/install the Chef Omnibus client package every time***

* [knife (zero) bootstrapで入れるChef Clientのバージョンを変える(test-kitchenも) - Qiita](http://qiita.com/marcy-terui/items/d3e5528a65280dc86f07)
