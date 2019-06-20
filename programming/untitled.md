# React Redux

### Why Redux?

Redux is a [state management tool](https://blog.logrocket.com/why-use-redux-reasons-with-clear-examples-d21bffd5835). In a simple React app, the state is handled in the React components themselves, but things get messy as the app expands. You could let the parent component like App.js handle all the state, but that means multiple levels of prop drilling.

Redux solves that by having the state stored in a single location, and passing each component exactly what they need.

### Redux vs React Context

Under the hood, since 2015, Redux is using the old React Context's Provider API. Instead of using it to propagate state, Redux, Apollo, and React-Router use it to provide observables \(objects with functions that subscribe to changes\).

The reason is that the old Provider API made it difficult to propagates updates past `shouldComponentUpdate()` or `PureComponent.`

The new updates to the API \(ver 16.3.0\) made the API official and safe to use in production, and introduced some changes to how the API works \([more info here](https://frontarm.com/james-k-nelson/when-context-replaces-redux/)\). But the tldr is that it is now safe to use it to propagate state.

Essentially, if you only want to performantly get data from a single store, then the new context API lets you build apps _without_ Redux.

But if you want to use Redux's dev tools \(like the time-travel debugger\), or use middlewares \(redux-thunk, redux-saga\), then redux would still be relevant.

The other thing to note is [performance optimization](https://frontarm.com/james-k-nelson/react-context-performance/). React Context does not do any of the performance optimizations for you.

### Redux setup

\[caption id="" align="alignnone" width="1501"\]![redux architecture](http://bitcom.systems/blog/img/using-redux-in-angular/using-redux-in-angular-diagram.png) source: [http://bitcom.systems/blog/using-redux-in-angular/](http://bitcom.systems/blog/using-redux-in-angular/)\[/caption\]

Redux is infamous for the amount of boilerplate needed to set it up. But all these moving parts are easier to understand with an overview.

There are also extensions to make working with redux easier, like: [https://github.com/zalmoxisus/redux-devtools-extension](https://github.com/zalmoxisus/redux-devtools-extension)

To begin,

```text
npm i redux react-redux
```

Essentially there are three parts to Redux:

Store, Actions, and Reducers

You can think of the process with an analogy similar to the one below:\[caption id="attachment\_388" align="alignnone" width="1548"\]![Annotation 2019-06-12 114518](https://stuffihavelearnthome.files.wordpress.com/2019/06/annotation-2019-06-12-114518.png) source: [https://www.udemy.com/react-redux/](https://www.udemy.com/react-redux/)\[/caption\]

The Store stores the state \(compiled department data\). It is created by the aptly named createStore function from redux. And it's passed to the App component by wrapping it with the Provider \(similar to how React's context API works\).

index.js would look something like this:

```text
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import { createStore } from "redux";
import "./index.css";
import App from "./App";
import reducers from "./reducers";

const store = createStore(reducers);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>  
  ,
  document.getElementById("root")
);
```

\(Note that if you name your reducer file index.js in the reducers folder, it would be automatically imported from that folder without specifying 'index.js'.\)

The concept of a 'globally available' store is fairly straightforward, but you may notice that we are creating a store with reducers. Reducers have a similar role to .reduce\(\) in JavaScript.

They act like the reception counter at this metaphorical insurance agency, where they look at the form \(action in redux\), and file it accordingly in the room that stores department data \(store\), based on the type of action \(action.type\) and the data on the form \(action.payload\).

in reducers/accountReducer.js:

```text
const INITIAL_STATE = {
  userData: null
};
export default (state = INITIAL_STATE, action) => {
  switch (action.type) {
    case "SIGN_UP":
      let users = {...state.users}.push(action.payload)
      return { ...state, users };
    case "DELETE_ACCOUNT":
      //TODO: code that returns the state.users without user's account
      return state;
    default:
      return state;
  }
};
```

All the reducers are then combined in index.js. Of course, this only makes sense if there's more than one or two reducers.

in reducers/index.js:

```text
import { combineReducers } from "redux";
import accountReducer from "./accountReducer";

export default combineReducers({
  account: accountReducer
});
```

So now we have the 'store' and the 'reception', but we need to be able to tell the 'receptionist' what action we want to take, maybe via a form. This is the Actions part of redux, and it's pretty straightforward. You have a bunch of different kind of actions, which at its simplest returns the type of action and its payload.

in actions/index.js

```text
export const signUp = userData => {
  return {
    type: "SIGN_UP",
    payload: userData
  };
};
```

So now that we have all three parts of redux, how do we wire it up in the actual component/s using it?

To do so, we need to import `connect` from react-redux. `connect` does what it says on the tin, and connects React components to the Redux store.

From the official docs:

> It does not modify the component class passed to it; instead, it returns a new, connected component class that wraps the component you passed in.

The above is helpful in understanding the weird connect syntax. Because essentially, it is a higher-order component that you can pass a React component to.

It takes in four optional parameters:  
`mapStateToProps?: Function`  
`mapDispatchToProps?: Function | Object`  
`mergeProps?: Function`  
`options?: Object`

> If a mapStateToProps function is specified, the new wrapper component will subscribe to Redux store updates. This means that any time the store is updated, mapStateToProps will be called. The results of mapStateToProps must be a plain object, which will be merged into the wrapped component’s props. If you don't want to subscribe to store updates, pass null or undefined in place of mapStateToProps.

`connect` is really flexible, so you can visit [here](https://react-redux.js.org/api/connect) for all the different ways of using it. I'm going to show the syntax for a component that subscribe to state as well as use the action creators:

```text
import React, { Component } from "react";
import { connect } from "react-redux";
import { signUp } from "../actions";

class createAccount extends Component {

// state here

  onSubmit = () => {
      this.props.signUp(this.state.userData);
  };

// renderForm returns a controlled React form

  render() {
    return
```

{this.renderForm\(\)}

; } } const mapStateToProps = state =&gt; { return { signUp: state.userData }; }; export default connect\( mapStateToProps, { signUp } \)\(createAccount\);

And that's it. There's quite a bit of boilerplate involved at the start, but Redux isn't difficult to use, and makes state management a lot cleaner by its separation of concerns. However, for smaller apps, React's context API may be sufficient, so look into that as well.

Happy coding!

resources:  
[https://frontarm.com/james-k-nelson/when-context-replaces-redux/](https://frontarm.com/james-k-nelson/when-context-replaces-redux/)  
[https://www.valentinog.com/blog/redux/](https://www.valentinog.com/blog/redux/)  
[https://www.udemy.com/react-redux/learn](https://www.udemy.com/react-redux/learn)

