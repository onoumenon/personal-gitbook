# Make ruby gem

{% embed url="https://lokalise.com/blog/create-a-ruby-gem-basics/" %}

{% embed url="https://andrewm.codes/blog/automating-ruby-gem-releases-with-github-actions/" %}

Notes:

* {name}.gemspec is similar to package.json
* `spec.files` are the files downloaded for the gem (note that spec is removed)
* `spec.add_dependency` or `spec.add_development_dependency` accordingly, or use Gemfile
* version is inside lib > {name} > version.rb (use semantic ver)
* will need to import files manually (unlike rails), in lib > {name}.rb, add line `require "{name}/{filename}"`

Rake task

* In rails app using gem, use railtie
* in gem, import rake task file (`import "./lib/tasks/{name}.rake"`) used in `Rakefile`
* &#x20;create the rake file, call the services you need
