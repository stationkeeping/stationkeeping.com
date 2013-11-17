---
title: "Configuration"
---

Show all configuration settings:
```
git config --list
```

Open Git's config file in your editor::
```
git config --global --edit
```

Setup Username and email:
```
$ git config --global user.name 'Example Name'
$ git config --global user.email 'example@example.com'
```

Tell Git to use colours:
```
$ git config --global color.ui true
```

Get the current branch (and pwd) in the command prompt by adding this to `~/.bash_profile`:
```
parse_git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

export PS1="\[\033[00m\]\u@\h\[\033[01;34m\] \W \[\033[31m\]\$(parse_git_branch) \[\033[00m\]$\[\033[00m\] "
```

Setup a global `.gitignore`:
```
git config --global core.excludesfile '/Users/pedr/.gitignore_global'
```

Use Sublime Text as the default editor (Assuming it has been set in your `bash_profile` using `export EDITOR='subl -w'`:
```
git config --global core.editor 'subl -w'
```
