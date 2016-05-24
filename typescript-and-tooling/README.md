# Tooling and TypeScript


---

# Tooling

- Traditionally web pages have been just static HTML, CSS and maybe some simple JS for dropdowns etc.
- Nowadays massive SPAs require something more advanced and thus the need for tooling
- Some basic needs for tooling:
  - Compiling ES6/TypeScript -> ES5 and LESS/SASS -> CSS
  - Combining multiple source files into single bundle file for faster loading
  - Running test suites
  - Performance optimizations

---

# Node.js & npm

- Node.js is JavaScript interpreter built on top of Chrome's V8 JavaScript engine
- npm (node package manager) is the package manager for Node
  - More packages than on any other package manager for any other language: over 270k (May 2016) ([modulecounts.com](http://www.modulecounts.com/))

---

# Webpack

- Widely used module bundler for web projects
- Configurable with loaders and plugins
- Handles both source code and assets like images and stylesheets
- Allows you to write modular

---

# ES6

- Newest version of EcmaScript standard that is the basis for JavaScript
- Published 2015
- Provides a lot of improvements for writing JavaScript in scale

---

# ES6 - Key Features

- Modules
- Arrow functions
- Classes
- Multiline strings
- `let` and `const` to replace `var`
- Enhancements on basic types such as `includes()` for string and `find()` for array

---

# Modules
Allows `import`ing and `export`ing code between files (modules)

_lib.js_
```javascript
export function square(x) {
   return x * x;
}
export function squareSum(x, y) {
   return Math.sqrt(square(x) + square(y));
}
```

_main.js_
```javascript
import { square, squareSum } from 'lib';
console.log(square(11)); // 121
console.log(squareSum(4, 3)); // 5
```

---

# Arrow functions
Traditional functions
```javascript
const myFunction = function (param1, param2) {
  return param1 + param2;
};
```

Arrow functions
```javascript
const myFunction = (param1, param2) => {
  return param1 + param2;
};
```

As lambda functions
```javascript
[1,2,3,4].map(`item => item * 2`)
```
which is the same as
```javascript
[1,2,3,4].map(`function (item) {`
  `return item * 2;`
`}`)
```

---

# Classes

```javascript
class MyClass {
  constructor(str) {
    this.myString = str;
  }

  getNumber() {
    return this.myNumber;
  }
}
```
```javascript
const myClass = new MyClass('Hello World');
```

Can be exported:

```javascript
export class MyClass { }
```

**TypeScript classes are a little different**

---

# Multiline strings
ES5 string:
```javascript
var str = "A VEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEERY LONG" +
"STRING" +
"!"
```

ES6 multiline string with backticks (`):
```javascript
const str = `A VEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEERY LONG
STRING
!
```

Also supports string interpolation
```javascript
const firstName = 'Roope';
const lastName = 'Hakulinen';
const str = `${lastName},
${firstName}`;
//Hakulinen,
//Roope
```


---

# Const and let

`const` keyword makes constant **reference**

```javascript
const input = [0, 1, 2, 3, 4];
input = []; // Uncaught TypeError: Assignment to constant variable.
input.push(5); // Works, as input is just the reference
```

For immutable objects & arrays there are libraries such as _Immutable.js_.

**Rule of thumb: Always use `const` if possible, `let` otherwise.**

---

# TypeScript

- Built by Microsoft to build JavaScript in scale
- Initial release October 2012
- Typed superset of JavaScript -> Any valid JS is valid TypeScript

---

# Advantages

- Type system on top of JavaScript to catch errors already compile-time
- Angular 2 is written in TypeScript

---

# Typing

- Provides the same types as in JavaScript: `number`, `string`, `boolean`, `null`, `undefined` and `object`.
- Also some "extra" types such as `any`, `void` and `enum`
- Arrays like `number[]`, `string[]` and `any[]`
- `any` is basically anything like `number`, `string` or `any[]`

---

# Interfaces

- Interfaces to declare the acceptable object structures
- Can have optional properties (declared with `?` before `:`)

```typescript
interface Person {
    firstName: string;
    lastName?: string;
}

function greeter(person: Person): string {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```

---

# Classes

- Like in other programming languages
  - can have constructors
  - implement interfaces
  - extend other classes
- Can be casted to other classes if properties match (same as for interfaces)

```typescript

interface Person {
    firstName: string;
    lastName: string;
}

class Student implements Person {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```

---

# Annotations
- Used to decorate classes and properties
- Like Java annotations

```typescript
@Component({
  template: 'my template'
})
class MyClass {
  @Input() myProperty: string;
}
```
