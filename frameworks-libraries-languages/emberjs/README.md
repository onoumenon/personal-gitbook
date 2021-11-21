# EmberJS

{% hint style="info" %}
Context: I used Ember during my first year of work, and this wiki serves as a log of major concepts in Ember.
{% endhint %}

{% embed url="https://emberjs.com/" %}

## Pros:&#x20;

* Convention over configuration&#x20;

Unlike React, Ember is a complete framework, and you have testing, routing and the data layer right out of the box. This means it's faster to set up due to the lack of bikeshedding.

* Stable and organized

Initially released in 2011, the core functionality/ concepts have reached a stable equilibrium, so developers do not need to worry about things changing dramatically (eg: Angular). The file structure and conventions also make it easier to coordinate PRs across larger teams.

* Not google/ facebook

The same can be said of Vue. This means more independence, but less influence.



## Cons

* Lesser usage means lesser add-on libraries and community support

The major drawback of EmberJS. A lot of niche libraries are also poorly maintained, which means you may need to monkey patch stuff.

* Less flexible

Despite the fact that EmberJS gives you some flexibility in what you can do (eg: Ember Data's default is JSON:API, but can be configured to work with other specifications), if your app has niche use cases, it can take even more time to set up.&#x20;

* Steeper learning curve

This is arguable. In adopting Ember, you are adopting a larger framework with a lot of pieces, and therefore, there's an information overload compared to React. The documentation of EmberJS is pretty decent, but lacking in the deeper levels. When it comes to the implementation details of their individual APIs, it gets kinda fuzzy. Eg: when you try to understand an API that might be in your codebase, ([https://api.emberjs.com/ember/3.22/classes/EngineInstance/methods/hasRegistration?anchor=hasRegistration](https://api.emberjs.com/ember/3.22/classes/EngineInstance/methods/hasRegistration?anchor=hasRegistration)), the docs provide the technical details, but not the overall context of what it's meant for.  \
\
However, compared with a React app + multiple 3rd party libraries providing the same functionality, it may be easier overall.

