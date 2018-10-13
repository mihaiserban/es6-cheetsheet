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
## Destructuring
## Spread Operator
## Function Parameters
## Getters/Setters
## Modules
## Data Structures
## Helpful string functions
## Helpful array functions
## Promises
## Async/Await
