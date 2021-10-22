---
description: Documentation for style consistency + tools used
---

# Preparation/ Setup

## Flowchart

{% embed url="https://en.wikibooks.org/wiki/Programming_Fundamentals/Flowcharts" %}

## IDE

![](<../../../.gitbook/assets/image (30).png>)

Historically, we had to use several software programs (a text editor, a compiler, a linker, and operating system commands) to make the conversion and run our program. However, today all those software programs with their associated tasks have been integrated into one program. However, this one program is really many software items that create an environment used by programmers to develop software. Thus the name: Integrated Development Environment or IDE.

IDEs available: Atom, VSCode, Eclipse, VIm, etc

IDE I'm using: VSCode (as I'm on Windows, and it's free)

## Version control

Refer to [GIT](../../git/)

**I'm using: GIT + SourceTree**

## Casing standards

{% embed url="https://en.wikibooks.org/wiki/Programming_Fundamentals/Identifier_Names" %}

Almost all programming languages and most coding shops have a standard code formatting style guide programmers are expected to follow. Among these are three common identifier casing standards:

* camelCase - each word is capitalized except the first word, with no intervening spaces
* PascalCase - each word is capitalized including the first word, with no intervening spaces
* snake\_case - each word is lowercase with underscores separating words
* kebab-case - each word is connected together with a dash-line just like a skewer.

C++, Java, and JavaScript typically use camelCase, with PascalCase reserved for libraries and classes. C# uses primarily PascalCase with camelCase parameters. Python uses snake\_case for most identifiers. Kebab case is used in Perl 6 language, but it is also used in URLs of websites. In addition, the following rules apply:

* Do not start with an underscore (used for technical programming/ private variables)
* CONSTANTS IN ALL UPPER CASE (often UPPER\_SNAKE\_CASE)

These rules are decided on by the industry (those who are using the programming language).

{% hint style="info" %}
Different programming languages may have different casing standards. Refer to: [https://www.chaseadams.io/posts/most-common-programming-case-types/#go-conventions](https://www.chaseadams.io/posts/most-common-programming-case-types/#go-conventions)
{% endhint %}

## Machine Set-Up (for Web Dev)

{% hint style="info" %}
2020 Update: I've given in to getting a macbook since set up is smoother on macs. Check [Mac Setup](mac-setup.md) for the new setup
{% endhint %}

On Windows:&#x20;

* Git for windows [https://gitforwindows.org/](https://gitforwindows.org)
* Chocolatey (Package manager) [https://chocolatey.org/install](https://chocolatey.org/install)
* npm (via `choco install`)
* node.js (via `choco install`)

On Mac:

*   get homebrew (package manager):

    ```
    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```

&#x20;Brew install the following:

* `nvm`
* `git`
* `npm`
* `node`

Additional for both OS: (node version manager)

```
npm install --global n
```

## Extensions

VSCode:

* [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)&#x20;
* [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)&#x20;
* [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
* [Auto Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)
* [Auto Close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)
* [Jest](https://marketplace.visualstudio.com/items?itemName=Orta.vscode-jest)
* [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)
* Quokka
* Git File History

Browser:

* [Octotree](https://www.octotree.io)
* React Dev Tool

## Bikeshedding&#x20;

![Source: Dilbert](<../../../.gitbook/assets/image (49).png>)

{% embed url="https://blog.echobind.com/integrating-prettier-eslint-airbnb-style-guide-in-vscode-47f07b5d7d6a" %}

### TLDR: Just download this config and get it over with

{% embed url="https://github.com/paulolramos/eslint-prettier-airbnb-react" %}

{% hint style="info" %}
The above config can be a tad noisy, so you can tweak it to your preferences and save the config files.
{% endhint %}



