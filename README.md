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

## HEROKU
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

## RSPEC
Get Detailed Results
```
rspec -fd
```

## EMAIL
#### Ações
ficam em uma classe EnzoMailer em `app/mailers/enzo_mailer.rb`  
#### Para ativar a ação
precisa chamar o método da classe + deliver, ex: `EnzoMailer.sample_email(@user).deliver`  
#### Texto do email
fica em um arquivo .html.erb em `app/views/enzo_mailer/sample_email.html.erb`  
#### Email e senha
ficam em uma Enviroment Variable que é ignorada pelo .gitignore em `config/environments/production.rb`  
### Preview
```
# Criar o preview method que pega uma variavel
# /spec/mailers/previews/enzo_mailer_preview.rb
  def retailer_order_mail_preview
    EnzoMailer.retailer_order_email(User.find(343),Order.last)
  end
  
# Para acessar
http://localhost:3000/rails/mailers/enzo_mailer/retailer_order_mail_preview
```

Mais info: https://launchschool.com/blog/handling-emails-in-rails
