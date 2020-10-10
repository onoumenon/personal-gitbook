# React concepts

## State management

From the React tic tac toe tutorial:

> **To collect data from multiple children, or to have two child components communicate with each other, you need to declare the shared state in their parent component instead. The parent component can pass the state back down to the children by using props; this keeps the child components in sync with each other and with the parent component.**
>
> Lifting state into a parent component is common when React components are refactored — let’s take this opportunity to try it out.

Basically the idea is that instead of asking different children component individually for its state, it's better for those display components to be 'dumb' and for the parent to manage the state \(in this case, the state of how many x's and o's there are on the board\) and pass it down.

## Immutability

There are generally two approaches to changing data. The first approach is to _mutate_ the data by directly changing the data’s values. The second approach is to replace the data with a new copy which has the desired changes.

### Pros:

**Simplifying complex changes** \(eg: viewing history of tic-tac-toe game becomes simple because the variables still exists\)

**Detecting change** \(to detect changes, React just has to compare prev object and current object\)

**Re-rendering** \(ability to use PureComponents\)

{% hint style="info" %}
#### `React.PureComponent` <a id="reactpurecomponent"></a>

`React.PureComponent` is similar to [`React.Component`](https://reactjs.org/docs/react-api.html#reactcomponent). The difference between them is that [`React.Component`](https://reactjs.org/docs/react-api.html#reactcomponent) doesn’t implement [`shouldComponentUpdate()`](https://reactjs.org/docs/react-component.html#shouldcomponentupdate), but `React.PureComponent` implements it with a shallow prop and state comparison.

If your React component’s `render()` function renders the same result given the same props and state, you can use `React.PureComponent` for a performance boost in some cases.

> Note
>
> `React.PureComponent`’s `shouldComponentUpdate()` only shallowly compares the objects.
>
> Furthermore, `React.PureComponent`’s `shouldComponentUpdate()` skips prop updates for the whole component subtree. Make sure all the children components are also “pure”.
{% endhint %}

## Functional Components

Basically dumb components that does not have state can be expressed simply as functional components.

```text
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

## Keys in dynamic lists

 `key` is a special and reserved property in React \(along with `ref`, a more advanced feature\). When an element is created, React extracts the `key` property and stores the key directly on the returned element. Even though `key` may look like it belongs in `props`, `key` cannot be referenced using `this.props.key`. **React automatically uses `key` to decide which components to update**. A component cannot inquire about its `key`.



