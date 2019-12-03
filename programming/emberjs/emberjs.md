# EmberJS QuickStart

## Intro

Ember is a front-end framework that follows Ruby on Rails' _Convention over Configuration_ guiding principle. The advantage is that you will usually end up using best practices if you follow those conventions, and waste less time thinking about configuration. 

The emberCLI is a useful tool for meta-programming and fast prototyping, as it can generate files using blueprints. For instance, generating a component in the CLI includes the test file with accompanying starter tests, a component and a template file.

It was one of the big 3 back in 2012, but has been replaced by the new kids on the block. 



### Core Concepts

**Routes** - URLs on client is handled by router.js, renders templates + models

**Templates** - organizes the HTML \(handlebar templates\)

**Models** - the data layer

**Components** - logic for UI behaviour 

**Hooks** - component lifecycle hooks and route hooks



![Source: Jackie Luo&apos;s Talk &quot;From React to Ember&quot;](https://stuffihavelearnthome.files.wordpress.com/2019/06/ember-vs-react.png)

  

![Source: Ember&apos;s docs](https://guides.emberjs.com/images/ember-core-concepts/ember-core-concepts.png)

###  

![](../../.gitbook/assets/image%20%288%29.png)

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

### 

### Routes

To generate a route:

```text
ember g route [route-name]
```

Creates

* a template to be displayed
* a route object \(fetches model\)
* an entry in the router \(app/router.js\)
* a unit test for the route

### 

### Components and Hooks

Similar to React's concept of Components and Hooks.

A component consists of two parts:

* A template that defines how it will look \(`app/templates/components/rental-listing.hbs`\)
* A JavaScript source file \(`app/components/rental-listing.js`\) that defines how it will behave.

[Docs for Hooks](https://guides.emberjs.com/release/components/the-component-lifecycle/)

note: didRender \(is called during both render and re-render\)

```text
ember g component [dir/component-name]
```

### 

### Models

There should only be a single source of truth for the data.

Represents persistent state, saved to web server or local \(local storage\).

 To generate:

```text
ember g model [model-name]
```

It generates a model file and a test file.

To initiate a model object: 

```text
import DS from 'ember-data';

export default DS.Model.extend({
+  title: DS.attr(),
+  owner: DS.attr(),
+  city: DS.attr(),
+  category: DS.attr(),
+  image: DS.attr(),
+  bedrooms: DS.attr(),
+  description: DS.attr()
});
```

### 

### Help

_`ember help generate`_ to get a list of other 'generate' commands

Naming conventions: [Docs](https://guides.emberjs.com/v1.10.0/concepts/naming-conventions/)

In Ember, the browser URL is mapped to a route, which then rendered a template, and loads a model available to the template.

###  

### VSC extensions

Besides my usual, you can also installed these extensions for productivity:

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



### Handlebar Helpers

Handle helpers allow our users to quickly see if a property is "Standalone" or part of a "Community".

```text
ember g helper rental-property-type
```

 This creates 2 files \(a JavaScript file and a test file\):

```text
installing helper
  create app/helpers/rental-property-type.js
installing helper-test
  create tests/integration/helpers/rental-property-type-test.js
```

Now in the rental-listing component file:

```text
<span>Type:</span> {{rental-property-type this.rental.category}} - {{this.rental.category}}
```

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

### Integration Test

For components:

```text
import { module, test } from 'qunit';
import { setupRenderingTest } from 'ember-qunit';
import { render, click } from '@ember/test-helpers';
import hbs from 'htmlbars-inline-precompile';
import EmberObject from '@ember/object';


module('Integration | Component | rental listing', function (hooks) {
  setupRenderingTest(hooks);

  hooks.beforeEach(function () {
    this.rental = EmberObject.create({
      image: 'fake.png',
      title: 'test-title',
      owner: 'test-owner',
      type: 'test-type',
      city: 'test-city',
      bedrooms: 3
    });
  });

  test('should display rental details', async function(assert) {
    await render(hbs`<RentalListing @rental={{this.rental}} />`);
    assert.equal(this.element.querySelector('.listing h3').textContent.trim(), 'test-title', 'Title: test-title');
    assert.equal(this.element.querySelector('.listing .owner').textContent.trim(), 'Owner: test-owner', 'Owner: test-owner');
  });

  test('should toggle wide class on click', async function(assert) {
    await render(hbs`<RentalListing @rental={{this.rental}} />`);
    assert.notOk(this.element.querySelector('.image.wide'), 'initially rendered small');
    await click('.image');
    assert.ok(this.element.querySelector('.image.wide'), 'rendered wide after click');
    await click('.image');
    assert.notOk(this.element.querySelector('.image.wide'), 'rendered small after second click');
  });
});
```

For handlebar helper:

```text
test("it renders correctly for a Community rental", async function(assert) { 
    this.set("inputValue", "Apartment");
    await render(hbs`{{rental-property-typ inputValue}}`);
    assert.equal(this.element.textContent.trim(), "Community");
});
```

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

### 

### Input Helpers

[docs here](https://guides.emberjs.com/release/templates/input-helpers/)

```text
<label for="facebook">Facebook</label>
{{input id="facebook" value="http://www.facebook.com"}}
```

More complex version:

```text
{{input
  value=this.value
  key-up=(action "handleFilterEntry")
  class="light"
  placeholder="Filter By City"
}}
{{yield this.results}}
```



### Services

[docs here](https://guides.emberjs.com/release/tutorial/service/)



