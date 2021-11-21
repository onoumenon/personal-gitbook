# Other JS Concepts

Resource:

{% embed url="https://johnresig.com/apps/learn/" %}

```
// The .bind method from Prototype.js 
Function.prototype.bind = function(){ 
  var fn = this, args = Array.prototype.slice.call(arguments), object = args.shift(); 
  return function(){ 
    return fn.apply(object, 
      args.concat(Array.prototype.slice.call(arguments))); 
  }; 
};
```

### Helper methods

```
assert( true, "I'll pass." ); 
assert( "truey", "So will I." ); 
assert( false, "I'll fail." ); 
assert( null, "So will I." ); 
log( "Just a simple log", "of", "values.", true ); 
error( "I'm an error!" );
```

### Hoisting

Hoisting is JavaScript's default behavior of moving all declarations to the top of the current scope (to the top of the current script or the current function).&#x20;

TLDR: hoists `var` and functions (including definition). (Read more: [http://adripofjavascript.com/blog/drips/variable-and-function-hoisting.html](http://adripofjavascript.com/blog/drips/variable-and-function-hoisting.html))

{% hint style="info" %}
&#x20;**function definition hoisting only occurs for function declarations**, not function expressions.
{% endhint %}

`let` and `const` are not hoisted but have block scope

{% hint style="info" %}
Always declare your variables/ functions at the top of the scope.
{% endhint %}

```
var canFly = function(){ return true; }; 
window.isDeadly = function(){ return true; }; 
assert( isNimble() && canFly() && isDeadly(), "Still works, even though isNimble is moved." ); 
function isNimble(){ return true; }
```

### Block scope

Variables declared with the `let` keyword can have Block Scope.

Variables declared inside a block **{}** can not be accessed from outside the block:

```
{
  let x = 2;
}
// x can NOT be used here
```

What this means:

```
let i = 5;
for (let i = 0; i < 10; i++) {
  // some statements
}
// Here i is 5, if var was used, it would be mutated


let carName = "Volvo";
// code here can not use window.carName
var carName = "Volvo";
// code here can use window.carName

// let variables also cannot be redeclared 
```

