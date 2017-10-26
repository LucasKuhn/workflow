# Workflow and Tricks
GENERATE HELPERS
```
rails generate controller ModelsController index  --no-helper --no-assets --no-controller-specs --no-view-specs
rails generate migration CreateProducts name:string part_number:string
[has many has many](https://stackoverflow.com/questions/4381154/rails-migration-for-has-and-belongs-to-many-join-table)
```

GITHUB
```
# Push local master to github lucas-staging branch
git push origin master:lucas-staging
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
Check And Update Migrations
```
heroku run rake db:version --app enzo-staging
heroku run rake db:migrate --app enzo-staging
```

Check Remotes
```
git remote -v
```
Precompile Assets
```
RAILS_ENV=production bundle exec rake assets:precompile
git add public/assets (OR .)
git commit -m "compiled assets"
git push staging master
```

## RSPEC
Get Detailed Results
```
rspec -fd
```

## EMAIL
### Action Mailer
Quem envia os emails é um mailer chamado Enzo Mailer
```ruby 
# app/mailers/enzo_mailer.rb

class EnzoMailer < ActionMailer::Base
  default from: "atendimento@nordweg.com"

  def order_confirmation_to_retailer(user, order)
    @user = user
    @order = order
    mail(to: @user.email, subject: 'Seu pedido foi realizado com sucesso!')
  end
end
```
Como o envio é feito através do MailChip e ele tem acesso ao domínio @nordweg.com, basta mudar aqui a parte `default from`para alterar quem envia o email

#### Texto do email
Está em formato HTML, e se encontra em:
```
app/views/enzo_mailer/order_confirmation_to_retailer.html.erb
```  

### Para enviar um email
Basta chamar o método do mailer e adicionar .deliver 
```ruby 
EnzoMailer.order_confirmation_to_retailer(@user).deliver
```

### Configurações de STMP
```ruby 
# enzo/config/environments/production.rb

  config.action_mailer.smtp_settings = {
    :address              => "smtp.mandrillapp.com",
    :port                 => 587,
    :user_name            => Rails.application.secrets.mandrill_user_name,
    :password             => Rails.application.secrets.mandrill_api_key,
    :authentication       => "plain",
    :enable_starttls_auto => true
  }
```

### Email e senha
Ficam em uma Enviroment Variable que é ignorada pelo .gitignore em `config/secrets.yml`  

### Preview
Primeiro é preciso criar um método que faça o preview com um valor existente (ex: User.last)
```
# /spec/mailers/previews/enzo_mailer_preview.rb
  def retailer_order_mail_preview
    EnzoMailer.order_confirmation_to_retailer_preview(User.last,Order.last)
  end
  
```
Para ver o preview basta acessar pelo browser:
```
http://localhost:3000/rails/mailers/enzo_mailer/order_confirmation_to_retailer_preview
```
[Setup Guide!](https://launchschool.com/blog/handling-emails-in-rails)
