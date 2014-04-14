# Node.js Style Guide

This is a guide for writing consistent and aesthetically pleasing node.js code.
It is inspired by [Felix Geisendörfer](https://github.com/felixge/node-style-guide) with some modifications for company practices.


## 4 Spaces for indentation

Use 4 spaces for indenting your code and swear an oath to never mix tabs and
spaces - a special kind of hell is awaiting you otherwise.

## Newlines

Use UNIX-style newlines (`\n`), and a newline character as the last character
of a file. Windows-style newlines (`\r\n`) are forbidden inside any repository.

## No trailing whitespace

Just like you brush your teeth after every meal, you clean up any trailing
whitespace in your JS files before committing. Otherwise the rotten smell of
careless neglect will eventually drive away contributors and/or co-workers.

## Use Semicolons

According to [scientific research][hnsemicolons], the usage of semicolons is
a core values of our community. Consider the points of [the opposition][], but
be a traditionalist when it comes to abusing error correction mechanisms for
cheap syntactic pleasures.

[the opposition]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[hnsemicolons]: http://news.ycombinator.com/item?id=1547647

## 120 characters per line

Limit your lines to 120 characters. Yes, screens have gotten much bigger over the last few years, but your brain has not. This allows you to use the spare room for split screen awesomeness.

## Use single quotes

Use single quotes, unless you are writing JSON.

*Right:*

```js
var foo = 'bar';
```

*Wrong:*

```js
var foo = "bar";
```

## Opening braces go on the same line

Your opening braces go on the same line as the statement.

*Right:*

```js
if (true) {
  console.log('winning');
}
```

*Wrong:*

```js
if (true)
{
  console.log('losing');
}
```

Also, notice the use of whitespace before and after the condition statement.

## Combine variable declarations

Declare one var per function because:
- JSLint and possibly other linters will complain about multiple var keywords per scope.
- A single var keyword minifies better than many, for obvious reasons.
- Combining declarations forces you to declare the variables in one place, in the beginning of the function.
 
For variables that aren’t initialized, they should appear last.

*Right:*

```js
var keys   = ['foo', 'bar'],
    values = [23, 42],
    object = {},
    key = keys.pop(),
    index;
```

*Wrong:*

```js
var keys = ['foo', 'bar'];
var values = [23, 42];
var object = {};
var key = ['foo', 'bar'];
```


## Use lowerCamelCase for variables, properties and function names

Variables, properties and function names should use `lowerCamelCase`.  They
should also be descriptive. Single character variables and uncommon
abbreviations should generally be avoided.

*Right:*

```js
var adminUser = db.query('SELECT * FROM users ...');
```

*Wrong:*

```js
var admin_user = db.query('SELECT * FROM users ...');
```

## Use UpperCamelCase for class names

Class names should be capitalized using `UpperCamelCase`.

*Right:*

```js
function BankAccount() {
}
```

*Wrong:*

```js
function bank_Account() {
}
```

## Use UPPERCASE for Constants

Constants should be declared as regular variables or static class properties,
using all uppercase letters.

Node.js / V8 actually supports mozilla's [const][const] extension, but
unfortunately that cannot be applied to class members, nor is it part of any
ECMA standard.

*Right:*

```js
var SECOND = 1 * 1000;

function File() {
}
File.FULL_PERMISSIONS = 0777;
```

*Wrong:*

```js
const SECOND = 1 * 1000;

function File() {
}
File.fullPermissions = 0777;
```

[const]: https://developer.mozilla.org/en/JavaScript/Reference/Statements/const

## Object / Array creation

Use trailing commas and put *short* declarations on a single line. Only quote
keys when your interpreter complains:

*Right:*

```js
var a = ['hello', 'world'];
var b = {
  good: 'code',
  'is generally': 'pretty',
};
```

*Wrong:*

```js
var a = [
  'hello', 'world'
];
var b = {"good": 'code'
        , is generally: 'pretty'
        };
```

## Use the === operator

Programming is not about remembering [stupid rules][comparisonoperators]. Use
the triple equality operator as it will work just as expected.

*Right:*

```js
var a = 0;
if (a === '') {
  console.log('winning');
}

```

*Wrong:*

```js
var a = 0;
if (a == '') {
  console.log('losing');
}
```

[comparisonoperators]: https://developer.mozilla.org/en/JavaScript/Reference/Operators/Comparison_Operators

## Use multi-line ternary operator

The ternary operator should be used on a single line. Do not split it up into multiple lines instead.

*Right:*

```js
var foo = (a === b) ? 1 : 2;
```

*Wrong:*

```js
var foo = (a === b)
  ? 1
  : 2;
```

## Do not extend built-in prototypes

Do not extend the prototype of native JavaScript objects. Your future self will
be forever grateful.

*Right:*

```js
var a = [];
if (!a.length) {
  console.log('winning');
}
```

*Wrong:*

```js
Array.prototype.empty = function() {
  return !this.length;
}

var a = [];
if (a.empty()) {
  console.log('losing');
}
```

## Use descriptive conditions

Any non-trivial conditions should be assigned to a descriptively named variable or function:

*Right:*

```js
var isValidPassword = password.length >= 4 && /^(?=.*\d).{4,}$/.test(password);

if (isValidPassword) {
  console.log('winning');
}
```

*Wrong:*

```js
if (password.length >= 4 && /^(?=.*\d).{4,}$/.test(password)) {
  console.log('losing');
}
```

## Write small functions

Keep your functions short. A good function fits on a slide that the people in
the last row of a big room can comfortably read. So don't count on them having
perfect vision and limit yourself to ~15 lines of code per function.

## Return early from functions

To avoid deep nesting of if-statements, always return a function's value as early
as possible.

*Right:*

```js
function isPercentage(val) {
  if (val < 0) {
    return false;
  }

  if (val > 100) {
    return false;
  }

  return true;
}
```

*Wrong:*

```js
function isPercentage(val) {
  if (val >= 0) {
    if (val < 100) {
      return true;
    } else {
      return false;
    }
  } else {
    return false;
  }
}
```

Or for this particular example it may also be fine to shorten things even
further:

```js
function isPercentage(val) {
  var isInRange = (val >= 0 && val <= 100);
  return isInRange;
}
```

## Name your closures

Feel free to give your closures a name. It shows that you care about them, and
will produce better stack traces, heap and cpu profiles.

*Right:*

```js
req.on('end', function onEnd() {
  console.log('winning');
});
```

*Wrong:*

```js
req.on('end', function() {
  console.log('losing');
});
```

## No nested closures

Use closures, but don't nest them. Otherwise your code will become a mess.

*Right:*

```js
setTimeout(function() {
  client.connect(afterConnect);
}, 1000);

function afterConnect() {
  console.log('winning');
}
```

*Wrong:*

```js
setTimeout(function() {
  client.connect(function() {
    console.log('losing');
  });
}, 1000);
```



## Comments

Use JSDoc

We follow the C++ style for comments in spirit.

All files, classes, methods and properties should be documented with JSDoc comments with the appropriate tags and types. Textual descriptions for properties, methods, method parameters and method return values should be included unless obvious from the property, method, or parameter name.

Inline comments should be of the // variety.

Complete sentences are recommended but not required. Complete sentences should use appropriate capitalization and punctuation.

### Comment Syntax

The JSDoc syntax is based on JavaDoc. Many tools extract metadata from JSDoc comments to perform code validation and optimizations. These comments must be well-formed.
```
/**
 * A JSDoc comment should begin with a slash and 2 asterisks.
 * Inline tags should be enclosed in braces like {@code this}.
 * @desc Block tags should always start on their own line.
 */
```

### JSDoc Indentation

If you have to line break a block tag, you should treat this as breaking a code statement and indent it four spaces.
```
/**
 * Illustrates line wrapping for long param/return descriptions.
 * @param {string} foo This is a param with a description too long to fit in
 *     one line.
 * @return {number} This returns something that has a description too long to
 *     fit in one line.
 */
project.MyClass.prototype.method = function(foo) {
  return 5;
};
```
You should not indent the @fileoverview command. You do not have to indent the @desc command.

Even though it is not preferred, it is also acceptable to line up the description.
```
/**
 * This is NOT the preferred indentation method.
 * @param {string} foo This is a param with a description too long to fit in
 *                     one line.
 * @return {number} This returns something that has a description too long to
 *                  fit in one line.
 */
project.MyClass.prototype.method = function(foo) {
  return 5;
};
```

### HTML in JSDoc

Like JavaDoc, JSDoc supports many HTML tags, like 
```
<code>, <pre>, <tt>, <strong>, <ul>, <ol>, <li>, <a>
```
, and others.

This means that plaintext formatting is not respected. So, don't rely on whitespace to format JSDoc:
```
/**
 * Computes weight based on three factors:
 *   items sent
 *   items received
 *   last timestamp
 */
```

It'll come out like this:

Computes weight based on three factors: items sent items received last timestamp

Instead, do this:
```
/**
 * Computes weight based on three factors:
 * <ul>
 * <li>items sent
 * <li>items received
 * <li>last timestamp
 * </ul>
 */
```
The JavaDoc style guide is a useful resource on how to write well-formed doc comments.

### Top/File-Level Comments

A copyright notice and author information are optional. File overviews are generally recommended whenever a file consists of more than a single class definition. The top level comment is designed to orient readers unfamiliar with the code to what is in this file. If present, it should provide a description of the file's contents and any dependencies or compatibility information. As an example:
```
/**
 * @fileoverview Description of file, its uses and information
 * about its dependencies.
 */
```

### Class Comments

Classes must be documented with a description and a type tag that identifies the constructor.
```
/**
 * Class making something fun and easy.
 * @param {string} arg1 An argument that makes this more interesting.
 * @param {Array.<number>} arg2 List of numbers to be processed.
 * @constructor
 * @extends {goog.Disposable}
 */
project.MyClass = function(arg1, arg2) {
  // ...
};
goog.inherits(project.MyClass, goog.Disposable);
```

### Method and Function Comments

Parameter and return types should be documented. The method description may be omitted if it is obvious from the parameter or return type descriptions. Method descriptions should start with a sentence written in the third person declarative voice.
```
/**
 * Operates on an instance of MyClass and returns something.
 * @param {project.MyClass} obj Instance of MyClass which leads to a long
 *     comment that needs to be wrapped to two lines.
 * @return {boolean} Whether something occurred.
 */
function PR_someMethod(obj) {
  // ...
}
```

### Property Comments
```
/** @constructor */
project.MyClass = function() {
  /**
   * Maximum number of things per pane.
   * @type {number}
   */
  this.someProperty = 4;
}
```

### JSDoc Tag Reference

Head over to [JSDoc 3 Tag Dictionary](http://usejsdoc.org/#JSDoc3_Tag_Dictionary) for more information.



## Object.freeze, Object.preventExtensions, Object.seal, with, eval

Crazy shit that you will probably never need. Stay away from it.

## Getters and setters

Do not use setters, they cause more problems for people who try to use your
software than they can solve.

Feel free to use getters that are free from [side effects][sideeffect], like
providing a length property for a collection class.

[sideeffect]: http://en.wikipedia.org/wiki/Side_effect_(computer_science)

## Use `use strict`


Use `use strict`. It helps out in a couple ways:

+ it catches some common coding bloopers by throwing exceptions;
+ it prevents relatively "unsafe" action (such as gaining access to the global object);
+ it disabled features that are confusing or poorly thought out;

More thorough explanation [here][strictmode].


[strictmode]: http://ejohn.org/blog/ecmascript-5-strict-mode-json-and-more/

## Always use the Radix

Everytime `parseInt` is used, one needs to make sure to provide the _radix_ to
avoid [unexpected results][parseint].

*Right:*

```js
parseInt("0x8", 10); // 0
```

*Wrong:*

```js
parseInt("0x8"); // 8
```

[parseint]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt

## Naming

All variable names should be as specific as possible.
Meaningless names should be avoided. Names such as `foo`, `bar`, and `temp`, despite being
part of the developer’s toolbox, don’t give any meaning to variables. There’s no way
for another developer to understand what the variable is being used for without understanding
all of the context.

*Right:*

```js
var count = 2;
var myName = 'Betfair';
var valid = true;
```

*Wrong:*

```js
// Easily confused with functions
var getCount = 10;
var isValid = true;
```

*Right:*

```js
function getName() {
  return myName;
}
```

*Wrong:*

```js
// Easily confused with variable
function theName() {
   return myName;
}
```

*Wrong:*

```js
// Non sense
var thisIsMyRandomName = 10;
var dx = null;
var x = 'Betfair'; 
```
