# Testing

## Arrange Act Assert

{% embed url="http://wiki.c2.com/?ArrangeActAssert" %}

{% embed url="https://integralpath.blogs.com/thinkingoutloud/2005/09/principles\_of\_t.html" %}

1\) **Tests serve as examples of how to use a class or method.**  Once used to having tests that show how things work \(and _that_ they work\), developers start using the key phrase, "Do you have a test for that?" Tests become like documentation.

2\) **Developer tests \(they call them "coder tests"\) are distinctly different from QA test and should be kept separate.**  QA tests target features and treat the system as a black box.  Unit test created by the developer operate at a lower level and test different things.

3\) **Name your tests carefully.** 

4\) **Write each test before writing the method under test.**  aka TDD. This ensures that you don't waste time writing code that isn't going to be tested and used.  It also encourages the developer to think as a user of the target method before thinking about implementation, which usually results in a cleaner, easier-to-use interface.

5\) **Follow the "3-As" pattern for test methods: Arrange, Act, Assert.** Specifically, use separate code paragraphs \(groups of lines of code separated by a blank line\) for each of the As.  Arrange is variable declaration and initialization.  Act is invoking the code under test.  Assert is using the Assert.\* methods to verify that expectations were met.  Following this pattern consistently makes it easy to revisit test code.

6\) **When writing application code, only write enough code to make a test work.**  If you know there should be more code to handle other logic cases, go write the tests for those cases.  This technique prevents gold-plating and ensures that you always have a test for the code you write. 

7\) **When you find you need to refactor working code, refactor and re-test prior to writing new code.**  This technique ensures your refactoring is correct prior to adding new functionality and applies to creating new methods, introducing inheritance, everything.  I equate this principle to the practice of running all tests in a solution after getting the latest code from the repository and prior to writing anything new.  I don't want to spend time debugging my newly written code under the false assumption that the system wasn't already broken.

## Testing Recipes

### Setup/ teardown

```text
import { unmountComponentAtNode } from "react-dom";

let container = null;
beforeEach(() => {
  // setup a DOM element as a render target
  container = document.createElement("div");
  document.body.appendChild(container);
});

afterEach(() => {
  // cleanup on exiting
  unmountComponentAtNode(container);
  container.remove();
  container = null;
});
```

You may use a different pattern, but keep in mind that we want to execute the cleanup _even if a test fails_. Otherwise, tests can become “leaky”, and one test can change the behavior of another test. That makes them difficult to debug.

