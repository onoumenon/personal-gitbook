# Set up

After you git clone, you need to make sure ruby and rails version match with the repository.

To manage ruby version, use something like rvm. [https://github.com/rvm/rvm#installing-rvm](https://github.com/rvm/rvm#installing-rvm)

If starting anew, [https://www.icdsoft.com/en/kb/view/installing\_ruby\_on\_rails\_with\_rvm](https://www.icdsoft.com/en/kb/view/installing\_ruby\_on\_rails\_with\_rvm)

Follow rails best practices: [https://github.com/flyerhzm/rails\_best\_practices](https://github.com/flyerhzm/rails\_best\_practices)

Once rails and bundle are installed,&#x20;

{% embed url="https://www.codewithjason.com/understanding-rails-secrets-credentials/" %}

`./bin/webpack-dev-server` to set up webpacker

to run seed data\
`rake db:seed`

to add to db: \
`rails console`

`Admin.create(email: "huitian@siliconjungles.com", password: '12345678', password_confirmation: '12345678')`

(base on schema)

see postgres db records:

Open Postico

Create a new favorite:\
User: developers\
localhost: 5432\
db name based on database.yml

{% embed url="https://nithinbekal.com/posts/rake-db-reseed/" %}

