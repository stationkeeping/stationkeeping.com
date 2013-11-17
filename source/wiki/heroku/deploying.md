---
title: "Deploying"
---

A typical deployment to staging might look like this:

Precompile assets locally:
```
$ rake assets:precompile
```

Add the updated manifest to Git
```
git add public/assets/manifest.yaml
```

Push the app to staging:
```
$ git push staging master
```
 
Transfer your development database to your staging database:

1. Dump your development database:
```
$ pg_dump -Fc --no-acl --no-owner -h localhost -U pedr example_development > example_staging.dump
```

2. Add `example_staging.dump` to Dropbox and get its public URL

3. Restore the staging database from your development version
```
$ heroku pgbackups:restore DATABASE 'https://dl.dropboxusercontent.com/u/316978/pulp_staging.dump' -a plp-s --confirm plp-s
```



