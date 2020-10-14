# React Native

Components: [https://reactnative.dev/docs/components-and-apis](https://reactnative.dev/docs/components-and-apis)



{% hint style="info" %}
With React Hooks, you should use functional components since they can support changing of states. Note: Hooks **don't work inside classes.**
{% endhint %}

* In JSX, JavaScript values are referenced with `{}`. 
* As a general rule, use props to configure a component when it renders. Use state to keep track of any component data that you expect to change over time.

## Rules of Hooks

{% embed url="https://reactjs.org/docs/hooks-rules.html" %}

* Only call hooks at the top level. Don't call them inside loops, conditions, or nested functions
* Only call hooks from React function components. Don't call from regular JS functions.

{% embed url="https://www.npmjs.com/package/eslint-plugin-react-hooks" %}



## State Hook

{% embed url="https://reactjs.org/docs/hooks-state.html" %}

```text
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);
  // count is the attribute, setCount the method and 0 is the initial state

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Previously, using a class to change state:

```text
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

**What does calling `useState` do?** It declares a “state variable”. Our variable is called `count` but we could call it anything else, like `banana`. This is a way to “preserve” some values between the function calls — `useState` is a new way to use the exact same capabilities that `this.state` provides in a class. Normally, variables “disappear” when the function exits but state variables are preserved by React.

**What do we pass to `useState` as an argument?** The only argument to the `useState()` Hook is the initial state. Unlike with classes, the state doesn’t have to be an object. We can keep a number or a string if that’s all we need. In our example, we just want a number for how many times the user clicked, so pass `0` as initial state for our variable. \(If we wanted to store two different values in state, we would call `useState()` twice.\)

**What does `useState` return?** It returns a pair of values: the current state and a function that updates it. This is why we write `const [count, setCount] = useState()`. This is similar to `this.state.count` and `this.setState` in a class, except you get them in a pair. If you’re not familiar with the syntax we used, we’ll come back to it [at the bottom of this page](https://reactjs.org/docs/hooks-state.html#tip-what-do-square-brackets-mean).

{% hint style="info" %}
You **don’t have to** use many state variables. State variables can hold objects and arrays just fine, so you can still group related data together. However, unlike `this.setState` in a class, updating a state variable always _replaces_ it instead of merging it.
{% endhint %}

## Effect Hook

{% embed url="https://reactjs.org/docs/hooks-effect.html" %}

You’ve likely performed data fetching, subscriptions, or manually changing the DOM from React components before. We call these operations “side effects” \(or “effects” for short\) because they can affect other components and can’t be done during rendering.

{% hint style="info" %}
It serves the same purpose as componentDidMount, componentDidUpdate, componentWillUnmount
{% endhint %}

### Effects without cleanup

Run some additional code after React has updated the DOM \(eg: network requests, manual DOM mutations, logging\).

```text
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

**What does `useEffect` do?** By using this Hook, you tell React that your component needs to do something after render. React will remember the function you passed \(we’ll refer to it as our “effect”\), and call it later after performing the DOM updates. In this effect, we set the document title, but we could also perform data fetching or call some other imperative API.

**Why is `useEffect` called inside a component?** Placing `useEffect` inside the component lets us access the `count` state variable \(or any props\) right from the effect. We don’t need a special API to read it — it’s already in the function scope. Hooks embrace JavaScript closures and avoid introducing React-specific APIs where JavaScript already provides a solution.

**Does `useEffect` run after every render?** Yes! By default, it runs both after the first render _and_ after every update. \(We will later talk about [how to customize this](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects).\) Instead of thinking in terms of “mounting” and “updating”, you might find it easier to think that effects happen “after render”. React guarantees the DOM has been updated by the time it runs the effects.

{% hint style="info" %}
Unlike `componentDidMount` or `componentDidUpdate`, effects scheduled with `useEffect` don’t block the browser from updating the screen. This makes your app feel more responsive. The majority of effects don’t need to happen synchronously. In the uncommon cases where they do \(such as measuring the layout\), there is a separate [`useLayoutEffect`](https://reactjs.org/docs/hooks-reference.html#uselayouteffect) Hook with an API identical to `useEffect`.
{% endhint %}

### Effects with Cleanup

Some effects require cleanup so we don't introduce a memory leak. Eg: we might want to set up a subscription to some external data source.

Specify how to "clean up" an effect by returning the clean up function is useEffect

```text
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

**Why did we return a function from our effect?** This is the optional cleanup mechanism for effects. Every effect may return a function that cleans up after it. This lets us keep the logic for adding and removing subscriptions close to each other. They’re part of the same effect!

**When exactly does React clean up an effect?** React performs the cleanup when the component unmounts. However, as we learned earlier, effects run for every render and not just once. This is why React _also_ cleans up effects from the previous render before running the effects next time. We’ll discuss [why this helps avoid bugs](https://reactjs.org/docs/hooks-effect.html#explanation-why-effects-run-on-each-update) and [how to opt out of this behavior in case it creates performance issues](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects) later below.

>

## Custom Hooks

Reuse of stateful logic between components. The state of each component is completely independent. Hooks are a way to reuse _stateful logic_, not state itself. In fact, each _call_ to a Hook has a completely isolated state — so you can even use the same custom Hook twice in one component.

```text
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

If a function’s name starts with ”`use`” and it calls other Hooks, we say it is a custom Hook. The `useSomething` naming convention is how our linter plugin is able to find bugs in the code using Hooks.



