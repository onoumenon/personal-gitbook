# Set up testing

Add gems

```
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem 'factory_bot_rails'
  gem 'faker'
  gem 'rspec-rails'
end
```

```
$ bundle install
```

```
rails generate rspec:install
```

{% code title="spec > rails_helper.rb" %}
```
Dir[Rails.root.join('spec', 'support', '**', '*.rb')].each { |f| require f }
```
{% endcode %}

{% code title="spec>support>factory_bot.rb" %}
```
# frozen_string_literal: true

RSpec.configure do |config|
  config.include FactoryBot::Syntax::Methods
end

```
{% endcode %}

{% code title="spec>factories>user.rb" %}
```
FactoryBot.define do
  factory :user do
    name { Faker::Name.name }
    ...
  end
end

```
{% endcode %}

