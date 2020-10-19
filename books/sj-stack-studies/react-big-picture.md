# React Big Picture

{% embed url="https://reactjs.org/docs/thinking-in-react.html" %}

Get the feature brief \(a UI mock\), API method and sample response.

Then break it down into the different components:  
- Use [Single Responsibility](https://en.wikipedia.org/wiki/Single-responsibility_principle) Principle

![](../../.gitbook/assets/image%20%28155%29.png)

Arrange the components in a hierarchy

`FilterableProductTable`

* `SearchBar`
* `ProductTable`
  * `ProductCategoryRow`
  * `ProductRow`

Then build a static version in react \(ie: without state\).

Determine what needs state:

1. Is it passed in from a parent via props? If so, it probably isn’t state.
2. Does it remain unchanged over time? If so, it probably isn’t state.
3. Can you compute it based on any other state or props in your component? If so, it isn’t state.

In the example, the states are:

* The search text the user has entered
* The value of the checkbox

{% hint style="info" %}
**All React components must act like pure functions with respect to their props.**
{% endhint %}

### The Data Flows Down <a id="the-data-flows-down"></a>

This is commonly called a “top-down” or “unidirectional” data flow. Any state is always owned by some specific component, and any data or UI derived from that state can only affect components “below” them in the tree.

### Handling events

Handling events with React elements is very similar to handling events on DOM elements. There are some syntax differences:

* React events are named using camelCase, rather than lowercase.
* With JSX you pass a function as the event handler, rather than a string.

For example, the HTML:

```text
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

is slightly different in React:

```text
<button onClick={activateLasers}>  Activate Lasers
</button>
```

Another difference is that you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly.

```text
function ActionLink() {
  function handleClick(e) {    e.preventDefault();    console.log('The link was clicked.');  }
  return (
    <a href="#" onClick={handleClick}>      Click me
    </a>
  );
}
```

## Lists and Keys

Rendering multiple components

```text
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>  <li>{number}</li>);
```

### Keys <a id="keys"></a>

Keys help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity:

```text
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li key={number.toString()}>    {number}
  </li>
);
```

Keys used within arrays should be unique among their siblings. However they don’t need to be globally unique. We can use the same keys when we produce two different arrays.



