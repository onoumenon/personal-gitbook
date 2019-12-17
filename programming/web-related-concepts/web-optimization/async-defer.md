# Javascript

{% embed url="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/javascript-startup-optimization/" %}

More information can be found in the website above.

![](../../../.gitbook/assets/image%20%2813%29.png)

The first optimisation is to reduce network transmission time:

## Network

* **Only sending the code a user needs \(code-split with webpack, lazy load\)**
* **Minification \(uglifyJS for ES5, babel-minify for ES2015+**
* **Compression \(brotli, gzip\)**
* **Removing unused code \(devtools code coverage to find unused code, babel-preset-env and browserlist to avoid transpiling avail modern features\)**
* **Caching code to minimize network trips.**
  * Use [HTTP caching](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching) to ensure browsers cache responses effectively. Determine optimal lifetimes for scripts \(max-age\) and supply validation tokens \(ETag\) to avoid transferring unchanged bytes.
  * Service Worker caching can make your app network resilient and give you eager access to features like [V8’s code cache](https://v8project.blogspot.com/2015/07/code-caching.html).
  * Use long-term caching to avoid having to re-fetch resources that haven't changed. If using Webpack, see [filename hashing](https://webpack.js.org/guides/caching/).

![Scripts take longer to parse and execute](../../../.gitbook/assets/image%20%2827%29.png)

{% hint style="info" %}
For deferring hubspot forms, a target div is needed so form will be rendered in it on load 

\(`window.onload = function(){}`\)
{% endhint %}



![](../../../.gitbook/assets/image%20%284%29.png)

