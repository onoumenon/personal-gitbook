# EmberJS

Ember is a front-end framework that follows Ruby on Rails' Convention over Configuration guiding principle.

 

![Source: Jackie Luo&apos;s Talk &quot;From React to Ember&quot;](https://stuffihavelearnthome.files.wordpress.com/2019/06/ember-vs-react.png)

  

![Source: Ember&apos;s docs](https://guides.emberjs.com/images/ember-core-concepts/ember-core-concepts.png)

###  

### QuickStart

```text
npm install -g ember-cli
```

```text
ember new [directory name]
```

```text
cd [directory-name]
ember serve
```

To generate a route:

```text
ember g route [route-name]
```

Creates

* a template to be displayed
* a route object \(fetches model\)
* an entry in the router \(app/router.js\)
* a unit test for the route

```text
ember g component [component-name]
```

Help

_`ember help generate`_ to get a list of other 'generate' commands

Naming conventions: [Docs](https://guides.emberjs.com/v1.10.0/concepts/naming-conventions/)

In Ember, the browser URL is mapped to a route, which then rendered a template, and loads a model available to the template.

###  

### VSC extensions

Besides my usual, I also installed these extensions for productivity:

* handlebar-formatter
* Ember.js
* Ember JS \(ES6\) and Handlebars code snippets



### Notes on Handlebars

Ember uses [Handlebars](http://handlebarsjs.com/) templates

```text
{{expression}}
```

```text
{{{triple bars unescapes html tags}}}
```

**Block expressions**

```text
{{#list people}}{{firstName}} {{lastName}}{{/list}}
```

list is a helper function, and people is an object of persons with first and last names.

```text
Handlebars.registerHelper('list', function(items, options) {
  var out = "<ul>";

  for(var i=0, l=items.length; i<l; i++) {
    out = out + "<li>" + options.fn(items[i]) + "</li>";
  }

  return out + "</ul>";
});
```

**Handlebar paths can include `../`**

```text
<div id="comments">
  {{#each comments}}
  <h2><a href="/posts/{{../permalink}}#{{id}}">{{title}}</a></h2>
  <div>{{body}}</div>
  {{/each}}
</div>
```

**Comments**

```text
{{!-- only output author name if an author exists --}}
```

```text
{{! This comment will not be in the output }}
```

**Register Helpers**

```text
var context = {
  author: {firstName: "Alan", lastName: "Johnson"},
  body: "I Love Handlebars",
  comments: [{
    author: {firstName: "Yehuda", lastName: "Katz"},
    body: "Me too!"
  }]
};

Handlebars.registerHelper('fullName', function(person) {
  return person.firstName + " " + person.lastName;
});
```

```text
var context = {
  items: [
    {name: "Handlebars", emotion: "love"},
    {name: "Mustache", emotion: "enjoy"},
    {name: "Ember", emotion: "want to learn"}
  ]
};

Handlebars.registerHelper('agree_button', function() {
  var emotion = Handlebars.escapeExpression(this.emotion),
      name = Handlebars.escapeExpression(this.name);

  return new Handlebars.SafeString(
    "<button>I agree. I " + emotion + " " + name + "</button>"
  );
});
```

**Literals \(includes `true`,`false`, `null`, `undefined`\)**

```text
{{agree_button "My Text" class="my-class" visible=true counter=4}}
```

**Partials \(code reuse\)**

```text
{{> userMessage tagName="h2" }}
```

```text
Handlebars.registerPartial('userMessage',
    '<{{tagName}}>By {{author.firstName}} {{author.lastName}}</{{tagName}}>'
    + '<div class="body">{{body}}</div>');
```

```text
var context = {
  author: {firstName: "Alan", lastName: "Johnson"},
  body: "I Love Handlebars",
  comments: [{
    author: {firstName: "Yehuda", lastName: "Katz"},
    body: "Me too!"
  }]
};
```

[Handlebars Built-in Helpers: ](http://handlebarsjs.com/builtin_helpers.html)\(if, else, unless, with , each, lookup, log...\)

Anything that is valid Handlebars syntax is valid Ember syntax.

###  

### Models

Represents persistent state, saved to web server or local \(local storage\).

###  

### Components and Hooks

Similar to React's concept of Components and Hooks.

[Docs for Hooks](https://guides.emberjs.com/release/components/the-component-lifecycle/)

note: didRender \(is called during both render and re-render\)

###  

### Test

`ember g acceptance-test [test-name]`

Open the test file. It should have the boilerplate already set up. Ember uses QUnit for unit testing.

`setupApplicationTest` ensures that your Ember app is started and shut down between each test.

`ember test --server` to run tests with Testem test runner \(runs QUnit in Chrome\).

Some of the helpers we'll use commonly are:

* [`visit`](https://github.com/emberjs/ember-test-helpers/blob/master/API.md#visit) - loads a given URL
* [`click`](https://github.com/emberjs/ember-test-helpers/blob/master/API.md#click) - pretends to be a user clicking on a specific part of the screen
* [`currentURL`](https://github.com/emberjs/ember-test-helpers/blob/master/API.md#currenturl) - returns the URL of the page we're currently on

```text
import {
  click,
  currentURL,
  visit
} from '@ember/test-helpers';
```

### 

### AddOns

[Library here](https://emberobserver.com/)

Installation:

`ember install ember-cli-mirage`

`ember install ember-cli-babel`

###  

### Adapters

[Docs here](https://guides.emberjs.com/release/models/customizing-adapters)

Adapter determines how data is persisted to a backend data store. Things such as the backend host, URL format and headers used to talk to a REST API can all be configured in an adapter.

`ember generate adapter application`

