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


#### Basic things to check

1. Make sure you have all its required envs set locally.

2. Make sure you have have set the Heroku labs feature up:
`$ heroku labs:enable user-env-compile -a myapp`

#### What's Going On Under The Hood

##### Heroku Precompiles

If you don't compile your assets locally and check the manifest into your repo, Heroku will assume you want it to precompile all your assets for you, so part of the push will involve compressing and transferring assets to Heroku. Once the assets are compiled, Asset Sync jumps in and uploads all the assets to S3. However all the original assets are left in the Slug, so it is very easy to go over the Slug limit.

Heroku does not keep its own manifest between slug compilations, so it will precompile everything every single time unless it detects a `public/assets/manifest.yml`in which case it will skip precompilation. Asset sync is more intelligent and will check for changed files to upload.

##### You precompile locally

You can precompile your assets locally using:
```
$ rake assets:precompile
```
Once the assets have been compiled, Asset Sync will upload all the assets to S3. If you don't want this process repeated when you push, you need to add `manifest.yml` to your repo. This way Heroku knows that all the assets have already been compiled, so it doesn't bother. This makes pushing a lot faster.

#### Notes

The pipeline fingerprints files based on content, so if you re-save a file or swap out a file for its duplicate, the fingerprint will not be updated.

You can use a `.slugignore` file to tell Heroku not to handle certain files or filetypes. For example a `.slugignore` file containing:
```
*.pdf
```
… will ignore all PDFs. If you have already precompiled a section of your assets and they won't change, this can speed up pushing considerably.

#### Debugging

To get better logging during precompilation *on Heroku*:

1. Use the `--trace` flag:

```
$ heroku assets:precompile --trace
```

2. Set logging to SDTOUT for the environment you want:

```
config.logger = Logger.new(STDOUT)
```