# Event loop

{% embed url="https://www.youtube.com/watch?v=8aGhZQkoFbQ" %}



{% embed url="https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop" %}

![](../../.gitbook/assets/image%20%2832%29.png)

## TLDR

* JS has a single-threaded runtime \(has one call stack, can only do one thing at a time\*\)
* A call stack is a data structure to keep track of its place in a script that calls multiple [functions](https://developer.mozilla.org/en-US/docs/Glossary/function), and is LIFO \(read more: [https://developer.mozilla.org/en-US/docs/Glossary/Call\_stack](https://developer.mozilla.org/en-US/docs/Glossary/Call_stack)\)
* slow processes are 'blocking', they block other processes from being called
* Solution: Async callbacks
* Async processes, when done, are placed in the queue \(FIFO\)
* The event loop runs processes in queue if stack is empty
* Rendering can't be done if stack is filled, but is higher priority than callbacks in queue
* Debounce callbacks so you don't flood the queue

