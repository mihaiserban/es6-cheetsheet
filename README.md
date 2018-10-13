# es6-cheetsheet
ES2015, ES2016, ES2017 and beyond.

## TABLE OF CONTENTS
- [Variable Declarations](#variable-declarations)
- [String Templates](#string-templates)
- [Arrow Functions](#arrow-functions)
- [Destructuring](#destructing)
- [Spread Operator](#spread-operator)
- [Function Parameters](#function-parameters)
- [Classes](#classes)
- [Getters/Setters](#getters-setters)
- [Modules](#modules)
- [Data Structures](#data-structures)
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
