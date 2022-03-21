# Clean Code

{% embed url="https://www.youtube.com/watch?v=7EmboKQH8lM" %}

* Long code is not good code
* Refactor by extracting functions until the function only does one thing
* Why: polite code lets the reader opt out at any point by introducing complexities a level at a time
* It's preferable to have a 'sea' of tiny functions instead as they would be organized and named and placed in meaningful folders
* Refactoring a large function: start by extracting and recognizing patterns. May need a class/ OOP if those objects emerge
* Max 3 param arguments. Otherwise, create a data structure for the params
* Don't pass booleans around in arguments (unless it's state for a switch), because you can do two separate functions instead
* Indenting: max 2 levels
* Avoid creating surprises in your code
* Avoid switch statements, cos it becomes a dependency nightmare if it calls on other func. Use polymorphism instead.
* Side effects: side effect funcs come in pairs (open and close)
* We're bad at managing pairs of functions, therefore we have the hack of garbage collection
* We can use lambda to  make side effect funcs safer, by having the new open func call the internal open, call the lambda process, and close the  file.
* command and query separation: a func that returns void must have a side effect, and a func that returns a value shouldn't
* prefer exceptions than error codes
* structured programming: go-tos were the norm, but unrestrained go-tos lead to halting problem  ([https://en.wikipedia.org/wiki/Halting\_problem](https://en.wikipedia.org/wiki/Halting\_problem)). So prefer the other 3 structures: sequence, if/else, iteration (loops)
* it's too hard to prove software is correct so we write tests instead (science rather than maths)
* comment code that cannot explain itself
