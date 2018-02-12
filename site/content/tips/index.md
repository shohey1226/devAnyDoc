---
date: 2016-03-09T00:11:02+01:00
title: How to use terminal
weight: 10
---

## Open file easily

Use [peco](https://github.com/peco/peco).

```
# in .bashrc or .bash_profile
function pecodaf {
  devany -f $(find . -type f -o -path ./.git -prune  | peco)
}
```

## Use deavny command from the history with peco

```
function pecodah {
  $(history | grep devany | cut -c 8- | peco)
}
```


