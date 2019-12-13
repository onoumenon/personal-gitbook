# Debounce

{% embed url="https://www.geeksforgeeks.org/debouncing-in-javascript/" %}

Debounce limits the rate a function is called. Eg: You press a button rapidly 3 times, but only 1 button press is registered.

It is important in JavaScript because it’s a single-threaded language, and large tasks will become blocking.

```text
<html> 
  <body> 
    <button id="debounce"> 
      Debounce 
    </button> 
    <script> 
    var button = document.getElementById("debounce"); 

    // debounce is a higher-order function that calls the `func` provided with a setTimeout of `delay`
    const debounce = (func, delay) => { 
      let debounceTimer 
      return function() { 
        const context = this
        const args = arguments 
        clearTimeout(debounceTimer) 
        debounceTimer = setTimeout(() => func.apply(context, args), delay) 
      } 
    }

    // Add a listener to the button, and provide debounce with a func and delay time
    button.addEventListener('click', debounce(function() {
      alert("Hello\nNo matter how many times you" + 
        "click the debounce button, I get " + 
        "executed once after 3 seconds of the last click.") 
    }, 3000)); 
    </script> 
  </body> 
</html> 
```

Here's what happening:  
  
A button with id `debounce` is created. 

It's declared as a var `button`. 

A higher order function `debounce` is created. A listener is attached to the button.

When the button is click, the function `debounce` is called.

1. Start with 0 timeout
2. If the debounced function is called again \(when button is clicked\), reset the timer again to the specified delay \(3 seconds\)
3. When the timeout expires, the function provided \(alert\) is applied to `this` which is the global object `window`. \(Check [this](this.md) out\)



