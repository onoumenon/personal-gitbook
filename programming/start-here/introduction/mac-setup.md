# Mac Setup

## Current config for Mac setup

{% hint style="info" %}
This is for my personal laptop, for Rails + React Native development
{% endhint %}

## Shell script for setting up RoR Environment

{% embed url="https://github.com/monfresh/laptop" %}

* [Bundler](http://bundler.io) for managing Ruby gems
* [chruby](https://github.com/postmodern/chruby) for managing [Ruby](https://www.ruby-lang.org/en/) versions (recommended over RVM and rbenv)
* [GitHub CLI](https://cli.github.com) brings GitHub to your terminal.
* [Heroku Toolbelt](https://toolbelt.heroku.com) for deploying and managing Heroku apps
* [Homebrew](http://brew.sh) for managing operating system libraries
* [Homebrew Cask](http://caskroom.io) for quickly installing Mac apps from the command line
* [Homebrew Services](https://github.com/Homebrew/homebrew-services) so you can easily stop, start, and restart services
* [Postgres](http://www.postgresql.org) for storing relational data
* [ruby-install](https://github.com/postmodern/ruby-install) for installing different versions of Ruby

You can run `laptop` on the terminal to update.

## Optimizing the Mac experience

Go through this to set up your mac settings (plus tools in your toolbox, continued below):

{% embed url="https://medium.com/better-programming/setting-up-your-mac-for-web-development-in-2020-659f5588b883" %}

## The Toolbox

### Terminal

Download [iTerm2](https://www.iterm2.com/downloads.html)

Download [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh) (with zsh-autosuggestions plugin)

`brew install git`

Download node version manager: [nvm](https://github.com/nvm-sh/nvm)

Download node (using `nvm install node`)

{% embed url="https://opensource.com/article/19/5/python-3-default-mac" %}

Download python version manager: [pyenv](https://github.com/pyenv/pyenv)

Download latest ver of python using pyenv&#x20;

Now that Python 3 is installed through pyenv, we want to set it as our global default version for pyenv environments:

```
$ pyenv global 3.7.3
# and verify it worked
$ pyenv version
3.7.3 (set by /Users/mbbroberg/.pyenv/version)
```

The power of pyenv comes from its control over our shell's path. In order for it to work correctly, we need to add the following to our configuration file (**.zshrc** for me, possibly **.bash\_profile** for you):

```
$ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

### VSCode for the IDE

Download VSCode with settings sync on&#x20;

{% embed url="https://code.visualstudio.com/docs/editor/settings-sync" %}

Extensions used

* Auto-close Tag
* Auto Import
* Auto Rename Tag
* Bracket Pair Colorizer
* ES7 React/Redux/GraphQL/React-Native
* ESLint
* Prettier
* GitLens
* Git Graph
* Git File History
* Jest
* React Native Tools
* YAML

## Setting up React Native environment

{% hint style="info" %}
Check the [docs](https://reactnative.dev/docs/0.61/getting-started) for complete instructions
{% endhint %}

Download Xcode

`brew install watchman`

`brew install yarn`



## Maintenence (WIP)

#### Delete cache/ files from Xcode, using shell script&#x20;

{% embed url="https://github.com/niklasberglund/xcode-clean.sh" %}

(https://ktatsiu.wordpress.com/2017/01/03/how-to-clean-up-tons-of-disk-space-from-xcode/,  https://developerinsider.co/clean-xcode-cache-quick-fix/)

#### Update ruby

enter command `laptop` in terminal

#### Clean up and upgrade casks in homebrew&#x20;

{% embed url="https://medium.com/@waxzce/keeping-macos-clean-this-is-my-osx-brew-update-cli-command-6c8f12dc1731" %}

#### Clean up unused versions and gems

\- Check and delete node versions (nvm)

\- Clean up gems (gem cleanup —dryrun)

\- Check and delete python versions (pyenv)

#### Clean up space&#x20;

{% embed url="https://www.cansurmeli.com/posts/a-developer-freeing-up-space-on-macos" %}

{% embed url="https://www.freecodecamp.org/news/how-to-free-up-space-on-your-developer-mac-f542f66ddfb" %}



