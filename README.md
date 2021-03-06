# babel-plugin-holes

[![NPM Version](https://img.shields.io/npm/v/babel-plugin-holes.svg?style=flat)](https://www.npmjs.org/package/babel-plugin-holes)
[![NPM Downloads](https://img.shields.io/npm/dm/babel-plugin-holes.svg?style=flat)](https://www.npmjs.org/package/babel-plugin-holes)

This plugin is made to bring the powerful expressivity of Scala functions to JavaScript.
It provides a powerful system of holes to help on functional programming and function
composition, enforcing the point-free style.

It's very well tested and has consistent rules that shouldn't break when used with other
plugins. You can also abstract the native JavaScript operations with it without overheads!

## Examples

```js
// Binary operators
const add = _ + _
const increment = _ + 1

console.log(add(100, 200)) // 300
console.log(increment(1000)) // 1001


// Unary operators
const negate = -_

console.log(negate(10)) // -10


// Operators as functions
const at = _[_]
const call = _(_)

console.log(at([5, 10, 15], 1)) // 10
call(console.log, 'Hello!') // 'Hello!'


// Identity function
const id = _

console.log(id('frobnicate')) // 'frobnicate'


// Method calls
const toString = _.toString()
const toHexString = _.toString(16)

console.log(toString(10)) // 10
console.log(toHexString(10)) // 'a'


// Binary property access
const goodPassword = _.length >= 8
const ageSum = _.age + _.age

console.log(goodPassword('dolphins')) // true
console.log(ageSum({ age: 21 }, { age: 30 })) // 51


// Binary method call
const ellipsize = _.toString() + '...'

console.log(ellipsize(100)) // 100...
```

It works on identifiers, simple members, computed members, calls, binary and unary
expressions, so you can compose functions that take more parameters, for example:

```js
const assocOne = assoc(_, 1, _)

console.log(assocOne('value', {})) // { value: 1 }
```

## Disabling in current scope

If you want to use the original underscore, you can disable this plugin in
current scope (and its children scopes) using `'no holes'` directive.

## Options

You can pass your own curry function with `{ curry: 'curry' }`. The generated functions
will use it, so:

```json
{
    "plugins": ["holes", { "curry": "curry" }]
}
```

```js
const add = _ + _
```

will emit:

```js
const add = curry((_2, _3) => _2 + _3)
```

## Installation

```sh
$ npm install --save-dev babel-plugin-holes
```

## Usage

### Via `.babelrc` (Recommended)

**.babelrc**

```json
{
  "plugins": ["holes"]
}
```

### Via CLI

```sh
$ babel --plugins holes script.js
```

### Via Node API

```javascript
require('babel-core').transform('code', {
  plugins: ['holes']
});
```

## Observation

Using `++` and `--` has no warranties because Babel code generator has a bug that emits wrong
code for a correct AST node. Their use is **not recommended**.

# License

MIT
