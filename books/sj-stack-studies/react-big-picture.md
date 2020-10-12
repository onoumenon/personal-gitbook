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



