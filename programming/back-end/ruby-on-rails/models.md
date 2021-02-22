# Models

{% embed url="https://www.youtube.com/watch?v=cuWoZw1D3oY" %}



* use rails generators to create models \(either `rails generate model` or `rails generate migration`\)
* `db:migrate` if using migration
* For user with registration, make sure to encrypt password
* you can use devise gem \(more things generated\) or bcrypt with secure\_password 
* [https://github.com/heartcombo/devise](https://github.com/heartcombo/devise)
* \([https://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html](https://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html)\)
* create the relevant routes for registration \(`rails routes` to check\)
* create the relevant controllers for those routes \(ie: registrations:create route should have a 'create' method in a registration controller class\)
* if using rails layouts/views, a link `get`and a form submit `post` by default. Prefer using button\_to for `delete` \(details: [https://cardoni.net/rails-button-to-vs-link-to-url-helpers/](https://cardoni.net/rails-button-to-vs-link-to-url-helpers/)\)
* avoid find and use find\_by if you don't want to throw errors [https://bigbinary.com/learn-rubyonrails-book/find-vs-find-by-vs-where-in-active-record\#find-by](https://bigbinary.com/learn-rubyonrails-book/find-vs-find-by-vs-where-in-active-record#find-by)
* use session cookies to store cookies for login \([https://guides.rubyonrails.org/security.html](https://guides.rubyonrails.org/security.html), [https://www.theodinproject.com/courses/ruby-on-rails/lessons/sessions-cookies-and-authentication](https://www.theodinproject.com/courses/ruby-on-rails/lessons/sessions-cookies-and-authentication)\)
* use generic `form.object.errors` to display errors in view  Additional resource: [https://bigbinary.com/learn-rubyonrails-book](https://bigbinary.com/learn-rubyonrails-book)

