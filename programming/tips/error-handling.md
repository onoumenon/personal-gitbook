# Error handling

{% embed url="https://kevinsmith.io/a-better-way-to-handle-validation-errors" %}

### TLDR

#### &#x20;The default (not ideal):

* Throwing exceptions
* Returning a bundle of error messages

#### The problem:

* Exceptions should be exceptional
* A bundle of errors requires the caller to inspect it or if it's a bunch of fully formed error messages, it's deciding _how_ the caller should handle it

#### The solution:&#x20;

_Tell, Don't Ask_

When a method have several outcomes, it should tell other objects about the outcome.





Read also: [Action-domain-responder](https://en.wikipedia.org/wiki/Action%E2%80%93domain%E2%80%93responder)
