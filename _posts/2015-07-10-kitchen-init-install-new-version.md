---
layout: post
title: kitchen init command install test-kitchen new version
---

# kitchen init install test-kitchen new version ?

I use kitchen 1.3.0

Today(2015-07-10) I typed following command
```
kitchen init --driver=kitchen-vagrant
```

result is

```
      create  .kitchen.yml
      create  test/integration/default
      create  .gitignore
      append  .gitignore
      append  .gitignore
Fetching: test-kitchen-1.4.1.gem (100%)
Successfully installed test-kitchen-1.4.1
Fetching: kitchen-vagrant-0.18.0.gem (100%)
Successfully installed kitchen-vagrant-0.18.0
Parsing documentation for kitchen-vagrant-0.18.0
Installing ri documentation for kitchen-vagrant-0.18.0
Parsing documentation for test-kitchen-1.4.1
Installing ri documentation for test-kitchen-1.4.1
Done installing documentation for kitchen-vagrant, test-kitchen after 1 seconds
2 gems installed
```

# check kitchen version and init help

kitchen version

```
$ kitchen version
Test Kitchen version 1.4.1
```

kitchen init help

```
$ kitchen help init
Usage:
  kitchen init

Options:
  -D, [--driver=one two three]                   # One or more Kitchen Driver gems to be installed or added to a Gemfile
                                                 # Default: kitchen-vagrant
  -P, [--provisioner=PROVISIONER]                # The default Kitchen Provisioner to use
                                                 # Default: chef_solo
      [--create-gemfile], [--no-create-gemfile]  # Whether or not to create a Gemfile if one does not exist. Default: false

Description:
  Init will add Test Kitchen support to an existing project for convergence integration testing. A default .kitchen.yml file (which is intended to be customized) is created in the project's root directory and one or more gems will be added to the project's Gemfile.
```
