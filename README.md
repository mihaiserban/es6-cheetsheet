# es6-cheetsheet
ES2015, ES2016, ES2017 and beyond.

## TABLE OF CONTENTS
- [Variable Declarations](#variable-declarations)
- [String Templates](#string-templates)
- [Arrow Functions](#arrow-functions)
- [Classes](#classes)
- [Destructuring](#destructing)
- [Spread Operator](#spread-operator)
- [Function Parameters](#function-parameters)
- [Getters/Setters](#getters-setters)
- [Modules](#modules)
- [Data Structures](#data-structures)
- [Helpful string functions](#helpful-string-functions)
- [Helpful array functions](#helpful-array-functions)
- [Promises](#promises)
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

### Destructing assignment

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

Old way of destructing an object:

```javascript
var person = { first_name: 'Joe', last_name: 'Appleseed' };
var first_name = person.first_name; // 'Joe'
var last_name = person.last_name; // 'Appleseed'
```

New way of destructing an object:

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
## Function Parameters
## Getters/Setters
## Modules
## Data Structures
## Helpful string functions
## Helpful array functions
## Promises
## Async/Await
