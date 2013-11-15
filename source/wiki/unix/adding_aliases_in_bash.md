---
title: "Adding Aliases In Bash"
---

### Termporary (In the current shell)

Basic format: 

`$ alias new_name='command to be performed'`

eg.

`$ be='bundle exec'` 

Then use:

`$ be some_gem_command'`

Or eg.

`$ alias go_to_example='cd ~/some/long/path/to/example'`

### Permanently (Across All Shells)

Open `~/.bash_profile` or the appropriate file(See [here](http://www.thegeekstuff.com/2008/10/execution-sequence-for-bash_profile-bashrc-bash_login-profile-and-bash_logout/) for information on .bash_profile / .bash_rc \ .profile precedence).

Add to a new line and save.

