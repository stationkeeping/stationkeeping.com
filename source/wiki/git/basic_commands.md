---
title: "Basic Commands"
---

Create a new git repo (in pwd):
```
$ git init
```

Show current staging status of all files in the repo:
```
$ git status
```

Show commit history:
```
$ git log
```

Show compact commit history with commit per line:
```
$ git log --oneline
```

Stage a file for commit:
```
$ git add example.txt
```

Stage all changed files for commit:
```
$ git add -A
```

Remove a staged file (The opposite of `$ git add`)
```
$ git reset example.txt
```

Remove a file called 'example.txt', deleting it from local filesystem:
```
$ git rm example.txt
```

Remove a file called 'example.txt', leaving in local file system:
```
$ git rm --cached example.txt
```

Show changes between files in working tree and previous commit
```
$ git diff
```

Show changes between staged files and previous commit
```
$ git diff -staged
```

Commit staged files:
```
$ git commit -m "Example commit message"
```

Commit staged files, adding any changed files that are tracked, but unstaged.
```
$ git commit -a -m "Example commit message"
```

Add a file to the previous commit:
```
$ git commit --amend -m "Example commit message"
```

Undo commit, returning committed files to staging:
```
$ git reset --soft HEAD^
```

Undo commit, discarding all changes:
```
$ git reset --hard HEAD^
```

Discard changes to file called 'example.html':
```
$ git checkout -- example.html
```

Add a remote repo as origin:
```
$ git remote add origin git@example.com:example/example.git
```

Push `master` branch to remote called `origin`, defaulting to origin in future:
```
$ git push -u origin master
```

Clone a pre-existing repository to a folder named after the respository:
```
$ git clone git@example.com:example/example.git
```

Clone a pre-existing repository to a folder named `example`:
```
$ git clone git@example.com:example/example.git example
```

List remotes:
```
$ git remote -v
```

Display the current branch:
```
$ git branch
```

Create a brach:
```
$ git branch example
```

Checkout the branch 'example':
```
$ git checkout example
```

Create and checkout a branch called example:
```
$ git checkout -b example
```

Merge a branch called 'example' with the 'master' branch:
```
$ git checkout master
$ git merge example
```

Delete a branch called 'example':
```
$ git branch -d example
```

Push local branch called 'example' to origin:
```
$ git push origin example
```

Show all remote branches:
```
$ git branch -r
```

Delete a remote branch called 'example':
```
$ git push origin :example
```

Show remote branch status (which are being tracked and which are stale):
```
$ git remote show origin
```

Clean up remote branches:
```
$ git remote prune origin
```

Show tags:
```
$ git tag
```

Create a tag called 'v1.0.0':
```
$ git tag -a 'v1.0.0' -m 'Version 1.0.0'
```

Push tags to origin:
```
$ git push --tags
```

Switch to a tag called '1.0.0':
```
$ git checkout '1.0.0'
```

Change remote url:

```
git remote set-url staging git@heroku.com:example.git
```