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



### Create a model/ migration via CLI

{% embed url="https://guides.rubyonrails.org/command_line.html" %}

{% embed url="https://guides.rubyonrails.org/v3.2/migrations.html" %}

```
# scaffold for generating a bunch of stuff like controller, views, tests
rails generate scaffold User name:string email:string

# to undo the above, use:
rails destroy scaffold User

rails generate model User name:string email:string
# Usage:
#  bin/rails generate model NAME [field[:type][:index] field[:type][:index]] [options]

rails generate migration AddSectionToDocument
# If the migration name is of the form “AddXXXToYYY” or “RemoveXXXFromYYY”
# and is followed by a list of column names and types then a migration
# containing the appropriate add_column and remove_column statements
# will be created.
```

```
rails db:migrate # or rake db:migrate
rails db:rollback # if correcting mistake in migration

# To go all the way back to the beginning, we can use

rails db:migrate VERSION=0
# note that only 0 works as current convention uses datetimes



```



**Supported Types**

Active Record supports the following database column types:

* :binary
* :boolean
* :date
* :datetime
* :decimal
* :float
* :integer
* :primary\_key
* :string
* :text
* :time
* :timestamp

{% embed url="https://stackoverflow.com/questions/10301794/difference-between-rake-dbmigrate-dbreset-and-dbschemaload" %}

### Associations



Listing 2.15: A user has many microposts.`app/models/user.rb`

```
class User < ApplicationRecord
  has_many :microposts
end
```

Listing 2.16: A micropost belongs to a user.`app/models/micropost.rb`

```
class Micropost < ApplicationRecord
  belongs_to :user
  validates :content, length: { maximum: 140 }
end
```

### Console

```
rails console
```

### Syntax

```
class User < ApplicationRecord
# class User inherits from ApplicationRecord
```

{% embed url="https://github.com/rubocop/ruby-style-guide" %}

Ruby favors each rather than for loops: [https://github.com/rubocop/ruby-style-guide#no-for-loops](https://github.com/rubocop/ruby-style-guide#no-for-loops)

```
arr.each { |elem| puts elem }

# elem is not accessible outside each block
elem # => NameError: undefined local variable or method `elem'


[5, 10, 15, 20, 25, 30].each_with_index do |num, idx|
    puts idx
end

[1,2,3].select(&:even?)
```

* snake\_case naming, camelCase for class names
* `:symbol` indicates a symbol (immutable identifier)
* `@` instance var, `@@` class var
*   ternary:

    ```
    result = some_condition ? something : something_else
    ```



    ```
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```


* Prefer `case` over `if-elsif` when compared value is the same in each clause.
* `&&, ||, ! have higher precedence over and, or, not`



### Lint

```
rubocop -A
```



### Deploy simple heroku app

```
heroku login
heroku create
git push heroku main
bundle lock --add-platform x86_64-linux #if show platform error
heroku logs
heroku run rails db:migrate
```
