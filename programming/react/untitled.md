# React Intro

## React

React is a Javascript library for UI/ frontend for web apps. (There's also React Native for building mobile apps, which we will get to later.

#### &#x20;

### **Why use React?**

**Faster renders:**

DOM operations can be unwieldy and slow in pure JS. React sidesteps this problem by having a virtual DOM that uses a [diffing algorithm](https://reactjs.org/docs/reconciliation.html) under the hood, which updates only necessary parts of the DOM tree that have changed.

React does this by implementing a heuristic algorithm base on two assumptions:

1. Two elements of different types will produce different trees.
2. The developer can hint at which child elements may be stable across different renders with a `key` prop.

#### **Modular:**

React introduces components, which are reusable units of UI with content that can be modified by passing them props (short for properties). For instance, a 'card' or 'modal' component can be created once and used throughout the site. This also means that web design remains consistent throughout, and that unit tests are easier to write.

**Easy to learn:**

Your mileage may vary. Having little to no experience in jQuery and AJAX before this, the concepts were foreign to me. Compared to Ember or Angular, React is NOT a framework. Frameworks are more opinionated. Even so, it was easy to get things started with Create React App's boilerplate. It is also the most popular amongst the Vue/ Angular/ React trio and has the highest user retention.

#### &#x20;

## **Drawbacks**

**Longer initial load time:**

Because React loads all its component on the first load, it has a slow initial load. You don't have to use React throughout a huge, sprawling website, and you can also do [code splitting](https://webpack.js.org/guides/code-splitting/). Here's an article on how to solve [this](https://hackernoon.com/improving-first-time-load-of-a-production-react-app-part-1-of-2-e7494a7c7ab0).

**Dependency on third-party libraries:**

React, compared to Angular, is lightweight as it is an UI kit, but does not provide a complete solution. Therefore, devs usually rely on 3rd-party libraries for some functionalities.\[caption id="" align="alignnone" width="954"]![](https://stuffihavelearnthome.files.wordpress.com/2019/02/ef521-1c5r29xh4icipqkfein9w0a.png) Source: _Cleveroad_\[/caption]

For more information on the pros/ cons of React: [visit here](https://medium.com/@hamzamahmood/advantages-of-developing-modern-web-apps-with-react-js-8504c571db71).

### ****

### **Prerequisites: JS ES6**

JS fundamentals you need: [https://www.robinwieruch.de/javascript-fundamentals-react-requirements/](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/)\
1\. JS classses\
2\. Arrow functions for handlers ([https://frontarm.com/james-k-nelson/when-to-use-arrow-functions/](https://frontarm.com/james-k-nelson/when-to-use-arrow-functions/))

```
// JavaScript ES6 arrow function
const Greeting = (props) => {
  return

{props.greeting}

; } // JavaScript ES6 arrow function without body and implicit return const Greeting = (props) =>

{props.greeting}
```

React **function** component: stateless\
React **class** components: stateful

3\. .map(), .reduce(), .filter()

```
class App extends Component {
  render() {
    var users = [
      { name: 'Robin' },
      { name: 'Markus' },
    ];

    return (
{users.map(user =>{user.name})}
); } }
```

4\. Ternary, && operators

You can't use if-else in JSX, but you can use ternary and && operators :

```
 return (

{ showUsers ? ( {users.map(user =>{user.name})} ) : ( null ) }
{ showUsers && ({users.map(user =>{user.name})} ) }
```

****

### **Planning before diving in:**

\
Step 1: Draw the browser DOM representation to determine components

![Image result for react components planning](https://stuffihavelearnthome.files.wordpress.com/2019/02/bc04a-1rta4nf2pi\_\_vcarqgxeebg.png)

Step 2: Draw the React components tree, decide whether you need Redux

![Related image](https://stuffihavelearnthome.files.wordpress.com/2019/02/8f912-187dj5eb3ydd7\_abhkb4uoq.png)

Step 3: Code your way up/down the tree ****&#x20;

Note: Hierarchy of components matters. For sibling components to share data, states and handlers should be in a parent, and the parent can't affect child component if it already has its own state.



#### Additional Resources:

[https://hackernoon.com/how-and-why-to-use-d3-with-react-d239eb1ea274](https://hackernoon.com/how-and-why-to-use-d3-with-react-d239eb1ea274)

[https://itnext.io/using-advanced-design-patterns-to-create-flexible-and-reusable-react-components-part-1-dd495fa1823](https://itnext.io/using-advanced-design-patterns-to-create-flexible-and-reusable-react-components-part-1-dd495fa1823)
