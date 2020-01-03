# Referential Transparency

{% embed url="https://programmingwords.com/home/referential-transparency" %}

> When a function is referentially transparent, it can be called at any time and anywhere and will always give the same result for the same input.

A referentially opaque function is akin to looking at a watch at different instances and getting different results each time.

Since the results of a referentially transparent function is always the same, you can memoize it.

"If I call this twice, will I get the same result?". If not, be more explicit about the dependencies. If it depends on current time or some other global variable, pass it in as a parameter.

