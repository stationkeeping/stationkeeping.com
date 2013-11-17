---
title: "Rebasing"
---

Rebasing is an alternative to conventional merging. Rather than merge all changes from one branch into another in a single monolithic commit, rebase plays back commits atomically and in sequence onto the other branch maintaining the granularity of the branch's commit history.


**You are changing the base/history of the current branch by inserting the changes of the branch used in the rebase before the current branch's changes. This means that the current branch is now in sync with the branch used to rebase up until it's own specific commits. So the last common commit between the branches is now more recent.**

#### Basic (Remote) Rebase

A simple `$ git rebase` will rebase the branch using the local version of the branch. If we are on the `master` branch, this will be `origin/master`. Rebase will store all changes on master since it diverged from origin/master. It will then play through all changes(commits) that occurred after the divergence found in `origin/master`, applying them to `master`. Once these have completed, it will then play back all the changes(commits) it has stored from `master`. So the sequence is:

1. Save all `master's` commits subsequent to split.
2. Starting at the split play back all commits on `origin/master`
3. Play back all `master's` commits stored in step 1.

#### Feature Branch Rebase

Rebasing a feature branch is slightly different, requiring an extra step to merge the feature branch back into master

1. Switch to the `feature` branch you want to merge into your `master` branch:
```
$ git checkout feature
```

2. Rebase using the `master` branch:
```
$ git rebase master
```

3. Switch to the `master` branch:
```
$ git checkout master
```

4. Merge the newly rebased `feature` branch with the `master` branch
```
$ git merge feature
```

#### Conflicts

Once a conflict is resolved (by editing the conflicted file), you need to mark the conflicted file as resolved using:
```
$ git add example.txt
```

Then tell rebase to continue:
```
$ git rebase --continue
```