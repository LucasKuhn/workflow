# Workflow and Tricks
Fix UNIX Socket
```
pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
```

### HEROKU
Get DB from Heroku Server
```
rake db:drop
rake db:create
rake db:migrate
heroku pg:backups capture --app enzomanager
curl -o latest.dump `heroku pg:backups public-url --app enzomanager`
pg_restore --verbose --clean --no-acl --no-owner -h localhost -U lucaskuhn -d enzo_development latest.dump
```

Check Remotes
```
git remote -v
```

### RSPEC
Get Detailed Results
```
rspec -fd
```
