# EmberJS Components

{% hint style="warning" %}
Components must have at least one dash in their name (to prevent clashes with HTML element names).
{% endhint %}

```
ember generate component my-component-name
```

Two parts: component.js (logic) and component.hbs (view)

{% code title="app/templates/components/blog-post.hbs" %}
```
<article class="blog-post">
  <h1>{{this.title}}</h1>
  <p>{{yield}}</p>
  <label for="title">Blog Title</label>
  <p>Edit title: {{input id="title" type="text" value=this.title}}</p>
</article>
```
{% endcode %}

#### Usage:

{% code title="app/templates/index.hbs" %}
```
{{#each model as |post|}}
  {{#blog-post title=post.title}}
    {{post.body}}
  {{/blog-post}}
{{/each}}
```
{% endcode %}

### &#x20;<a href="toc_defining-a-component-subclass" id="toc_defining-a-component-subclass"></a>
