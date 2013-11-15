---
title: "PostgreSQL Basics"
---

- brew update
- brew install postgresql
- initdb /usr/local/var/postgres
- cp /usr/local/Cellar/postgresql/(your-pg-version)/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/
- launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
- pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
- echo 'export PGHOST=localhost' >> ~/.bash_profile
