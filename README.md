# es6-cheetsheet
ES2015, ES2016, ES2017 and beyond.

## TABLE OF CONTENTS
- [Variable Declarations](#variable-declarations)
- [String Templates and Padding](#string-templates-and-padding)
- [Destructuring](#destructing)
- [Spread Operator](#spread-operator)
- [Arrow Functions](#arrow-functions)
- [Function Parameters](#function-parameters)
- [Classes](#classes)
- [Getters/Setters](#getters-setters)
- [Modules](#modules)
- [Data Structures](#data-structures)
- [Helpful array functions](#helpful-array-functions)
- [Promises](#promises)
- [Async/Await](#async-await)

## Variable Declarations

`let` is the new `var`. Constants work just like `let`, but can’t be reassigned.
`let` and `const` are block scoped. Therefore, referencing
block-scoped identifiers before they are defined will produce
a `ReferenceError`.
