## Archived & Unmaintained 

Look at https://github.com/mimischi/dokku-sentry for a more up to date version.

# dokku-sentry

Deploy [Sentry](https://github.com/getsentry/sentry) 8.14+ on [dokku](http://dokku.viewdocs.io/dokku/) with official [postgres](https://github.com/dokku/dokku-postgres) and [redis](https://github.com/dokku/dokku-redis) plugins.

Forked from https://github.com/darklow/dokku-sentry

Instructions:

## Install dokku plugins

Go to your dokku server and install following plugins:


1) Install official **postgresql** plugin
```
sudo dokku plugin:install https://github.com/dokku/dokku-postgres.git postgres
```

2) Install official **redis** plugin
```
sudo dokku plugin:install https://github.com/dokku/dokku-redis.git redis

```

3) Install dokku letsencrypt plugin


## Prepare dokku


1) Create dokku app
```
ssh dokku@yourserver apps:create sentry
```

2) Create postgresql db and link it to the app
```
ssh dokku@yourserver postgres:create sentry
ssh dokku@yourserver postgres:link sentry sentry
```

3) Create redis instance and link it to the app
```
ssh dokku@yourserver redis:create sentry
ssh dokku@yourserver redis:link sentry sentry
```

4) Add SENTRY_CONF to env vars
```
ssh dokku@yourserver config:set sentry SENTRY_CONF=./
```

## Clone and deploy

1) Clone this repository locally
```
git clone https://github.com/sumukh/dokku-sentry.git
cd dokku-sentry
```

2) Add remote dokku
```
git remote add dokku dokku@yourserver:sentry
```

3) Deploy app
```
git push dokku
```

## Setup sentry

After you have sucessfully deployed app to dokku, run following commands to finish installing sentry:

1) Create database schema.

After migrations you will be prompted to create initial user.

```
ssh -t dokku@yourserver run sentry "sentry upgrade"
```

2) Create user (Optional). Use this step if you skipped it in the previous command.
```
ssh -t dokku@yourserver run sentry "sentry createuser"
```

**VOILÀ!** Open your dokku app url, login and enjoy Sentry!


## Customize sentry config

You can customise `sentry.conf.py` to fit your needs. However you can also override any config variable using dokku env vars. Use `SC_` prefix (as of Sentry Config) to override specific sentry config variables.


## Notes

To run sentry help and other commands:

```
ssh dokku@yourserver run sentry "sentry help"
```

To enable console prompt use `-t`

```
ssh -t dokku@yourserver run sentry "sentry"
```

If something goes wrong you can use dokku logs to debug:

```
ssh dokku@yourserver logs sentry -t
```
