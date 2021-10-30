# big o & data structures

![](https://biercoff.com/content/images/2016/07/Screenshot-2016-07-15-16-16-10.png)

{% embed url="https://github.com/trekhleb/javascript-algorithms" %}

{% embed url="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures" %}

{% embed url="https://www.educative.io/blog/javascript-data-structures" %}

###

### Array

Array: [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array)

An indexed collection, ie: ordered list



### Set

keyed collection, where key and value are the same, can be converted to array

think of it as a set ie superset, subset

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Set)

```
const mySet1 = new Set()

mySet1.add(1)           // Set [ 1 ]
mySet1.add(5)           // Set [ 1, 5 ]
mySet1.add(5)           // Set [ 1, 5 ]
mySet1.add('some text') // Set [ 1, 5, 'some text' ]
const o = {a: 1, b: 2}
mySet1.add(o)

mySet1.add({a: 1, b: 2})   // o is referencing a different object, so this is okay

mySet1.has(1)              // true
mySet1.has(3)              // false, since 3 has not been added to the set
mySet1.has(5)              // true
mySet1.has(Math.sqrt(25))  // true
mySet1.has('Some Text'.toLowerCase()) // true
mySet1.has(o)       // true

mySet1.size         // 5

mySet1.delete(5)    // removes 5 from the set
mySet1.has(5)       // false, 5 has been removed

mySet1.size         // 4, since we just removed one value

console.log(mySet1)
// logs Set(4) [ 1, "some text", {…}, {…} ] in Firefox
// logs Set(4) { 1, "some text", {…}, {…} } in Chrome
```



```
//case sensitive & duplicate omission
new Set("Firefox")  // Set(7) { "F", "i", "r", "e", "f", "o", "x" }
new Set("firefox")  // Set(6) { "f", "i", "r", "e", "o", "x" }
```

### Weakset

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/WeakSet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/WeakSet)

The main differences to the [`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Set) object are:

* `WeakSet`s are collections of **objects only**. They cannot contain arbitrary values of any type, as [`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Set)s can.
* The `WeakSet` is _weak_, meaning references to objects in a `WeakSet` are held _weakly_. If no other references to an object stored in the `WeakSet` exist, those objects can be garbage collected.

#### [Use case: Detecting circular references](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/WeakSet#use\_case\_detecting\_circular\_references) <a href="use_case_detecting_circular_references" id="use_case_detecting_circular_references"></a>

Functions that call themselves recursively need a way of guarding against circular data structures by tracking which objects have already been processed.

`WeakSet`s are ideal for this purpose:

```
// Execute a callback on everything stored inside an object
function execRecursively(fn, subject, _refs = null){
  if(!_refs)
    _refs = new WeakSet();

  // Avoid infinite recursion
  if(_refs.has(subject))
    return;

  fn(subject);
  if("object" === typeof subject){
    _refs.add(subject);
    for(let key in subject)
      execRecursively(fn, subject[key], _refs);
  }
}

const foo = {
  foo: "Foo",
  bar: {
    bar: "Bar"
  }
};

foo.bar.baz = foo; // Circular reference!
execRecursively(obj => console.log(obj), foo);
```

### &#x20;Map

The **`Map`** object holds key-value pairs and remembers the original insertion order of the keys

performs better than object for frequent insertion/ deletion, has .size()

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Map)

```
const contacts = new Map()
contacts.set('Jessie', {phone: "213-555-1234", address: "123 N 1st Ave"})
contacts.has('Jessie') // true
contacts.get('Hilary') // undefined
contacts.set('Hilary', {phone: "617-555-4321", address: "321 S 2nd St"})
contacts.get('Jessie') // {phone: "213-555-1234", address: "123 N 1st Ave"}
contacts.delete('Raymond') // false
contacts.delete('Jessie') // true
console.log(contacts.size) // 1
```

The correct usage for storing data in the Map is through the `set(key, value)` method.

```
wrongMap.has('bla')    // false
wrongMap.delete('bla') // false
console.log(wrongMap)  // Map { bla: 'blaa', bla2: 'blaaa2' }
```

But that way of setting a property does not interact with the Map data structure. It uses the feature of the generic object. The value of 'bla' is not stored in the Map for queries. Other operations on the data fail:

```
const wrongMap = new Map()
wrongMap['bla'] = 'blaa'
wrongMap['bla2'] = 'blaaa2'

console.log(wrongMap)  // Map { bla: 'blaa', bla2: 'blaaa2' }
```

Therefore, this appears to work in a way:

Setting Object properties works for Map objects as well, and can cause considerable confusion.

### Weak map <a href="setting_object_properties" id="setting_object_properties"></a>

{% embed url="https://stackoverflow.com/questions/29413222/what-are-the-actual-uses-of-es6-weakmap" %}

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/WeakMap)

Like map, but unreferenced keys/ values are garbage collected



### Use cases

Some use cases that would otherwise cause a memory leak and are enabled by `WeakMap`s include:

* Keeping private data about a specific object and only giving access to it to people with a reference to the Map. A more ad-hoc approach is coming with the private-symbols proposal but that's a long time from now.
* Keeping data about library objects without changing them or incurring overhead.
* Keeping data about a small set of objects where many objects of the type exist to not incur problems with hidden classes JS engines use for objects of the same type.
* Keeping data about host objects like DOM nodes in the browser.
* Adding a capability to an object from the outside (like the event emitter example in the other answer).

{% embed url="https://github.com/monmohan/dsjslib" %}

[https://github.com/humanwhocodes/computer-science-in-javascript](https://github.com/humanwhocodes/computer-science-in-javascript)

\
User defined data structures
----------------------------



### Stack

```
"use strict";
module.exports = class Stack {
    constructor() {
        this.items = [];
        this.top = null;
    }

    getTop() {
        if (this.items.length == 0)
            return null;
        return this.top;
    }

    isEmpty() {
        return this.items.length == 0;
    }

    size() {
        return this.items.length;
    }

    push(element) {
        this.items.push(element);
        this.top = element;
    }

    pop() {
        if (this.items.length != 0) {
            if (this.items.length == 1) {
                this.top = null;
                return this.items.pop();
            } else {
                this.top = this.items[this.items.length - 2];
                return this.items.pop();
            }

        } else
            return null;
    }
}
```



### Queue

```
"use strict";
module.exports = class Queue {

    constructor() {
        this.items = [];
        this.front = null;
        this.back = null;

    }



    isEmpty() {
        return this.items.length == 0;
    }

    getFront() {
        if (this.items.length != 0) {
            return this.items[0];
        } else
            return null;
    }

    size() {
        return this.items.length;
    }

    enqueue(element) {
        this.items.push(element);
    }



    dequeue() {
        if (this.items.length == 0) {
            return null;
        } else {
            return this.items.shift();


        }

    }


}
```

### Linked List

```
module.exports = class Node {
  constructor(data) {
    this.data = data;
    this.nextElement = null;
  }
}
```

```
"use strict";
const Node = require('./Node.js');

module.exports = class LinkedList {
  constructor() {
    this.head = null;
  }

  //Insertion At Head  
  insertAtHead(newData) {
    let tempNode = new Node(newData);
    tempNode.nextElement = this.head;
    this.head = tempNode;
    return this; //returning the updated list
  }

  isEmpty() {
    return (this.head == null);
  }

  //function to print the linked list
  printList() {
    if (this.isEmpty()) {
      console.log("Empty List");
      return false;
    } else {
      let temp = this.head;
      while (temp != null) {
        process.stdout.write(String(temp.data));
        process.stdout.write(" -> ");
        temp = temp.nextElement;
      }
      console.log("null");
      return true;
    }
  }

  getHead() {
    return this.head;
  }
  setHead(newHead) {
    this.head = newHead;
    return this;
  }
  getListStr() {
    if (this.isEmpty()) {
      console.log("Empty List");
      return "null";
    } else {
      let st = "";
      let temp = this.head
      while (temp != null) {
        st += String(temp.data);
        st += " -> ";
        temp = temp.nextElement;
      }
      st += "null";
      return st;
    }
  }
  insertAtTail(newData) {
    //Creating a new Node with data as newData
    let node = new Node(newData);

    //check for case when list is empty
    if (this.isEmpty()) {
      //Needs to Insert the new node at Head
      this.head = node;
      return this;
    }

    //Start from head
    let currentNode = this.head;

    //Iterate to the last element
    while (currentNode.nextElement != null) {
      currentNode = currentNode.nextElement;
    }

    //Make new node the nextElement of last node of list
    currentNode.nextElement = node;
    return this;
  }
  search(value) {
    //Start from the first element
    let currentNode = this.head;

    //Traverse the list until you find the value or reach the end
    while (currentNode != null) {
      if (currentNode.data == value) {
        return true; //value found
      }
      currentNode = currentNode.nextElement
    }
    return false; //value not found
  }
  deleteAtHead() {
    //if list is empty, do nothing
    if (this.isEmpty()) {
      return this;
    }
    //Get the head and first element of the list
    let firstElement = this.head;

    //If list is not empty, link head to the nextElement of firstElement
    this.head = firstElement.nextElement;

    return this;
  }
  deleteVal(value) {
    let deleted = null; //True or False
    //Write code here

    //if list is empty return false
    if (this.isEmpty()) {
      return false;
    }

    //else get pointer to head
    let currentNode = this.head;
    // if first node's is the node to be deleted, delete it and return true
    if (currentNode.data == value) {
      this.head = currentNode.nextElement;
      return true;
    }

    // else traverse the list
    while (currentNode.nextElement != null) {
      // if a node whose next node has the value as data, is found, delete it from the list and return true
      if (currentNode.nextElement.data == value) {
        currentNode.nextElement = currentNode.nextElement.nextElement;
        return true;
      }
      currentNode = currentNode.nextElement;
    }
    //else node was not found, return false
    deleted = false;
    return deleted;
  }
  deleteAtTail() {
    // check for the case when linked list is empty
    if (this.isEmpty()) {
      return this;
    }
    //if linked list is not empty, get the pointer to first node
    let firstNode = this.head;
    //check for the corner case when linked list has only one element
    if (firstNode.nextElement == null) {
      this.deleteAtHead();
      return this;
    }
    //otherwise traverse to reach second last node
    while (firstNode.nextElement.nextElement != null) {
      firstNode = firstNode.nextElement;
    }
    //since you have reached second last node, just update its nextElement pointer to point at null, skipping the last node
    firstNode.nextElement = null;
    return this;
  }
}
```

