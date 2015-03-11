---
layout: post
title: apt-get install without any interaction
---
If you want to install some packages without any interaction  
maybe for automate install , you can do that with following.

```
DEBIAN_FRONTEND=noninteractive apt-get -y install med-bio
```

[How do I ask apt-get to skip any interactive post-install configuration steps?](http://serverfault.com/questions/227190/how-do-i-ask-apt-get-to-skip-any-interactive-post-install-configuration-steps)

I think *biomaj* and *med-config* require interaction.

Come to think of it, I saw this writing in Dockerfile.
