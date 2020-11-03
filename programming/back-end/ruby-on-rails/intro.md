# Intro

To create a new app:

```text
$ rails new blog
```



| File/Folder | Purpose |
| :--- | :--- |
| app/ | Contains the controllers, models, views, helpers, mailers, channels, jobs, and assets for your application.  |
| bin/ | Contains the rails script that starts your app and can contain other scripts you use to setup, update, deploy, or run your application. |
| config/ | Configure your application's routes, database, and more. This is covered in more detail in [Configuring Rails Applications](https://guides.rubyonrails.org/configuring.html). |
| config.ru | Rack configuration for Rack based servers used to start the application. For more information about Rack, see the [Rack website](https://rack.github.io/). |
| db/ | Contains your current database schema, as well as the database migrations. |
| Gemfile Gemfile.lock | These files allow you to specify what gem dependencies are needed for your Rails application. These files are used by the Bundler gem. For more information about Bundler, see the [Bundler website](https://bundler.io/). |
| lib/ | Extended modules for your application. |
| log/ | Application log files. |
| package.json | This file allows you to specify what npm dependencies are needed for your Rails application. This file is used by Yarn. For more information about Yarn, see the [Yarn website](https://yarnpkg.com/lang/en/). |
| public/ | The only folder seen by the world as-is. Contains static files and compiled assets. |
| Rakefile | This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Rails. Rather than changing `Rakefile`, you should add your own tasks by adding files to the `lib/tasks` directory of your application. |
| README.md | This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on. |
| storage/ | Active Storage files for Disk Service. This is covered in [Active Storage Overview](https://guides.rubyonrails.org/active_storage_overview.html). |
| test/ | Unit tests, fixtures, and other test apparatus. These are covered in [Testing Rails Applications](https://guides.rubyonrails.org/testing.html). |
| tmp/ | Temporary files \(like cache and pid files\). |
| vendor/ | A place for all third-party code. In a typical Rails application this includes vendored gems. |
| .gitignore | This file tells git which files \(or patterns\) it should ignore. See [GitHub - Ignoring files](https://help.github.com/articles/ignoring-files) for more info about ignoring files. |
| .ruby-version | This file contains the default Ruby version. |

Common commands

{% embed url="https://gist.github.com/mdang/95b4f54cadf12e7e0415" %}

### Notes from

[https://www.youtube.com/watch?v=B3Fbujmgo60&ab\_channel=TraversyMedia](https://www.youtube.com/watch?v=B3Fbujmgo60&ab_channel=TraversyMedia)

Use `--api` while setting up the app if you want RoR as the backend and use another FE framework.

[http://localhost:3000/rails/info/routes](http://localhost:3000/rails/info/routes) to show what routes you have

gem bundle is similar to npm packages convention to add 3rd party libraries

partials are indicated with `_` prefix





