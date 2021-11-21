# RoR

![](<../../../.gitbook/assets/image (162).png>)



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

### Routes

config/routes.rb

`rails routes`

`rails routes -c controller_name`

```bash
# Search with pattern
$ rails routes -g wishlist
#       Prefix Verb URI Pattern                   Controller#Action
# wishlist_user GET  /users/:id/wishlist(.:format) users#wishlist
```



creating static pages:

```
rails generate controller StaticPages home help
```

&#x20;The routes for the `home` and `help` actions in the Static Pages controller.`config/routes.rb`

```
Rails.application.routes.draw do
  get  'static_pages/home'
  get  'static_pages/help'
  root 'application#hello'
end
```

{% embed url="https://guides.rubyonrails.org/routing.html" %}

### &#x20;Layout

{% embed url="https://guides.rubyonrails.org/layouts_and_rendering.html" %}



```
<%= yield(:title) %> | Ruby on Rails Tutorial Sample App
```

This relies on the definition of a page title (using `provide`) in each view, as in

```
<% provide(:title, "Home") %>
<h1>Sample App</h1>
<p>
  This is the home page for the
  <a href="https://www.railstutorial.org/">Ruby on Rails Tutorial</a>
  sample application.
</p>
```

#### Helper



Listing 4.2: Defining a `full_title` helper.`app/helpers/application_helper.rb`

```
module ApplicationHelper

  # Returns the full title on a per-page basis.
  def full_title(page_title = '')
    base_title = "Ruby on Rails Tutorial Sample App"
    if page_title.empty?
      base_title
    else
      page_title + " | " + base_title
    end
  end
end
```

Now that we have a helper, we can use it to simplify our layout by replacing

```
<title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
```

with

```
<title><%= full_title(yield(:title)) %></title>
```

###

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
*   It’s worth noting that the `nil` object is special, in that it is the _only_ Ruby object that is false in a boolean context, apart from `false` itself. We can see this using `!!` (read “bang bang”), which negates an object twice, thereby coercing it to its boolean value:

    ```
    >> !!nil
    => false
    ```

    In particular, all other Ruby objects are _true_, even 0:

    ```
    >> !!0
    => true
    ```

    \




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



### Testing

#### When to test

When deciding when and how to test, it’s helpful to understand _why_ to test. In my view, writing automated tests has three main benefits:

1. Tests protect against _regressions_, where a functioning feature stops working for some reason.
2. Tests allow code to be _refactored_ (i.e., changing its form without changing its function) with greater confidence.
3. Tests act as a _client_ for the application code, thereby helping determine its design and its interface with other parts of the system.



In this context, it’s helpful to have a set of guidelines on when we should test first (or test at all). Here are some suggestions based on my own experience:

* When a test is especially short or simple compared to the application code it tests, lean toward writing the test first.
* When the desired behavior isn’t yet crystal clear, lean toward writing the application code first, then write a test to codify the result.
* Because security is a top priority, err on the side of writing tests of the security model first.
* Whenever a bug is found, write a test to reproduce it and protect against regressions, then write the application code to fix it.
* Lean against writing tests for code (such as detailed HTML structure) likely to change in the future.
* Write tests before refactoring code, focusing on testing error-prone code that’s especially likely to break.

In practice, the guidelines above mean that we’ll usually write **controller and model tests** first and integration tests (which test functionality across models, views, and controllers) second. And when we’re writing application code that isn’t particularly brittle or error-prone, or is likely to change (as is often the case with views), we’ll often skip testing altogether.



To run tests:

```
rails test
```

