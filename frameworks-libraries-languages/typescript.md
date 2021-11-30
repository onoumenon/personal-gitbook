# Typescript

Why?

JS is weakly typed:&#x20;

*   JavaScript’s equality operator (`==`) _coerces_ its arguments, leading to unexpected behavior:

    ```
    if ("" == 0) {  // It is! But why??}if (1 < x < 3) {  // True for *any* value of x!}
    ```
*   JavaScript also allows accessing properties which aren’t present:

    ```
    const obj = { width: 10, height: 15 };// Why is this NaN? Spelling is hard!const area = obj.width * obj.heigth;
    ```

\
to define a User interface with specific types:

```
interface User {
  name: string;
  id: number;
}
```

```
const user: User = {
  username: "Hayes",
  id: 0,
};
```

will throw:

```
Type '{ username: string; id: number; }' is not assignable to type 'User'.
Object literal may only specify known properties, and 'username' does not exist in type 'User'.
```

There is already a small set of primitive types available in JavaScript: `boolean`, `bigint`, `null`, `number`, `string`, `symbol`, and `undefined`, which you can use in an interface. TypeScript extends this list with a few more, such as `any` (allow anything), [`unknown`](https://www.typescriptlang.org/play#example/unknown-and-never) (ensure someone using this type declares what the type is), [`never`](https://www.typescriptlang.org/play#example/unknown-and-never) (it’s not possible that this type could happen), and `void` (a function which returns `undefined` or has no return value).





### Composing Types <a href="#composing-types" id="composing-types"></a>

With TypeScript, you can create complex types by combining simple ones. There are two popular ways to do so: with unions, and with generics.

#### Unions <a href="#unions" id="unions"></a>

With a union, you can declare that a type could be one of many types. For example, you can describe a `boolean` type as being either `true` or `false`:

```
type MyBool = true | false;Try
```

_Note:_ If you hover over `MyBool` above, you’ll see that it is classed as `boolean`. That’s a property of the Structural Type System. More on this below.

A popular use-case for union types is to describe the set of `string` or `number` [literals](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) that a value is allowed to be:

```
type WindowStates = "open" | "closed" | "minimized";type LockStates = "locked" | "unlocked";type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;Try
```

Unions provide a way to handle different types too. For example, you may have a function that takes an `array` or a `string`:

```
function getLength(obj: string | string[]) {  return obj.length;}
```

To learn the type of a variable, use `typeof`:

| Type      | Predicate                          |
| --------- | ---------------------------------- |
| string    | `typeof s === "string"`            |
| number    | `typeof n === "number"`            |
| boolean   | `typeof b === "boolean"`           |
| undefined | `typeof undefined === "undefined"` |
| function  | `typeof f === "function"`          |
| array     | `Array.isArray(a)`                 |

### Structural Type System <a href="#structural-type-system" id="structural-type-system"></a>

One of TypeScript’s core principles is that type checking focuses on the _shape_ that values have. This is sometimes called “duck typing” or “structural typing”.

In a structural type system, if two objects have the same shape, they are considered to be of the same type.

```
interface Point {
  x: number;
  y: number;
}
 
function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}
 
// logs "12, 26"
const point = { x: 12, y: 26 };
logPoint(point);
Try
The point variable is never declared to be a Point type. However, TypeScript compares the shape of point to the shape of Point in the type-check. They have the same shape, so the code passes.
```

```
const point3 = { x: 12, y: 26, z: 89 };
logPoint(point3); // logs "12, 26" 

const rect = { x: 33, y: 3, width: 30, height: 80 };
logPoint(rect); // logs "33, 3" 

const color = { hex: "#187ABF" };
logPoint(color);

# Argument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.
  Type '{ hex: string; }' is missing the following properties from type 'Point': x, yArgument of type '{ hex: string; }' is not assignable to parameter of type 'Point'.
  Type '{ hex: string; }' is missing the following properties from type 'Point': x, y
```

The shape-matching only requires a subset of the object’s fields to match.

There is no difference between how classes and objects conform to shapes:

```
class VirtualPoint {  
        x: number;  
        y: number;   

        constructor(x: number, y: number) {
        this.x = x;
        this.y = y;  
    }
} 
const newVPoint = new VirtualPoint(13, 56);
logPoint(newVPoint); // logs "13, 56"

```

### Static type-checking <a href="#static-type-checking" id="static-type-checking"></a>

Ideally, we could have a tool that helps us find these bugs _before_ our code runs. That’s what a static type-checker like TypeScript does. _Static types systems_ describe the shapes and behaviors of what our values will be when we run our programs. A type-checker like TypeScript uses that information and tells us when things might be going off the rails.

```
const message = "hello!"; message();This expression is not callable.
  Type 'String' has no call signatures.This expression is not callable.
  Type 'String' has no call signatures.Try
```

Running that last sample with TypeScript will give us an error message before we run the code in the first place.\


### Non-exception Failures

So far we’ve been discussing certain things like runtime errors - cases where the JavaScript runtime tells us that it thinks something is nonsensical. Those cases come up because [the ECMAScript specification](https://tc39.github.io/ecma262/) has explicit instructions on how the language should behave when it runs into something unexpected.

For example, the specification says that trying to call something that isn’t callable should throw an error. Maybe that sounds like “obvious behavior”, but you could imagine that accessing a property that doesn’t exist on an object should throw an error too. Instead, JavaScript gives us different behavior and returns the value `undefined`:

```
const user = {  name: "Daniel",  age: 26,};
user.location; // returns undefined
```

Ultimately, a static type system has to make the call over what code should be flagged as an error in its system, even if it’s “valid” JavaScript that won’t immediately throw an error. In TypeScript, the following code produces an error about `location` not being defined:

```
const user = {  name: "Daniel",  age: 26,};
user.location;
Property 'location' does not exist on type '{ name: string; age: number; }'.Property 'location' does not exist on type '{ name: string; age: number; }'.Try
```

### Tooling

![](<../.gitbook/assets/Screenshot 2021-11-26 at 3.21.58 PM.png>)

IDE extensions to help prevent mistakes

### `tsc`, the TypeScript compiler <a href="#tsc-the-typescript-compiler" id="tsc-the-typescript-compiler"></a>

```
npm install -g typescript
```

We’ve been talking about type-checking, but we haven’t yet used our type-_checker_. Let’s get acquainted with our new friend `tsc`, the TypeScript compiler. First we’ll need to grab it via npm.

```
tsc hello.ts
# converts file to js
```

So TypeScript doesn’t get in your way. Of course, over time, you may want to be a bit more defensive against mistakes, and make TypeScript act a bit more strictly. In that case, you can use the [`noEmitOnError`](https://www.typescriptlang.org/tsconfig#noEmitOnError) compiler option. Try changing your `hello.ts` file and running `tsc` with that flag:

```
tsc --noEmitOnError hello.ts
```

You’ll notice that `hello.js` never gets updated.

Explicit Types

Up until now, we haven’t told TypeScript what `person` or `date` are. Let’s edit the code to tell TypeScript that `person` is a `string`, and that `date` should be a `Date` object. We’ll also use the `toDateString()` method on `date`.

```
function greet(person: string, date: Date) {  console.log(`Hello ${person}, today is ${date.toDateString()}!`);}Try
```

What we did was add _type annotations_ on `person` and `date` to describe what types of values `greet` can be called with. You can read that signature as ”`greet` takes a `person` of type `string`, and a `date` of type `Date`“.

With this, TypeScript can tell us about other cases where `greet` might have been called incorrectly. For example…

```
function greet(person: string, date: Date) {  console.log(`Hello ${person}, today is ${date.toDateString()}!`);} greet("Maddison", Date());Argument of type 'string' is not assignable to parameter of type 'Date'.Argument of type 'string' is not assignable to parameter of type 'Date'.Try
```

Huh? TypeScript reported an error on our second argument, but why?

Perhaps surprisingly, calling `Date()` in JavaScript returns a `string`. On the other hand, constructing a `Date` with `new Date()` actually gives us what we were expecting.

Anyway, we can quickly fix up the error:

```
function greet(person: string, date: Date) {  console.log(`Hello ${person}, today is ${date.toDateString()}!`);} greet("Maddison", new Date());Try
```

\


By default TypeScript targets ES3, an extremely old version of ECMAScript. We could have chosen something a little bit more recent by using the [`target`](https://www.typescriptlang.org/tsconfig#target) option. Running with `--target es2015` changes TypeScript to target ECMAScript 2015, meaning code should be able to run wherever ECMAScript 2015 is supported. So running `tsc --target es2015 hello.ts` gives us the following output:

```
function greet(person, date) {  console.log(`Hello ${person}, today is ${date.toDateString()}!`);}greet("Maddison", new Date());
```

### Type Annotations on Variables <a href="#type-annotations-on-variables" id="type-annotations-on-variables"></a>

When you declare a variable using `const`, `var`, or `let`, you can optionally add a type annotation to explicitly specify the type of the variable:

```
let myName: string = "Alice";Try
```

> TypeScript doesn’t use “types on the left”-style declarations like `int x = 0;` Type annotations will always go _after_ the thing being typed.

In most cases, though, this isn’t needed. Wherever possible, TypeScript tries to automatically _infer_ the types in your code. For example, the type of a variable is inferred based on the type of its initializer:

```
// No type annotation needed -- 'myName' inferred as type 'string'let myName = "Alice";Try
```

Parameter Type Annotations

When you declare a function, you can add type annotations after each parameter to declare what types of parameters the function accepts. Parameter type annotations go after the parameter name:

```
// Parameter type annotationfunction greet(name: string) {  console.log("Hello, " + name.toUpperCase() + "!!");}Try
```

When a parameter has a type annotation, arguments to that function will be checked:

```
// Would be a runtime error if executed!greet(42);Argument of type 'number' is not assignable to parameter of type 'string'.Argument of type 'number' is not assignable to parameter of type 'string'.Try
```

> Even if you don’t have type annotations on your parameters, TypeScript will still check that you passed the right number of arguments.

#### Return Type Annotations <a href="#return-type-annotations" id="return-type-annotations"></a>

You can also add return type annotations. Return type annotations appear after the parameter list:

```
function getFavoriteNumber(): number {  return 26;}Try
```

Much like variable type annotations, you usually don’t need a return type annotation because TypeScript will infer the function’s return type based on its `return` statements. The type annotation in the above example doesn’t change anything. Some codebases will explicitly specify a return type for documentation purposes, to prevent accidental changes, or just for personal preference.

#### &#x20;<a href="#anonymous-functions" id="anonymous-functions"></a>

#### Anonymous Functions <a href="#anonymous-functions" id="anonymous-functions"></a>

Anonymous functions are a little bit different from function declarations. When a function appears in a place where TypeScript can determine how it’s going to be called, the parameters of that function are automatically given types.

Here’s an example:

```
// No type annotations here, but TypeScript can spot the bugconst names = ["Alice", "Bob", "Eve"]; // Contextual typing for functionnames.forEach(function (s) {  console.log(s.toUppercase());Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?}); // Contextual typing also applies to arrow functionsnames.forEach((s) => {  console.log(s.toUppercase());Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?Property 'toUppercase' does not exist on type 'string'. Did you mean 'toUpperCase'?});
```

#### Optional Properties

Object types can also specify that some or all of their properties are _optional_. To do this, add a `?` after the property name



#### Defining a Union Type

The first way to combine types you might see is a _union_ type. A union type is a type formed from two or more other types, representing values that may be _any one_ of those types. We refer to each of these types as the union’s _members_.

Let’s write a function that can operate on strings or numbers:

```
function printId(id: number | string) {  console.log("Your ID is: " + id);}// OKprintId(101);// OKprintId("202");// ErrorprintId({ myID: 22342 });Argument of type '{ myID: number; }' is not assignable to parameter of type 'string | number'.
  Type '{ myID: number; }' is not assignable to type 'number'.Argument of type '{ myID: number; }' is not assignable to parameter of type 'string | number'.
  Type '{ myID: number; }' is not assignable to type 'number'.Try
```

#### &#x20;<a href="#working-with-union-types" id="working-with-union-types"></a>

Another example is to use a function like `Array.isArray`:

```
function welcomePeople(x: string[] | string) {  if (Array.isArray(x)) {    // Here: 'x' is 'string[]'    console.log("Hello, " + x.join(" and "));  } else {    // Here: 'x' is 'string'    console.log("Welcome lone traveler " + x);  }}
```

\
A _type alias_ is exactly that - a _name_ for any _type_. The syntax for a type alias is:

```
type Point = {  x: number;  y: number;};
// Exactly the same as the earlier example
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
printCoord({ x: 100, y: 100 });

```

Differences Between Type Aliases and Interfaces

Type aliases and interfaces are very similar, and in many cases you can choose between them freely. Almost all features of an `interface` are available in `type`, the key distinction is that a type cannot be re-opened to add new properties vs an interface which is always extendable.





But by _combining_ literals into unions, you can express a much more useful concept - for example, functions that only accept a certain set of known values:

```
function printText(s: string, alignment: "left" | "right" | "center") {
  // ...
}

printText("Hello, world", "left");
printText("G'day, mate", "centre");
Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.Argument of type '"centre"' is not assignable to parameter of type '"left" | "right" | "center"'.Try
```

Numeric literal types work the same way:

```
function compare(a: string, b: string): -1 | 0 | 1 {
  return a === b ? 0 : a > b ? 1 : -1;
  }
```

