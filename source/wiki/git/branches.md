---
title: "Branches"
---

Show local branches:
```
$ git branch
```

Show all branches:
```
$ git branch -a
```

Show remote branches:
```
$ git branch -r
```

To checkout a branch called 'example' that exists in your (local) remotes as 'origin/example':
```
$ git checkout example
```

To create and switch to a branch called 'example':
```
$ git checkout -b example
```

To push this branch to a remote:
```
$ git push origin example
```

However, whilst this will send your branch to the remote, allowing others to download it, *this will not link your local branch with the remote version of the branch*, and any changes made to this branch by others will *not* be automatically merged with your local version.

To link your local version to the the remote version:
```
$ git branch --set-upstream example origin/example
```

You can branch from any ref no matter how far back.