# RoR

![](<../../.gitbook/assets/image (162).png>)



### Create a rails app

```
rails new hello_app #to create new app 
# change gemfile as needed
bundle install #to install gems
rails server

```

Note: there's options for rails api:&#x20;

[https://guides.rubyonrails.org/api\_app.html](https://guides.rubyonrails.org/api\_app.html)

```
rails new app_name --api -T -d=postgresql
```

\


Allowing connections to the local web server.`config/environments/development.rb`

```
Rails.application.configure do
  .
  .
  .
  # Allow connections to local server.
  config.hosts.clear
end
```



### OR start from template

```
# OR use the below command to start from a template 
rails new hello_app \
  -d postgresql \
  -m https://raw.githubusercontent.com/mattbrictson/rails-template/main/template.rb
# change gemfile as needed
bundle install #to install gems
yarn start

```



### Create a model/ migration

```
rails generate scaffold User name:string email:string
# or
rails generate migration AddSectionToDocument
```

