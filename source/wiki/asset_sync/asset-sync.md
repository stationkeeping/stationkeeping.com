---
title: "Asset Sync"
---

Use the built-in-initializer to configure asset-sync. This means it pulls its parameters from environmental variables rather than a `config/initializer or a config file.

Add the following envs to Heroku using `$ heroku config:add XXXX=XXXX`

```
AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXXX
AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXX
FOG_DIRECTORY=XXXXXXX
FOG_PROVIDER=AWS
ASSET_SYNC_GZIP_COMPRESSION=true
FOG_REGION=eu-west-1
```

*If there are already files synced when you add `ASSET_SYNC_GZIP_COMPRESSION=true` you will need to delete everything from S3 and re-sync.*

*You must set '$ heroku labs:enable user-env-compile -a XXXXAPPNAMEXXXX` or it won't work and you'll get errors from Asset Sync regarding missing Fog Provider / Bucket *

