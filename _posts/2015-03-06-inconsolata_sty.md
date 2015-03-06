---
layout: post
title: inconsolata.sty problem with R CMD CHECK
---

When execute R CMD CHECK packagename, R says about missing inconsolata.sty .
To resolve it.
I do following command.

```
mkdir -p ~/texmf/tex/latex
mv inconsolata.sty ~/texmf/tex/latex/
mktexlsr ~/texmf
```

output

```
mktexlsr: Updating /home/manabu/texmf/ls-R...
mktexlsr: Done.
```
