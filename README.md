# Workflow and Tricks
GENERATE HELPERS
```
rails generate controller ModelsController index  --no-helper --no-assets --no-controller-specs --no-view-specs
rails generate migration CreateProducts name:string part_number:string
[has many has many](https://stackoverflow.com/questions/4381154/rails-migration-for-has-and-belongs-to-many-join-table)
```

GITHUB
```
# Push local master to github staging branch
git push origin master:staging
```

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
```
# Push local master to heroku staging
git push staging master
```
Server Things
```
heroku run rake db:version --app enzo-staging
heroku restart --app enzo-staging
```

Check Remotes
```
git remote -v
```
Precompile Assets
```
c refspec stagin
```

### RSPEC
Get Detailed Results
```
rspec -fd
```
