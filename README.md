# es6-cheetsheet
ES2015, ES2016, ES2017 and beyond.

Pull requests are welcomed 🙏🏻❤️

## TABLE OF CONTENTS
- [Variable Declarations](#variable-declarations)
- [String Templates](#string-templates)
- [Arrow Functions](#arrow-functions)
- [Classes](#classes)
- [Destructuring](#destructuring)
- [Spread Operator](#spread-operator)
- [Getters/Setters](#getters-setters)
- [Modules](#modules)
- [Data Structures](#data-structures)
- [Helpful string functions](#helpful-string-functions)
- [Helpful array functions](#helpful-array-functions)
- [Promises](#promises)
- [Generators](#generators)
- [Async/Await](#async-await)

## Variable Declarations

ES6 brought `let` and `const` with proper lexical scoping.
`let` is the new `var`. Constants work just like `let`, but can’t be reassigned.
`let` and `const` are block scoped. Therefore, referencing
block-scoped identifiers before they are defined will produce
a `ReferenceError`.

Example using `var`:

```javascript
var variable = 5;

{
  console.log('inside', variable); //5
  var variable = 10;
}

console.log('outside', variable); //10
```

Observe what happens when we use `let`:

```javascript
let variable = 5;

{
  console.log('inside', variable); //ReferenceError
  let variable = 10;
}

console.log('outside', variable); //5
```

Example using `const`:

```javascript
const variable = 5;

variable = variable*2; //TypeError: Attempted to assign to readonly property.
```

Constants are tricky with array and objects. The `reference` becomes constant but the value does not.

```javascript
const variable = [5];

console.log(variable) //[5]

variable = [2]; //TypeError: Attempted to assign to readonly property.

variable[0] = 1;
console.log(variable) //[1]
```

## String Templates

Template strings provide syntactic sugar for constructing strings.

Using **Template Literals**, we can now construct strings that have special
characters in them without needing to escape them explicitly.

```javascript
// Basic literal string creation
`This is a pretty little template string.`

// Multiline strings
`In ES5 this is
 not legal.`

// Interpolate variable bindings
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

// Unescaped template strings
String.raw`In ES5 "\n" is a line-feed.`

// Construct an HTTP request prefix is used to interpret the replacements and construction
GET`http://foo.org/bar?a=${a}&b=${b}
    Content-Type: application/json
    X-Credentials: ${credentials}
    { "foo": ${foo},
      "bar": ${bar}}`(myOnReadyStateChangeHandler);
```

**Template Literals** can also accept expressions:

```javascript
let today = new Date();
let text = `The time and date is ${today.toLocaleString()}`;
```

## Arrow Functions

Arrows are a function shorthand using the `=>` syntax. 
Arrow functions allow you to preserve the lexical value of `this`.

Take the example below where we have a nested function, in which we would like to preserve the
context of `this` from its lexical scope:

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.prefixName = function (arr) {
    return arr.map(function (character) {
        return this.name + character; // Cannot read property 'name' of undefined
    });
};
```

Using **Arrow Functions**, the lexical value of `this` isn't shadowed and we
can re-write the above as shown:

```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.prefixName = function (arr) {
    return arr.map(character => this.name + character);
};
```

If an arrow is inside another function, it shares the "arguments" variable of its parent function.
Example:

```javascript
// Lexical arguments
function square() {
  let example = () => {
    let numbers = [];
    for (let number of arguments) {
      numbers.push(number * number);
    }

    return numbers;
  };

  return example();
}

square(2, 4, 7.5, 8, 11.5, 21); // returns: [4, 16, 56.25, 64, 132.25, 441]
```

## Classes

Prior to ES6, we implemented Classes by creating a constructor function and
adding properties by extending the prototype:

```javascript
function Person(name, age, gender) {
    this.name   = name;
    this.age    = age;
    this.gender = gender;
}

Person.prototype.incrementAge = function () {
    return this.age += 1;
};
```

And created extended classes by the following:

```javascript
function Personal(name, age, gender, occupation, hobby) {
    Person.call(this, name, age, gender);
    this.occupation = occupation;
    this.hobby = hobby;
}

Personal.prototype = Object.create(Person.prototype);
Personal.prototype.constructor = Personal;
Personal.prototype.incrementAge = function () {
    Person.prototype.incrementAge.call(this);
    this.age += 20;
    console.log(this.age);
};
```

ES6 classes are a simple sugar over the prototype-based OO pattern. Classes support prototype-based inheritance, super calls, instance and static methods and constructors:

```javascript
class Person {
    constructor(name, age, gender) {
        this.name   = name;
        this.age    = age;
        this.gender = gender;
    }

    incrementAge() {
      this.age += 1;
    }
}
```

And extend them using the `extends` keyword:

```javascript
class Personal extends Person {
    constructor(name, age, gender, occupation, hobby) {
        super(name, age, gender);
        this.occupation = occupation;
        this.hobby = hobby;
    }

    incrementAge() {
        super.incrementAge();
        this.age += 20;
        console.log(this.age);
    }
}
```

## Destructuring

### Destructuring assignment

Destructuring assignment allows you to assign the properties of an array or object to variables using syntax that looks similar to array or object literals.

Old way:
```javascript
var first = someArray[0];
var second = someArray[1];
var third = someArray[2];
```

New way:
```javascript
var [first, second, third] = someArray;
```

### Destructuring arrays and iterables

If you want to declare your variables at the same time, you can add a `var`, `let`, or `const` in front of the assignment.

```javascript
var [ variable1, variable2, ..., variableN ] = array;
let [ variable1, variable2, ..., variableN ] = array;
const [ variable1, variable2, ..., variableN ] = array;
```

You can skip over items in the array being destructured:

```javascript
var [,,third] = ["foo", "bar", "baz"];
console.log(third);
// "baz"
```

You can capture all trailing items in an array with a “rest” pattern:

```javascript
var [head, ...tail] = [1, 2, 3, 4];
console.log(tail);
// [2, 3, 4]
```

### Destructuring objects

Old way of destructuring an object:

```javascript
var person = { first_name: 'Joe', last_name: 'Appleseed' };
var first_name = person.first_name; // 'Joe'
var last_name = person.last_name; // 'Appleseed'
```

New way of destructuring an object:

```javascript
let person = { first_name: 'Joe', last_name: 'Appleseed' };
let {first_name, last_name} = person;

console.log(first_name); // 'Joe'
console.log(last_name); // 'Appleseed'
```

When you destructure on properties that are not defined, you get undefined:

```javascript
var { missing } = {};
console.log(missing); // undefined
```

## Spread Operator

The spread syntax is simply three dots: `...`
It allows an iterable to expand in places where 0+ arguments are expected.

### Calling Functions without Apply:

```javascript
function doStuff (x, y, z) { }
var args = [0, 1, 2];

// Call the function, passing args
doStuff.apply(null, args);
```

Using spread operator: 

```javascript
doStuff(...args);
```

Or another example using Math functions:

```javascript
const arr = [2, 4, 8, 6, 0];
const max = Math.max(...arr);

console.log(max); //8
```

### Combine arrays

```javascript
let mid = [3, 4];
let arr = [1, 2, ...mid, 5, 6]; //[1, 2, 3, 4, 5, 6]
```

### Copy arrays

```javascript 
var arr = [1,2,3];
var arr2 = [...arr]; // like arr.slice()
arr2.push(4)
```

## Getters/Setters

ES6 has started supporting getter and setter functions within classes. Using the following example:

```javascript
class Person {

    constructor(name) {
        this._name = name;
    }

    get name() {
      if(this._name) {
        return this._name.toUpperCase();  
      } else {
        return undefined;
      }  
    }

    set name(newName) {
      if (newName == this._name) {
        console.log('I already have this name.');
      } else if (newName) {
        this._name = newName;
      } else {
        return false;
      }
    }
}

let person = new Person("John Doe");

// uses the get method in the background
if (person.name) {
  console.log(person.name);  // John Doe
}

// uses the setter in the background
person.name = "Jane Doe";
console.log(person.name);  // Jane Doe 
```

## Modules

Prior to ES6, we used libraries such as [Browserify](http://browserify.org/)
to create modules on the client-side, and [require](https://nodejs.org/api/modules.html#modules_module_require_id)
in **Node.js**. With ES6, we can now directly use modules of all types
(AMD and CommonJS).

### Exporting in CommonJS

```javascript
module.exports = 1;
module.exports = { foo: 'bar' };
module.exports = ['foo', 'bar'];
module.exports = function bar () {};
```

### Exporting in ES6

**Named Exports**:

```javascript
export function multiply (x, y) {
  return x * y;
};
```

As well as **exporting a list** of objects:

```javascript
function add (x, y) {
  return x + y;
};

function multiply (x, y) {
  return x * y;
};

export { add, multiply };
```

**Default export**:

In our module, we can have many named exports, but we can also have a default export. It’s because our module could be a large library and with default export we can import then an entire module.

Important to note that there's only `one default export per module`. 

```javascript
export default function (x, y) {
  return x * y;
};
```

This time we don’t have to use curly braces for importing and we have a chance to name imported statement as we wish.

```javascript
import multiply from 'module';
// === OR ===
import whatever from 'module';
```

A module can have both named exports and a default export:

```javascript
// module.js
export function add (x, y) {
  return x + y;
};
export default function (x, y) {
  return x * y;
};

// app.js
import multiply, { add } from 'module';
```

The default export is just a named export with the special name default.

```javascript
// module.js
export default function (x, y) {
  return x * y;
};

// app.js
import { default } from 'module';
```

### Importing in ES6

```javascript
import { add } from 'module';
```

We can even import many statements:

```javascript
import { add, multiply } from 'module';
```

Imports may also be **aliased**:

```javascript
import { 
  add as addition, 
  multiply as multiplication
} from 'module';
```

and use wildcard (`*`) to import all exported statemets:

```javascript
import * from 'module';
```

## Data Structures

### Map

A `Map` is a data structure allows to associate data to a key.

Before it's intruduction in ES6, people generally used objects as maps, by associating some object or value to a specific key value:

```javascript
const person = {}
person.name = 'John'
person.age = 18

console.log(person.name) //John
console.log(person.age) //18
```

`Map` example:

```javascript
const person = new Map()

person.set('name', 'John')
person.set('age', 18)

const name = person.get('name')
const age = person.get('age')

console.log(name) //John
console.log(age) //18
```

The `Map` also provide us with methods to help us manage the data.

`delete()` method - deletes an item from a map by key:
```javascript
person.delete('name')
```

`clear()` method - delete all items from a map:
```javascript
person.clear()
```

`has()` method - check if a map contains an item by key:
```javascript
const hasName = person.has('name')
```

`size()` method - check the number of items in a map:
```javascript
const size = person.size
```

We can also use a couple of methods to iterate:

`entries()` — get all entries
`keys()` — get only all keys
`values()` — get only all values

Find more details about `Map` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

### WeakMap

A `WeakMap` is a special kind of map.

In a `Map`, items are never garbage collected. A `WeakMap` instead lets all its items be freely garbage collected. Every key of a `WeakMap` is an object. When the reference to this object is lost, the value can be garbage collected.

Main differences between `WeakMap` and `Map`:

 - you cannot iterate over the keys or values (or key-values) of a WeakMap
 - you cannot clear all items from a WeakMap
 - you cannot check its size
 
A WeakMap exposes those methods, which are equivalent to the Map ones:

```javascript
get(k)
set(k, v)
has(k)
delete(k)
```

The use cases of a `WeakMap` are less evident than the ones of a `Map`, and you might never find the need for them, but essentially it can be used to build a memory-sensitive cache that is not going to interfere with garbage collection, or for careful encapsualtion and information hiding.

Find more details about `WeakMap` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)

### Set

A `Set` is a collection for unique values. The values can be primitives or object references.

```javascript
let set = new Set();
set.add(1);
set.add('1');
set.add({ key: 'value' });
console.log(set); // Set {1, '1', Object {key: 'value'}}
```

Most importantly is that it does not allow duplicate values, one good use if to remove duplicate values from an array:

```javascript
[ ...new Set([1, 2, 3, 1, 2, 3]) ] // [1, 2, 3]
```

Iteration using built-in method **forEach** and **for..of**:

```javascript
//forEach
let set = new Set([1, '1', { key: 'value' }]);
set.forEach(function (value) {
  console.log(value);
  // 1
  // '1'
  // Object {key: 'value'}
});
```

```javascript
// for..of
let set = new Set([1, '1', { key: 'value' }]);
for (let value of set) {
  console.log(value);
  // 1
  // '1'
  // Object {key: 'value'}
};
```

Similar to `Map`, `Set` provides us with methods such as `has()`, `delete()`, `clear()`.

Find more details about `Set` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

### WeakSet

Like a `WeakMap`, `WeakSet` is a `Set` that doesn’t prevent its values from being garbage-collected. It has simpler API than `WeakMap`, because has only three methods:

```javascript
new WeakSet([iterable])
WeakSet.prototype.add(value)    : any
WeakSet.prototype.has(value)    : boolean
WeakSet.prototype.delete(value) : boolean
```

Important thing to note `WeakSet` is a collection that can‘t be iterated and whose size cannot be determined.

## Helpful string functions

### .includes( )

```javascript
var string = 'string';
var substring = 'str';

console.log(string.indexOf(substring) > -1);
```

Instead of checking for a return value `> -1` to denote string containment,
we can simply use `.includes()` which will return a boolean:

```javascript
const string = 'string';
const substring = 'str';

console.log(string.includes(substring)); // true
```

### .repeat( )

```javascript
function repeat(string, count) {
    var strings = [];
    while(strings.length < count) {
        strings.push(string);
    }
    return strings.join('');
}
```

In ES6, we now have access to a terser implementation:

```javascript
// String.repeat(numberOfRepetitions)
'str'.repeat(3); // 'strstrstr'
```

## Helpful array functions

`from`
```javascript
const inventory = [
    {name: 'mars', quantity: 2},
    {name: 'snickers', quantity: 3}
];
console.log(Array.from(inventory, item => item.quantity + 2)); // [4, 5]
```

`of`
```javascript
Array.of("Twinkle", "Little", "Star"); // returns ["Twinkle", "Little", "Star"]
```

`find`
```javascript
const inventory = [
    {name: 'mars', quantity: 2},
    {name: 'snickers', quantity: 3}
];
console.log(inventory.find(item => item.name === 'mars')); // {name: 'mars', quantity: 2}
```

`findIndex`
```javascript
const inventory = [
    {name: 'mars', quantity: 2},
    {name: 'snickers', quantity: 3}
];
console.log(inventory.find(item => item.name === 'mars')); // 0
```

`fill` method takes up to three arguments value, start and end. The start and end arguments are optional with default values of 0 and the length of the this object.
```javascript
[1, 2, 3].fill(1); // [1, 1, 1]
[1, 2, 3].fill(4, 1, 2); // [1, 4, 3]
```

## Promises

Promises are one of the most exciting additions to JavaScript ES6. Promises are a pattern that greatly simplifies asynchronous programming by making the code look synchronous and avoid problems associated with callbacks.

Prior to ES6, we used [bluebird](https://github.com/petkaantonov/bluebird) or
[Q](https://github.com/kriskowal/q). Now we have Promises natively.

`A Promise is an object that is used as a placeholder for the eventual results of a deferred (and possibly asynchronous) computation.`

The `resolve` and `reject` are functions themselves and are used to send back values to the promise object.

```javascript
const myPromise = new Promise((resolve, reject) => {
    if (Math.random() * 100 <= 90) {
        resolve('Hello, Promises!');
    }
    reject(new Error('In 10% of the cases, I fail. Miserably.'));
});

myPromise.then((resolvedValue) => {
    console.log(resolvedValue); //Hello, Promises!
}, (error) => {
    console.log(error); //In 10% of the cases, I fail. Miserably.
});
```

**Chaining Promises**:

Promises allow us to turn our horizontal code (callback hell):

```javascript
func1(function (value1) {
    func2(value1, function (value2) {
        func3(value2, function (value3) {
          // Do something with value 3
        });
    });
});
```

Into vertical code like so:

```javascript
func1(value1)
    .then(func2)
    .then(func3, value3 => {
        // Do something with value 3
    });
```

**Parallelize Promises**:

We can use `Promise.all()` to handle an array of asynchronous
operations.

```javascript
let urls = [
  '/api/commits',
  '/api/issues/opened',
  '/api/issues/assigned',
  '/api/issues/completed',
  '/api/issues/comments',
  '/api/pullrequests'
];

let promises = urls.map((url) => {
  return new Promise((resolve, reject) => {
    $.ajax({ url: url })
      .done((data) => {
        resolve(data);
      });
  });
});

Promise.all(promises)
  .then((results) => {
    // Do something with results of all our promises
 });
```

## Generators

A `generator` is a function which can be exited and later re-entered. Their context (variable bindings) will be saved across re-entrances.

Generators in JavaScript are a very powerful tool for asynchronous programming as they mitigate the problems with callbacks, such as `Callback Hell` and `Inversion of Control`. 

This pattern is what `async` functions are built on top of.

For creating a generator function, we use `function *` syntax instead of just `function`.

Calling a generator function does not execute its body immediately; an iterator object for the function is returned instead.
When the iterator's `next()` method is called, the generator function's body is executed until the first `yield` expression, which specifies the value to be returned from the iterator or, with `yield*`, delegates to another generator function. 

The `next()` method returns an object with a value property containing the yielded value and a done property which indicates whether the generator has yielded its last value as a boolean. Calling the `next()` method with an argument will resume the generator function execution, replacing the yield expression where execution was paused with the argument from `next()`. 

**Simple example**:

```javascript
function* generator(i) {
  yield i;
  yield i + 10;
}

var gen = generator(10);

console.log(gen.next().value);// expected output: 10
console.log(gen.next().value); // expected output: 20
```

**Example with yield***:

```javascript
function* anotherGenerator(i) {
  yield i + 1;
  yield i + 2;
  yield i + 3;
}

function* generator(i) {
  yield i;
  yield* anotherGenerator(i);
  yield i + 10;
}

var gen = generator(10);

console.log(gen.next().value); // 10
console.log(gen.next().value); // 11
console.log(gen.next().value); // 12
console.log(gen.next().value); // 13
console.log(gen.next().value); // 20
```

**Infinite Data generator example**:

```javascript
function * naturalNumbers() {
  let num = 1;
  while (true) {
    yield num;
    num = num + 1
  }
}
const numbers = naturalNumbers();
console.log(numbers.next().value)
console.log(numbers.next().value)
// 1
// 2
```

## Async/Await

Async/Await is a ES2016 feature.

Note that `await` may only be used in functions marked with the `async` keyword. It works similarly to generators, suspending execution in your context until the promise settles. If the awaited expression isn’t a promise, its casted into a promise.

 `async await` allows us to perform the same thing we accomplished using Generators and Promises with less effort:

```javascript
var request = require('request');

function getJSON(url) {
  return new Promise(function(resolve, reject) {
    request(url, function(error, response, body) {
      resolve(body);
    });
  });
}

async function main() {
  var data = await getJSON();
  console.log(data); // NOT undefined!
}

main();
```

Under the hood, it performs similarly to Generators. I highly recommend using them over Generators + Promises.
