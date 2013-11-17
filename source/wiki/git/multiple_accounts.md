---
title: "Multiple Accounts"
---

## Multiple github account authentication using SSH

To manage multiple [github](http://www.github.com) accounts using multiple [ssh](http://en.wikipedia.org/wiki/Secure_Shell) keys:

* Create a config file in your home .ssh directory `vim ~/.ssh/config`
* Use the following as an example for two accounts:

        Host github_repos1
            HostName github.com
            User git
            IdentityFile "/Users/stationkeeping/.ssh/id_rsa1"
            TCPKeepAlive yes
            IdentitiesOnly yes

        Host github_repos2
            HostName github.com
            User git
            IdentityFile "/Users/stationkeeping/.ssh/id_rsa2"
            TCPKeepAlive yes
            IdentitiesOnly yes

* Add your rsa identities to the authentication agent: `ssh-add ~/.ssh/id_rsa1` etc.
* When adding your remote to the local repos ensure you use the **Host** name in replacement for git@github.com e.g. `git remote add origin github_repos1:stationkeeping/repos1.git`.
* Check the git remotes: `git remote -v`