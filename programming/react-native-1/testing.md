# Testing

## Static Analysis

The first step to improve your code quality is to start using static analysis tools. Static analysis checks your code for errors as you write it, but without running any of that code.

* **Linters** ESLint
* **Type checking** Flow/ Typescript

## Structuring Tests



1. **Given** - some precondition
2. **When** - some action executed by the function that you’re testing
3. **Then** - the expected outcome

This is also known as AAA \(Arrange, Act, Assert\).



## Mocking

Sometimes, when your tested objects have external dependencies \(eg: API call\), you’ll want to “mock them out.” “Mocking” is when you replace some dependency of your code with your own implementation.

Why you shouldn't call the actual service:

* It could make the tests slow and unstable \(because of the network requests involved\)
* The service may return different data every time you run the test
* Third party services can go offline when you really need to run tests!

## Integration Tests

{% hint style="info" %}
Please note that the terminology around what integration testing means is not always consistent. Also, the line between what is a unit test and what is an integration test may not always be clear. For this guide, your test falls into "integration testing" if it:

* Combines several modules of your app as described above
* Uses an external system
* Makes a network call to other application \(such as the weather service API\)
* Does any kind of file or database I/O
{% endhint %}

## Component Tests

React components are responsible for rendering your app, and users will directly interact with their output. Even if your app's business logic has high testing coverage and is correct, without component tests you may still deliver a broken UI to your users. Component tests could fall into both unit and integration testing, but because they are such a core part of React Native, we'll cover them separately.

For testing React components, there are two things you may want to test:

* Interaction: to ensure the component behaves correctly when interacted with by a user \(eg. when user presses a button\)
* Rendering: to ensure the component render output used by React is correct \(eg. the button's appearance and placement in the UI\)

[React Native Testing Library](https://callstack.github.io/react-native-testing-library/) 

When testing user interactions, test the component from the user perspective—what's on the page? What changes when interacted with?

As a rule of thumb, prefer using things users can see or hear:

* make assertions using rendered text or [accessibility helpers](https://reactnative.dev/docs/accessibility#accessibility-properties)

Conversely, you should avoid:

* making assertions on component props or state
* testID queries

Avoid testing implementation details like props or state—while such tests work, they are not oriented toward how users will interact with the component and tend to break by refactoring \(for example when you'd like to rename some things or rewrite class component using hooks\).

```text
test('given empty GroceryShoppingList, user can add an item to it', () => {
  const { getByPlaceholder, getByText, getAllByText } = render(
    <GroceryShoppingList />
  );

  fireEvent.changeText(
    getByPlaceholder('Enter grocery item'),
    'banana'
  );
  fireEvent.press(getByText('Add the item to list'));

  const bananaElements = getAllByText('banana');
  expect(bananaElements).toHaveLength(1); // expect 'banana' to be on the list
});
```

## Snapshot Testing

Snapshots have several weak points:

* For you as a developer or reviewer, it can be hard to tell whether a change in snapshot is intended or whether it's evidence of a bug. Especially large snapshots can quickly become hard to understand and their added value becomes low.
* When snapshot is created, at that point it is considered to be correct-even in the case when the rendered output is actually wrong.
* When a snapshot fails, it's tempting to update it using the `--updateSnapshot` jest option without taking proper care to investigate whether the change is expected. Certain developer discipline is thus needed.

Snapshots themselves do not ensure that your component render logic is correct, they are merely good at guarding against unexpected changes and for checking that the components in the React tree under test receive the expected props \(styles and etc.\).

We recommend that you only use small snapshots \(see [`no-large-snapshots` rule](https://github.com/jest-community/eslint-plugin-jest/blob/master/docs/rules/no-large-snapshots.md)\). If you want to test a _change_ between two React component states, use [`snapshot-diff`](https://github.com/jest-community/snapshot-diff). When in doubt, prefer explicit expectations as described in the previous paragraph.

## E2E

E2E testing libraries allow you to find and control elements in the screen of your app: for example, you can _actually_ tap buttons or insert text into `TextInputs` the same way a real user would. Then you can make assertions about whether or not a certain element exists in the app’s screen, whether or not it’s visible, what text it contains, and so on.

E2E tests give you the highest possible confidence that part of your app is working. The tradeoffs include:

* writing them is more time consuming compared to the other types of tests
* they are slower to run
* they are more prone to flakiness \(a "flaky" test is a test which randomly passes and fails without any change to code\)

 [Detox](https://github.com/wix/detox/) is a popular framework because it’s tailored for React Native apps. Another popular library in the space of iOS and Android apps is [Appium](http://appium.io/).

## React Native Testing Library

{% embed url="https://github.com/vanGalilea/react-native-testing" %}

{% embed url="https://blog.usejournal.com/where-and-how-to-start-testing-your-react-native-app-%EF%B8%8F-and-how-to-keep-on-testin-ec3464fb9b41" %}



**Prioritizing test subjects**- where to start? start with the most important modules, screens, utils and the core logics of your app. This probably means your state management part of the app or any other place where your fetching data or making external calls. From there move to hooks, screens and any other piece of core util.

**Real life examples**- I’ve devoted a whole [repo](https://github.com/vanGalilea/react-native-testing) to this, the repo has a solid structure of test [configuration](https://github.com/vanGalilea/react-native-testing/blob/master/jest.config.js) and [setup](https://github.com/vanGalilea/react-native-testing/blob/master/jest.setup.js) that will help you along the way. The examples cover:

* [Clicking buttons and asserting onPress’ outcome](https://github.com/vanGalilea/react-native-testing/blob/master/__tests__/Counter-test.tsx).
* [Filling a simple login form and asserting successful submission](https://github.com/vanGalilea/react-native-testing/blob/master/__tests__/LoginSubmission-test.tsx).
* [Custom hook testing \(number of alternatives\)](https://github.com/vanGalilea/react-native-testing/blob/master/__tests__/CounterUsesCustomHook-test.tsx).
* [Mocking fetch calls](https://github.com/vanGalilea/react-native-testing/blob/7d13c63ffcb5de3df02c8b42d2e2aaf76421953e/__tests__/LoginSubmission-test.tsx#L36).
* [Mocking navigation through screens](https://github.com/vanGalilea/react-native-testing/blob/7d13c63ffcb5de3df02c8b42d2e2aaf76421953e/__tests__/LoginSubmission-test.tsx#L13)\([react navigation v5](https://reactnavigation.org/)\).
* [E2E feel due to real navigation throughout screens](https://github.com/vanGalilea/react-native-testing/blob/master/__tests__/Home-test.tsx).
* [Handling and mocking providers](https://github.com/vanGalilea/react-native-testing/blob/master/src/test/test-utils.tsx).
* [Mocking external lib.’s components](https://github.com/vanGalilea/react-native-testing/blob/master/__tests__/Video-test.tsx)

