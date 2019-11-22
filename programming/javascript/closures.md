# Closures

> Closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time. 
>
> From MDN

```text
// Lexical Scoping: the closure function sayHello has access to the parent's variable, "John"
function greeting() {
  var name = "John";
  function sayHello() {
    alert(name);
  }
  sayHello();
}
greeting()
// alerts "John"


// Function that returns a function (Higher-order function)
function higherOrderFunction() {
  var name = "John";
  function sayHello() {
    alert(name);
  }
  return sayHello;
}
var greeting = higherOrderFunction();
greeting()
// alerts "John"
// This works because functions in JS form closures (think of a glasshouse housing the variables in its lexical environment). 


// Function Factory: function that creates other functions
function makePowerHouse(x) {
  return function(y) {
    return y ** x;
  };
}

var toThePowerOf5 = makePowerHouse(5);
var toThePowerOf10 = makePowerHouse(10);

console.log(toThePowerOf5(2));  //32
console.log(toThePowerOf10(2)); //1024
```

{% embed url="https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36" %}

> Among other things, closures are commonly used to give objects data privacy. Data privacy is an essential property that helps us program to an interface, not an implementation. This is an important concept that helps us build more robust software because implementation details are more likely to change in breaking ways than interface contracts.
>
> Source: [https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36)





