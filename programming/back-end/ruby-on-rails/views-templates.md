# Views/ templates

{% embed url="https://guides.rubyonrails.org/layouts\_and\_rendering.html" %}

### Yield

**3.2 Understanding yield**

Within the context of a layout, `yield` identifies a section where content from the view should be inserted. The simplest way to use this is to have a single `yield`, into which the entire contents of the view currently being rendered is inserted:

```text
<html>
  <head>
  </head>
  <body>
  <%= yield %>
  </body>
</html>
```

You can also create a layout with multiple yielding regions:

```text
<html>
  <head>
  <%= yield :head %>
  </head>
  <body>
  <%= yield %>
  </body>
</html>
```

### partials

will match 'views/shared/\_ad\_banner.html.erb:

```text
<%= render "shared/ad_banner" %>

<h1>Products</h1>

<p>Here are a few of our fine products:</p>

```

{% embed url="https://guides.rubyonrails.org/layouts\_and\_rendering.html\#using-partials" %}

### link\_to

Displays "About" as text, `about_path` is from `rails route` prefix

```text
<%= link_to "About", about_path, class:"nav-link" %>
```

{% embed url="https://www.rubyguides.com/2019/05/rails-link\_to-method/" %}

### flash messages

{% embed url="https://api.rubyonrails.org/classes/ActionDispatch/Flash.html" %}

{% embed url="https://www.rubyguides.com/2019/11/rails-flash-messages/" %}



