---
description: Documentation for style consistency + tools used
---

# Introduction

## Flowchart

{% embed url="https://en.wikibooks.org/wiki/Programming\_Fundamentals/Flowcharts" %}

## IDE

![](../../../.gitbook/assets/image%20%282%29.png)

Historically, we had to use several software programs \(a text editor, a compiler, a linker, and operating system commands\) to make the conversion and run our program. However, today all those software programs with their associated tasks have been integrated into one program. However, this one program is really many software items that create an environment used by programmers to develop software. Thus the name: Integrated Development Environment or IDE.

IDEs available: Atom, VSCode, Eclipse, VIm, etc

IDE I'm using: VSCode \(as I'm on Windows, and it's free\)

## Version control

Refer to [GIT](../../git/)

I'm using: GIT + SourceTree

## Casing standards

{% embed url="https://en.wikibooks.org/wiki/Programming\_Fundamentals/Identifier\_Names" %}

Almost all programming languages and most coding shops have a standard code formatting style guide programmers are expected to follow. Among these are three common identifier casing standards:

* camelCase - each word is capitalized except the first word, with no intervening spaces
* PascalCase - each word is capitalized including the first word, with no intervening spaces
* snake\_case - each word is lowercase with underscores separating words
* kebab-case - each word is connected together with a dash-line just like a skewer.

C++, Java, and JavaScript typically use camelCase, with PascalCase reserved for libraries and classes. C\# uses primarily PascalCase with camelCase parameters. Python uses snake\_case for most identifiers. Kebab case is used in Perl 6 language, but it is also used in URLs of websites. In addition, the following rules apply:

* Do not start with an underscore \(used for technical programming/ private variables\)
* CONSTANTS IN ALL UPPER CASE \(often UPPER\_SNAKE\_CASE\)

These rules are decided on by the industry \(those who are using the programming language\).

{% hint style="info" %}
Different programming languages may have different casing standards. Refer to: [https://www.chaseadams.io/posts/most-common-programming-case-types/\#go-conventions](https://www.chaseadams.io/posts/most-common-programming-case-types/#go-conventions)
{% endhint %}

## ESLint/ Prettier



