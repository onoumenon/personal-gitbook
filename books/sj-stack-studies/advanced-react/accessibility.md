# Accessibility

### Semantic HTML <a href="semantic-html" id="semantic-html"></a>

Use fragments so as not to break semantic html

```
function ListItem({ item }) {
  return (
    <>      <dt>{item.term}</dt>
      <dd>{item.description}</dd>
    </>  );
}
```

### Forms

Labels `for` is `htmlFor`

```
<label htmlFor="namedInput">Name:</label><input id="namedInput" type="text" name="name"/>
```

Errors\
[https://webaim.org/techniques/formvalidation/](https://webaim.org/techniques/formvalidation/)

### Focus control

* Don't remove blue focus outline
* Programatically manage focus (eg: when modal is closed)

### Test keyboard functionality

