# React Hooks

{% hint style="info" %}
With React Hooks, you should use functional components since they can support changing of states. Note: Hooks **don't work inside classes.**
{% endhint %}

* In JSX, JavaScript values are referenced with `{}`. 
* As a general rule, use props to configure a component when it renders. Use state to keep track of any component data that you expect to change over time.

## Rules of Hooks

{% embed url="https://reactjs.org/docs/hooks-rules.html" %}

* **Only call hooks at the top level. Don't call them inside loops, conditions, or nested functions**
* **Only call hooks from React function components/ custom hooks. Don't call from regular JS functions.**

\*\*\*\*

```text
 // 🔴 We're breaking the first rule by using a Hook in a condition
  if (name !== '') {
    useEffect(function persistForm() {
      localStorage.setItem('formData', name);
    });
  }
```

The `name !== ''` condition is `true` on the first render, so we run this Hook. However, on the next render the user might clear the form, making the condition `false`. Now that we skip this Hook during rendering, the order of the Hook calls becomes different:

```text
useState('Mary')           // 1. Read the name state variable (argument is ignored)
// useEffect(persistForm)  // 🔴 This Hook was skipped!
useState('Poppins')        // 🔴 2 (but was 3). Fail to read the surname state variable
useEffect(updateTitle)     // 🔴 3 (but was 4). Fail to replace the effect
```

**Instead, put the condition inside the function of useEffect**

```text
  useEffect(function persistForm() {
    // 👍 We're not breaking the first rule anymore
    if (name !== '') {
      localStorage.setItem('formData', name);
    }
  });
```



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

You can declare useEffect multiple times.

**Hooks let us split the code based on what it is doing** rather than a lifecycle method name. React will apply _every_ effect used by the component, in the order they were specified.

### Issue with previous class component lifecycle hooks method

**But what happens if the `friend` prop changes** while the component is on the screen? Our component would continue displaying the online status of a different friend. This is a bug. We would also cause a memory leak or crash when unmounting since the unsubscribe call would use the wrong friend ID. Forgetting to handle `componentDidUpdate` properly is a common source of bugs in React applications.

There is no special code for handling updates because `useEffect` handles them _by default_. It cleans up the previous effects before applying the next effects. To illustrate this, here is a sequence of subscribe and unsubscribe calls that this component could produce over time:

```text
// Mount with { friend: { id: 100 } } props
ChatAPI.subscribeToFriendStatus(100, handleStatusChange);     // Run first effect

// Update with { friend: { id: 200 } } props
ChatAPI.unsubscribeFromFriendStatus(100, handleStatusChange); // Clean up previous effect
ChatAPI.subscribeToFriendStatus(200, handleStatusChange);     // Run next effect

// Update with { friend: { id: 300 } } props
ChatAPI.unsubscribeFromFriendStatus(200, handleStatusChange); // Clean up previous effect
ChatAPI.subscribeToFriendStatus(300, handleStatusChange);     // Run next effect

// Unmount
ChatAPI.unsubscribeFromFriendStatus(300, handleStatusChange); // Clean up last effect
```

### Skip update

You can tell React to _skip_ applying an effect if certain values haven’t changed between re-renders. To do so, pass an array as an optional second argument to `useEffect`:

```text
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if count changes
```



> If you use this optimization, make sure the array includes **all values from the component scope \(such as props and state\) that change over time and that are used by the effect**. Otherwise, your code will reference stale values from previous renders. Learn more about [how to deal with functions](https://reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies) and [what to do when the array changes too often](https://reactjs.org/docs/hooks-faq.html#what-can-i-do-if-my-effect-dependencies-change-too-often).
>
> If you want to run an effect and clean it up only once \(on mount and unmount\), you can pass an empty array \(`[]`\) as a second argument. This tells React that your effect doesn’t depend on _any_ values from props or state, so it never needs to re-run. This isn’t handled as a special case — it follows directly from how the dependencies array always works.
>
> If you pass an empty array \(`[]`\), the props and state inside the effect will always have their initial values. While passing `[]` as the second argument is closer to the familiar `componentDidMount` and `componentWillUnmount` mental model, there are usually [better](https://reactjs.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies) [solutions](https://reactjs.org/docs/hooks-faq.html#what-can-i-do-if-my-effect-dependencies-change-too-often) to avoid re-running effects too often. Also, don’t forget that React defers running `useEffect` until after the browser has painted, so doing extra work is less of a problem.
>
> We recommend using the [`exhaustive-deps`](https://github.com/facebook/react/issues/14920) rule as part of our [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation) package. It warns when dependencies are specified incorrectly and suggests a fix.

###  <a id="next-steps"></a>

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

**A custom Hook is a JavaScript function whose name starts with ”`use`” and that may call other Hooks.**

```text
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {  
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

## Hooks API

{% embed url="https://reactjs.org/docs/hooks-reference.html" %}

### useState

**Functional updates**

If the new state is computed using the previous state, you can pass a function to `setState`. The function will receive the previous value, and return an updated value. Here’s an example of a counter component that uses both forms of `setState`:

```text
function Counter({initialCount}) {
  const [count, setCount] = useState(initialCount);
  return (
    <>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount(prevCount => prevCount - 1)}>-</button>
      <button onClick={() => setCount(prevCount => prevCount + 1)}>+</button>
    </>
  );
}
```

If your update function returns the exact same value as the current state, the subsequent rerender will be skipped completely.

**Lazy initial state**

The `initialState` argument is the state used during the initial render. In subsequent renders, it is disregarded. If the initial state is the result of an expensive computation, you may provide a function instead, which will be executed only on the initial render:

```text
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
```

### useEffect

**Cleaning up an effect**

Often, effects create resources that need to be cleaned up before the component leaves the screen, such as a subscription or timer ID. To do this, the function passed to `useEffect` may return a clean-up function. For example, to create a subscription:

```text
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    // Clean up the subscription
    subscription.unsubscribe();
  };
});
```

**Timing of effects**

Unlike `componentDidMount` and `componentDidUpdate`, the function passed to `useEffect` fires **after** layout and paint, during a deferred event.  For effects that are dependant on timing, use [`useLayoutEffect`](https://reactjs.org/docs/hooks-reference.html#uselayouteffect). It has the same signature as `useEffect`, and only differs in when it is fired.

**Conditionally firing an effect**

To conditionally fire an effect, pass a second argument to `useEffect` that is the array of values that the effect depends on:

```text
useEffect(
  () => {
    const subscription = props.source.subscribe();
    return () => {
      subscription.unsubscribe();
    };
  },
  [props.source],
);
```

You can pass an empty array if you want the effect to fire only once.

### useContext

```text
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);  return (    <button style={{ background: theme.background, color: theme.foreground }}>      I am styled by theme context!    </button>  );
}
```

