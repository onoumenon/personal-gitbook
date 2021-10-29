# JS

#### Array Methods

Once you have created arrays, there are a few things you can do with them:

* `concat()` — Join several arrays into one
* `indexOf()` — Returns the first position at which a given element appears in an array
* `join()` — Combine elements of an array into a single string and return the string
* `lastIndexOf()` — Gives the last position at which a given element appears in an array
* `pop()` — Removes the last element of an array
* `push()` — Add a new element at the end
* `reverse()` — Sort elements in a descending order
* `shift()` — Remove the first element of an array
* `slice()` — Pulls a copy of a portion of an array into a new array
* `sort()` — Sorts elements alphabetically
* `splice()` — Adds elements in a specified way and position
* `toString()` — Converts elements to strings
* `unshift()` —Adds a new element to the beginning
* `valueOf()` — Returns the primitive value of the specified object

#### Global Functions

* `isFinite()` — Determines whether a passed value is a finite number
* `isNaN()` — Determines whether a value is NaN or not
* `Number()` —- Returns a number converted from its argument
* `parseFloat()` — Parses an argument and returns a floating-point number
* `parseInt()` — Parses its argument and returns an integer



### JavaScript Loops

Loops are part of most programming languages. They allow you to execute blocks of code desired number of times with different values:

```
for (before loop; condition for loop; execute after loop) {
    // what to do during the loop
}
```

You have several parameters to create loops:

* `for` — The most common way to create a loop in JavaScript
* `while` — Sets up conditions under which a loop executes
* `do while` — Similar to the `while` loop but it executes at least once and performs a check at the end to see if the condition is met to execute again
* `break` —Used to stop and exit the cycle at certain conditions
* `continue` — Skip parts of the cycle if certain conditions are met



### Performance

#### Bad:

`for (let i = 0; i < arr.length; i++) {`\


#### Better Code:

`let l = arr.length;`\
`for (let i = 0; i < l; i++) {`

The bad code accesses the length property of an array each time the loop is iterated.

The better code accesses the length property outside the loop and makes the loop run faster.

### Don’t abuse closures <a href="1229" id="1229"></a>

JavaScript looks for variables by scope chain. If a variable cannot be found in the current scope, the JavaScript engine looks in the parent scope of the current scope, then up to the global scope level by level. So the deeper the closure is nested, the longer the variable lookup takes.

Bad code:

![](https://miro.medium.com/max/60/1\*Rmkw2ZvN6S57c1kSkq1nVw.png?q=20)![](https://miro.medium.com/max/1400/1\*Rmkw2ZvN6S57c1kSkq1nVw.png)

The up-level scoped variable `count` is used inside the `process` function, which makes it more time-consuming for the JavaScript engine to find the `count` variable when the `process` function is called.

At the same time, if the scope is nested multiple levels and there are dozens of lines of code between `process` and `count`, it is easy to confuse the count variable when reading the process function.

A better approach is to pass `count` as a parameter to `process`.

Good code:

![](https://miro.medium.com/max/60/1\*\_mXz6mfOoKfd7CLo1L4H1g.png?q=20)![](https://miro.medium.com/max/1400/1\*\_mXz6mfOoKfd7CLo1L4H1g.png)\






{% embed url="https://codility.com/media/train/1-TimeComplexity.pdf" %}
