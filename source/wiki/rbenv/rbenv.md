---
title: "Rbenv"
---

List Ruby versions available to rbenv:
```
$ rbenv install --list
```

If the versions are stale (latest versions missing), update rbenv's `ruby-build` plugin:
```
$ brew unlink ruby-build
$ brew install ruby-build
```