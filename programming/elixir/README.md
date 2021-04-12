# Elixir

Why Elixir?

{% embed url="https://habr.com/en/post/449522/" %}

Key points:

* can deal with high load
* functional programming \(immutability\)
* multiple restrictions on runtime makes it harder to write, but the application more reliable
* ecosystem is still very young
* more effective way of using elixir: use only for 'hottest' part of a system with high load requirements

Interesting points:

When talking about multithreading in, say, C, we know that there is shared memory and several threads that may compete for resources. In such conditions, it is tough to create a reliable code for high-load projects because of random race conditions and segmentation faults.



