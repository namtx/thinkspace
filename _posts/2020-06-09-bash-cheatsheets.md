---
layout: post
title: "bash cheatsheets"
description: "Collection of bash cheatsheets"
comments: true
keywords: "bash"
---

### for loop

```bash
for VARIABLE in 1 2 3 4 .. N
do
    # command 1
    # command 2
done
```

Bash **version 3.0+** supports built-in for setting up ranges:

```bash
for i in {1..5}
do
    # echo $i
done
```      

Bash **version 4.0+** supports for setting up a step value using `{START..END..INCREMENT}`

```bash
#!/bin/bash
echo "Bash version ${BASH_VERSION}"
for i in {0..10..2}
  do
      # echo $i
  done
```
