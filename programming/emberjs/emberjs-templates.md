# EmberJS Templates

### Overview

There are two kinds of templates: Routes and Components

```text
ember generate component my-component-name
ember generate route my-route-name
```

Don't add css links in hbs files. Style rules should go in the `app/styles` directory instead.

A template only has access to data in its context.

{% code-tabs %}
{% code-tabs-item title="app/components/my-component.js" %}
```text
import Component from '@ember/component';

export default Component.extend({
  firstName: 'Trek',
  lastName: 'Glowacki',
  favoriteFramework: 'Ember'
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="app/templates/application.hbs" %}
```text
Hello, <strong>{{this.firstName}} {{this.lastName}}</strong>!
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Template

```text
<div>
  {{outlet}}
</div>

<!-- One way to use a component within a template -->
<MyComponent />

{{! Example of a comment that will be invisible, even
if it contains things in {{curlyBraces}} }}

```

### Component

```text
{{this.numberOfSquirrels}}

<!-- Some data passed down from a parent component
or controller -->
{{@weatherStatus}}

<!-- This button uses Ember Actions to make it interactive.
A method named `plantATree` is called when the button is
clicked. `plantATree` comes from the JavaScript file
associated with the template, like a Component or
Controller -->
<button onclick={{action 'plantATree'}}>
  More trees!
<button>

<!-- Here's an example of template logic in action.
If the `this.skyIsBlue` property is `true`, the text
inside will be shown -->
{{#if this.skyIsBlue}}
  If the skyIsBlue property is true, show this message
{{/if}}

<!-- You can pass a whole block of markup and handlebars
content from one component to another. yield is where
the block shows up when the page is rendered -->
{{yield}}
```

Lastly, it's important to know that arguments can be passed from one Component to another through 

{% code-tabs %}
{% code-tabs-item title="templates:app/templates/components/some-other-component.hbs" %}
```text
<MyComponent @favoriteFramework={{this.favoriteFramework}} />
```
{% endcode-tabs-item %}
{% endcode-tabs %}



### Helper functions

Takes in two types of arguments, `positional` \(an array of the positional values passed in the template\) or `named` \(an object of the named values passed in the template\), which are passed into the function, and should return a value. 

Ember gives you the ability to [write your own helpers](https://guides.emberjs.com/release/templates/writing-helpers/), and comes with some [helpers built-in](https://guides.emberjs.com/release/templates/built-in-helpers).

{% code-tabs %}
{% code-tabs-item title="app/helpers/sum.js" %}
```text
import { helper as buildHelper } from '@ember/component/helper';

export function sum(params) {
  return params[0] + params[1]
};

export const helper = buildHelper(sum);
```
{% endcode-tabs-item %}
{% endcode-tabs %}



{% code-tabs %}
{% code-tabs-item title="app/templates/application.hbs" %}
```text
<p>Total: {{sum 1 2}}</p>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### Helpers can be nested. Parenthesis used because curly braces cannot be nested.

```text
{{sum (multiply 2 4) 2}}
```

#### Get properties dynamically

```text
{{get this.address this.part}}
```

#### Nesting built-in helpers

```text
{{get "foo" (concat "item" this.index)}}
```

#### Conditionals \(if, unless\)

```text
<div class="is-car {{if this.isFast "zoooom" "putt-putt-putt"}}">
</div>
```

#### Can be nested

```text
<div>
  {{if this.isFast (if this.isFueled "zoooom")}}
</div>
```

#### Block invocation

```text
{{#if this.person}}
  Welcome back, <b>{{this.person.firstName}} {{this.person.lastName}}</b>!
{{/if}}
```

[`{{if}}`](https://www.emberjs.com/api/ember/release/classes/Ember.Templates.helpers/methods/if?anchor=if) checks for truthiness, which means all values except `false`, `undefined`, `null`, `''`, `0` or `[]` 

#### else:

```text
{{#if this.isAtWork}}
  Ship that code!
{{else if this.isReading}}
  You can finish War and Peace eventually...
{{/if}}
```

#### Iterator \(each\)

```text
<ul>
  {{#each this.people as |person|}}
    <li>Hello, {{person.name}}!</li>
  {{/each}}
</ul>
```

#### 2nd param = index

```text
<ul>
  {{#each this.people as |person index|}}
    <li>Hello, {{person.name}}! You're number {{index}} in line</li>
  {{/each}}
</ul>
```

#### Else for empty array

```text
{{#each this.people as |person|}}
  Hello, {{person.name}}!
{{else}}
  Sorry, nobody is here.
{{/each}}
```

#### Display keys in Object

```text
import Component from '@ember/component';

export default Component.extend({
  willRender() {
    // Set the "categories" property to a JavaScript object
    // with the category name as the key and the value a list
    // of products.
    this.set('categories', {
      'Bourbons': ['Bulleit', 'Four Roses', 'Woodford Reserve'],
      'Ryes': ['WhistlePig', 'High West']
    });
  }
});
```

#### each-in

```text
<ul>
  {{#each-in this.categories as |category products|}}
    <li>{{category}}
      <ol>
        {{#each products as |product|}}
          <li>{{product}}</li>
        {{/each}}
      </ol>
    </li>
  {{/each-in}}
</ul>
```



### Binding Element Attributes

```text
<div id="logo">
  <img src={{this.logoUrl}} alt="Logo">
</div>
```

```text
<input type="checkbox" disabled={{this.isAdministrator}}>
```



#### Adding Other Attributes \(Including Data Attributes\) <a id="toc_adding-other-attributes-including-data-attributes"></a>

Include this:

{% code-tabs %}
{% code-tabs-item title="app/components/link-to/component.js" %}
```text
import LinkComponent from '@ember/routing/link-component';

export default LinkComponent.extend({
  attributeBindings: ['data-toggle', 'lang']
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

So that this includes the data attr and lang:

```text
{{#link-to "photos" data-toggle="dropdown" lang="es"}}Fotos{{/link-to}}
```



#### link-to

{% code-tabs %}
{% code-tabs-item title="app/router.js" %}
```text
Router.map(function() {
  this.route('photos', function(){
    this.route('edit', { path: '/:photo_id' });
  });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="app/templates/photos.hbs" %}
```text
<ul>
  {{#each this.photos as |photo|}}
    <li>{{#link-to "photos.edit" photo}}{{photo.title}}{{/link-to}}</li>
  {{/each}}
</ul>
```
{% endcode-tabs-item %}
{% endcode-tabs %}



`{{link-to}}` component takes one or two arguments:

* The name of a route. In this example, it would be `index`, `photos`, or`photos.edit`.
* At most one model for each [dynamic segment](https://guides.emberjs.com/release/routing/defining-your-routes/#toc_dynamic-segments). By default, Ember.js will replace each segment with the value of the corresponding object's `id` property. In the example above, the second argument is each `photo` object, and the `id` property is used to fill in the dynamic segment with either `1`, `2`, or `3`. If there is no model to pass to the component, you can provide an explicit value instead

When the rendered link matches the current route, and the same object instance is passed into the component, then the link is given `class="active"`.



#### Example for Multiple Segments <a id="toc_example-for-multiple-segments"></a>

{% code-tabs %}
{% code-tabs-item title="app/router.js" %}
```text
Router.map(function() {
  this.route('photos', function(){
    this.route('photo', { path: '/:photo_id' }, function(){
      this.route('comments');
      this.route('comment', { path: '/comments/:comment_id' });
    });
  });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="app/templates/photo/index.hbs" %}
```text
<div class="photo">
  {{this.body}}
</div>

<p>{{#link-to "photos.photo.comment" this.primaryComment}}Main Comment{{/link-to}}</p>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

If you specify only one model, it will represent the innermost dynamic segment `:comment_id`. The `:photo_id` segment will use the current photo.  


### Setting query-params

```text
// Explicitly set target query params
{{#link-to "posts" (query-params direction="asc")}}Sort{{/link-to}}

// Binding is also supported
{{#link-to "posts" (query-params direction=this.otherDirection)}}Sort{{/link-to}}
```

#### Inline expression

```text
A link in {{#link-to "index"}}Block Expression Form{{/link-to}},
and a link in {{link-to "Inline Form" "index"}}.
```

#### You can add attributes to links

```text
{{link-to "Edit this photo" "photo.edit" this.photo class="btn btn-primary"}}
```

#### Replacing history entries <a id="toc_replacing-history-entries"></a>

```text
<p>
  {{#link-to "photo.comment" 5 this.primaryComment replace=true}}
    Main Comment for the Next Photo
  {{/link-to}}
</p>
```



### Actions

{% code-tabs %}
{% code-tabs-item title="app/templates/components/single-post.hbs" %}
```text
<h3><button {{action "toggleBody"}}>{{this.title}}</button></h3>
{{#if this.isShowingBody}}
  <p>{{this.body}}</p>
{{/if}}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



{% code-tabs %}
{% code-tabs-item title="app/components/single-post.js" %}
```text
import Component from '@ember/component';

export default Component.extend({
  actions: {
    toggleBody() {
      this.toggleProperty('isShowingBody');
    }
  }
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### Action Params

```text
<p><button {{action "select" this.post}}>✓</button> {{this.post.title}}</p>
```

```text
import Component from '@ember/component';

export default Component.extend({
  actions: {
    select(post) {
      console.log(post.get('title'));
    }
  }
});
```

#### Specify Events

```text
<p>
  <button {{action "select" this.post on="mouseUp"}}>✓</button>
  {{this.post.title}}
</p>
```

#### Allowing Modifier Keys

```text
<button {{action "anActionName" allowedKeys="alt"}}>
  click me
</button>
```

Action will be fire when alt is clicked.

### Allowing Default Browser Action <a id="toc_allowing-default-browser-action"></a>

By default action prevents default action.

```text
<a href="newPage.htm" {{action "logClick"}}>Go</a>
```

So if you want the link to still go to newPage.htm on click:

```text
<a href="newPage.htm" {{action "logClick" preventDefault=false}}>Go</a>
```

### Modifying the action's first parameter <a id="toc_modifying-the-actions-first-parameter"></a>

If a `value` option for the [`{{action}}`](https://www.emberjs.com/api/ember/release/classes/Ember.Templates.helpers/methods/action?anchor=action) helper is specified, its value will be considered a property path that will be read off of the first parameter of the action.

```text
<input type="text" value={{this.favoriteBand}} onblur={{action "bandDidChange"}} />
```

Using the `value` option modifies that behavior by extracting that property from the event object:

```text
<input type="text" value={{this.favoriteBand}} onblur={{action "bandDidChange" value="target.value"}} />
```

The `newValue` parameter thus becomes the `target.value` property of the event object, which is the value of the input field the user typed. 

### Attaching Actions to Non-Clickable Elements <a id="toc_attaching-actions-to-non-clickable-elements"></a>

Check with the accessibility [docs](https://guides.emberjs.com/release/reference/accessibility-guide/)



### Ember input and textarea

The [`{{input}}`](https://www.emberjs.com/api/ember/release/classes/Ember.Templates.helpers/methods/if?anchor=input) and [`{{textarea}}`](https://www.emberjs.com/api/ember/release/classes/Ember.Templates.helpers/methods/if?anchor=textarea) helpers in Ember.js are the easiest way to create common form controls with two-way bindings and can automatically update.  


```text
<label for="facebook">Facebook</label>
{{input id="facebook" value="http://www.facebook.com"}}
```

#### Input Dispatch Actions

```text
<label for="firstname">First Name</label>
{{input id="firstname" value=this.firstName key-press=(action "updateFirstName")}}
```

#### Checkboxes

```text
<label for="admin-checkbox">Is Admin?</label>
{{input id="admin-checkbox" type="checkbox" name="isAdmin" checked=this.isAdmin}}
```

#### Textareas

```text
{{textarea value=this.name cols="80" rows="6"}}
```



### Dev helpers

```text
{{log "Name is: " this.name}}
```

```text
{{#each this.items as |item|}}
  {{debugger}}
{{/each}}
```

```text
> get this.item.name
> context
```

#### Creating your own helpers

{% hint style="warning" %}
\(Note: Recommended to make helpers stateless, with no side effects, etc.\)
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="Example Currency Formatter" %}
```text
Your total is {{format-currency this.model.totalDue}}.
```
{% endcode-tabs-item %}
{% endcode-tabs %}

To generate a helper:

```text
ember generate helper format-currency
```

{% code-tabs %}
{% code-tabs-item title="app/helpers/format-currency.js" %}
```text
import { helper } from '@ember/component/helper';

export function formatCurrency([value, ...rest]) {
  let dollars = Math.floor(value / 100);
  let cents = value % 100;
  let sign = '$';

  if (cents.toString().length === 1) { cents = '0' + cents; }
  return `${sign}${dollars}.${cents}`;
}

export default helper(formatCurrency);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

To pass in multiple arguments:

```text
{{my-helper "hello" "world"}}
```

Named arguments

```text
{{format-currency 350 sign="£"}}
```

```text
import { helper } from '@ember/component/helper';

export function formatCurrency([value, ...rest], namedArgs) {
  let dollars = Math.floor(value / 100);
  let cents = value % 100;
  let sign = namedArgs.sign === undefined ? '$' : namedArgs.sign;

  if (cents.toString().length === 1) { cents = '0' + cents; }
  return `${sign}${dollars}.${cents}`;
});

export default helper(formatCurrency);
```

Destructuring named args:

```text
import { helper } from '@ember/component/helper';

export function myHelper(params, { option1, option2, option3 }) {
  console.log(option1); // => "hello"
  console.log(option2); // => "world"
  console.log(option3); // => "goodbye cruel world"
});

export default helper(myHelper);
```

In short, args for passing values, and hashes for passing options.

#### Class-based Helpers \(use only if needed\) <a id="toc_class-based-helpers"></a>

If you need helpers that access services, etc.

```text
import Helper from '@ember/component/helper';

export default Helper.extend({
  compute([value, ...rest], hash) {
    let dollars = Math.floor(value / 100);
    let cents = value % 100;
    let sign = hash.sign === undefined ? '$' : hash.sign;

    if (cents.toString().length === 1) { cents = '0' + cents; }
    return `${sign}${dollars}.${cents}`;
  }
});
```

#### Unescape html 

{% hint style="danger" %}
#### This can make the element VULNERABLE to XSS attacks!
{% endhint %}

By default, ember escapes HTML.

```text
export function makeBold([param, ...rest]) {
  return htmlSafe(`<b>${param}</b>`);
});
```

If you really need it with unsafe input \(eg: from user\):

```text
import Ember from 'ember';
import { helper } from '@ember/component/helper';
import { htmlSafe } from '@ember/string';

export function makeBold([param, ...rest]) {
  let value = Ember.Handlebars.Utils.escapeExpression(param);
  return htmlSafe(`<b>${value}</b>`);
});

export default helper(makeBold);
```



