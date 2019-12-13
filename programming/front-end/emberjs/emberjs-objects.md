---
description: A TLDR version of the official Ember guide
---

# EmberJS Objects

## Object Model

Module: [_@ember/object_](https://api.emberjs.com/ember/release/classes/@ember%2Fobject/methods/extend?anchor=extend)\_\_

Ember objects extend plain JS objects to provide framework's functionalities \(eg: binding, being observable\).

To define a new Ember _class_, call the [`extend()`](http://emberjs.com/api/classes/Ember.Object.html#method_extend) method on [`Ember.Object`](http://emberjs.com/api/classes/Ember.Object.html)

#### 

#### Overriding Parent Class method

use this.\_super\(\)

```text
normalizeResponse(store, primaryModelClass, payload, id, requestType)  {
  // Customize my JSON payload for Ember-Data
  return this._super(...arguments);
}
```

#### 

#### Reopening a class

Reopen the class using [`reopen()`](http://emberjs.com/api/classes/Ember.Object.html#method_reopen) method. It adds instance methods and properties that are shared across all instances of a class

When you need to add static methods or static properties to the class itself you can use [`reopenClass()`](http://emberjs.com/api/classes/Ember.Object.html#method_reopenClass).

#### 

#### [Computed Properties](https://api.emberjs.com/ember/release/functions/@ember%2Fobject/computed)

Let you declare functions as properties. Take properties and manipulate them to create new values.

```text
let obj = Ember.Object.extend({
  baz: {foo: 'BLAMMO', bar: 'BLAZORZ'},

  something: Ember.computed('baz.{foo,bar}', function() {
    return this.get('baz.foo') + ' ' + this.get('baz.bar');
  })
});
```

#### 

#### Chain computed properties

```text
Person = Ember.Object.extend({
  firstName: null,
  lastName: null,
  age: null,
  country: null,

  fullName: Ember.computed('firstName', 'lastName', function() {
    return `${this.get('firstName')} ${this.get('lastName')}`;
  }),

  description: Ember.computed('fullName', 'age', 'country', function() {
    return `${this.get('fullName')}; Age: ${this.get('age')}; Country: ${this.get('country')}`;
  })
});

let captainAmerica = Person.create({
  firstName: 'Steve',
  lastName: 'Rogers',
  age: 80,
  country: 'USA'
});

captainAmerica.get('description'); // "Steve Rogers; Age: 80; Country: USA"
```

#### 

#### Dynamic Updating Computed Properties

```text
captainAmerica.set('firstName', 'William');

captainAmerica.get('description'); // "William Rogers; Age: 80
```

Setting any dependent property will propagate changes through any computed properties.



#### @each \(Computed Properties\)

```text
export default Ember.Component.extend({
  todos: null,

  init() {
    this.set('todos', [
      Ember.Object.create({ isDone: true }),
      Ember.Object.create({ isDone: false }),
      Ember.Object.create({ isDone: true }),
    ]);
  },

  incomplete: Ember.computed('todos.@each.isDone', function() {
    let todos = this.get('todos');
    return todos.filterBy('isDone', false);
  })
});
```

`todos.@each.isDone` instructs Ember.js to update bindings and fire observers when any of the following events occurs:

* The `isDone` property of any of the objects in the `todos` array changes.
* An item is added to the `todos` array.
* An item is removed from the `todos` array.
* The `todos` property of the component is changed to a different array.

`@each` key can be dependant on more than one key. For example, if you are using `Ember.computed` to sort an array by multiple keys, you would declare the dependency with braces: `todos.@each.{priority,title}`

####  <a id="toc_computed-property-macros"></a>

#### Computed Property Macros <a id="toc_computed-property-macros"></a>

The above code for incomplete can be simplified to:

```text
  incomplete: Ember.computed.filterBy('todos', 'isDone', false)
```

Note:`@each` only works one level deep. You cannot use nested forms like`todos.@each.owner.name` or `todos.@each.owner.@each.name`.

Use \[\] instead of @each if you don't care about the properties of individual array item changing.

```text
const Hamster = Ember.Object.extend({
  excitingChores: Ember.computed('chores.[]', function() {
    return this.get('chores').map(function(chore, index) {
      return `CHORE ${index}: ${chore.toUpperCase()}!`;
    });
  })
});

const hamster = Hamster.create({
  chores: ['clean', 'write more unit tests']
});

hamster.get('excitingChores'); // ['CHORE 1: CLEAN!', 'CHORE 2: WRITE MORE UNIT TESTS!']
hamster.get('chores').pushObject('review code');
hamster.get('excitingChores'); // ['CHORE 1: CLEAN!', 'CHORE 2: WRITE MORE UNIT TESTS!', 'CHORE 3: REVIEW CODE!']
```

Can be abstracted to:

```text
const Hamster = Ember.Object.extend({
  excitingChores: Ember.computed.map('chores', function(chore, index) {
    return `CHORE ${index}: ${chore.toUpperCase()}!`;
  })
});
```



### Observers

_**Note:** Favour computed properties rather than observers._

Used when you want to perform some behavior after a binding has finished synchronizing.



#### Observers and async operations

Observers are syncronous so unexpected behaviours can occur:

```text
Person.reopen({
  lastNameChanged: Ember.observer('lastName', function() {
    // The observer depends on lastName and so does fullName. Because observers
    // are synchronous, when this function is called the value of fullName is
    // not updated yet so this will log the old value of fullName
    console.log(this.get('fullName'));
  })
});
```

Use  [`Ember.run.once()`](http://emberjs.com/api/classes/Ember.run.html#method_once)to avoid these problems:

```text
Person.reopen({
  partOfNameChanged: Ember.observer('firstName', 'lastName', function() {
    Ember.run.once(this, 'processFullName');
  }),

  processFullName() {
    // This will only fire once if you set two properties at the same time, and
    // will also happen in the next run loop once all properties are synchronized
    console.log(this.get('fullName'));
  }
});

person.set('firstName', 'John');
person.set('lastName', 'Smith');
```

####  <a id="toc_observers-and-object-initialization"></a>

#### Observers and object initialization <a id="toc_observers-and-object-initialization"></a>

Observers never fire until after the initialization of an object is complete.

Instead, specify that the observer should also run after `init` by using [`Ember.on()`](http://emberjs.com/api/classes/Ember.html#method_on)

```text
Person = Ember.Object.extend({
  init() {
    this.set('salutation', 'Mr/Ms');
  },

  salutationDidChange: Ember.on('init', Ember.observer('salutation', function() {
    // some side effect of salutation changing
  }))
});
```

Note: Unconsumed computed properties do not trigger observers



#### Outside class definitions

You can also add observers to an object outside of a class definition using [`addObserver()`](http://emberjs.com/api/classes/Ember.Object.html#method_addObserver):

```text
person.addObserver('fullName', function() {
  // deal with the change
});
```



### Bindings

Favour computed properties over bindings.

Easiest way is  [`computed.alias()`](http://emberjs.com/api/classes/Ember.computed.html#method_alias)

```text
husband = Ember.Object.create({
  pets: 0
});

Wife = Ember.Object.extend({
  pets: Ember.computed.alias('husband.pets')
});

wife = Wife.create({
  husband: husband
});

wife.get('pets'); // 0

// Someone gets a pet.
husband.set('pets', 1);
wife.get('pets'); // 1
```

Use [`computed.oneWay()`](http://emberjs.com/api/classes/Ember.computed.html#method_oneWay) for performance optimization or for certain use cases \(eg: setting delivery address based on home address which can later be updated\).

### Enumerables methods

Enumerable object that contains child objects. In order for Ember to observe when you make a change to an enumerable, you need to use special methods that `Ember.Enumerable` provides. 

[API for Ember.Enumerable](https://api.emberjs.com/ember/release/classes/MutableArray)

[API for Ember.Array](https://api.emberjs.com/ember/2.14/classes/Ember.Array)





