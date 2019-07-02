# EmberJS Components

{% hint style="warning" %}
Components must have at least one dash in their name \(to prevent clashes with HTML element names\).
{% endhint %}

```text
ember generate component my-component-name
```

Two parts: component.js \(logic\) and component.hbs \(view\)

{% code-tabs %}
{% code-tabs-item title="app/templates/components/blog-post.hbs" %}
```text
<article class="blog-post">
  <h1>{{this.title}}</h1>
  <p>{{yield}}</p>
  <label for="title">Blog Title</label>
  <p>Edit title: {{input id="title" type="text" value=this.title}}</p>
</article>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### Usage:

{% code-tabs %}
{% code-tabs-item title="app/templates/index.hbs" %}
```text
{{#each model as |post|}}
  {{#blog-post title=post.title}}
    {{post.body}}
  {{/blog-post}}
{{/each}}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Defining a Component Subclass <a id="toc_defining-a-component-subclass"></a>

