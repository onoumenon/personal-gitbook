# New Architecture

{% embed url="https://formidable.com/blog/2019/react-codegen-part-1/" %}

## Old architecture

![](<../../.gitbook/assets/image (161).png>)

* Rely on asynchronous JSON messages transmitted across The Bridge.&#x20;
* These are sent to the native code with the expectation (**but not a guarantee**) that they will elicit a response some time in the future.
* All data has to get serialized into JSON on its way in and deserialized on its way out. This double ser-de pass can be costly for data-intensive problems. It also prevents sharing any memory between native and JS.



