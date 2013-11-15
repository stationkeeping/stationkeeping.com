---
title: "Heroku Basics"
---

#### Basic Commands

Create app (initialise local Git repo first):

`$ heroku create --stack cedar`

Change the app name to something more memorable:

`$ heroku apps:rename newname`

Push app to heroku:

`$ git push heroku master`

Open the app in the browser:

`$ heroku open`

Check the app status/processes:

`$ heroku ps`

Run remote command on the Heroku shell:

`$ heroku run XXXX`

E.g.

`$ heroku run rake db:migrate`

Run the irb console on Heroku:

`$ heroku run console`

Or run bash:

`$ heroku run bash`

Drop the database

`heroku pg:reset DATABASE`

#### Keeping Environmental Variables in Sync

Keep all your environmental variables in a `.env` locally (**keep it out of version control**). Push to Heroku using:

```
heroku config:push
```

It is also possible to pull environmental variables from Heroku using:

```
heroku config:pull --overwrite --interactive
```

#### Custom domains

Add the domain to the app using:

`$ heroku domains:add example.com`
`$ heroku domains:add www.example.com`

Add [Zerigo addon](https://addons.heroku.com/zerigo_dns)

`$ heroku addons:add zerigo_dns`

Point nameservers to Zerigo's nameservers (nameservers might not show up as having changed for a long time):

```
a.ns.zerigo.net
b.ns.zerigo.net
c.ns.zerigo.net
d.ns.zerigo.net
e.ns.zerigo.net
```

Switch mail over using `Zerigo add-on > DNS > + addSnippet > Google Apps Email`. This will add MX records for Google Apps/Mail

Set up mail.example.com subdomain using `Zerigo add-on > DNS > + add` and Add:

```
Host: mail.example.com
Type: CNAME
Data: ghs.google.com
```

Choose

Verify changes:

Using Zerigo's tools found under `Zerigo add-on > DNS > Domains > Tools > Nameserver check`

Using 'host' (Depends on DNS propagation):

`$ host www.example.com`

**Currently Zerigo doesn't work with European instances**

To do the same thing without Zerigo, create 2 DNS records:

1. CNAME for www

> CNAME record named `www` with a value of `example.herokuapp.com`

2. A Record pointing to [wwwizer](http://wwwizer.com/naked-domain-redirect) which will redirect naked domains appropriately (no setup needed)

> Create a blank A Record pointing to 174.129.25.170 

174.129.25.170 is [wwwizer](http://wwwizer.com/naked-domain-redirect) which will redirect naked domains appropriately (no setup needed)

#### Set Timezone

Heroku uses the [tz database timezone format](http://en.wikipedia.org/wiki/List_of_tz_database_time_zones), so add the following as an environmental var:

```
$ heroku config:set TZ=Europe/London
```

#### Transferring Postgres Database to Heroku

For lots of detail see: https://devcenter.heroku.com/articles/pgbackups#importing_from_a_backup

**You need to install the pgbackups gem**

```
$ heroku addons:add pgbackups
```

You need to install 

1. Dump the development database

```
pg_dump -Fc --no-acl --no-owner -h localhost -U pedr example_database_name > example_database_name.dump
```

2. Place the dump file on a public server (Dropbox public folder works fine) eg.

```
'https://dl.dropboxusercontent.com/u/316978/example_database_name.dump'
```

3. Restore the remote database from the dump

```
$ heroku pgbackups:restore DATABASE 'https://dl.dropboxusercontent.com/u/316978/example_database_name.dump'
```

Note: If you aren't using `rake assets:precompile` locally, config vars need to be available during Heroku's build phase. This is enabled with:
```
$ heroku labs:enable user-env-compile --app example-app-name
```

You can reset the database (actually equivalent to dropping it) with:
```
$ heroku pg:reset HEROKU_POSTGRESQL_RED_URL -a plp-s
```