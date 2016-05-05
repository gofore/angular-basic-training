# TypeScript and tooling

---

# TypeScript
- Built by Microsoft to build JavaScript in scale
- Initial release October 2012
- Typed superset of JavaScript -> Any valid JS is valid TypeScript
---
# Advantages
- Adds type system on top of JavaScript to catch clear errors already on compile-time
- Angular 2 is written in TypeScript

---
# Typing
- Provides the same types as in JavaScript: `number`, `string`, `boolean`, `null`, `undefined` and `object`.
- Also some "extra" types such as `any`, `enum`, `void` and `tuple`

---
# Interfaces and classes

```typescript
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person): string {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```
---
```typescript
class Student {
    fullName: string;
    constructor(public firstName, public middleInitial, public lastName) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person : Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = new Student("Jane", "M.", "User");

document.body.innerHTML = greeter(user);
```
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
  - Most packages of any package manager for any language: over 270k ([modulecounts.com](http://www.modulecounts.com/))

---
# Webpack
- Widely used module bundler for web projects

---
# ES6
- Newest version of EcmaScript standard that is the basis for JavaScript
- Published 2015
- Provides a lot of improvements for writing JavaScript in scale

---
# Key features
- Modules
- Classes
- `let` and `const` to replace `var`
- Arrow functions
- Enhancements on basic types such as `includes()` for string and `find()` for array
---
# Few examples
`const` keyword and arrow functions:
```javascript
const input = [0, 1, 2, 3, 4];
console.log(input.map(item => item * 2)); // Prints [0, 2, 4, 6, 8]
```

```javascript
const haystack = 'abcde';
console.log(haystack.includes('bc')); // Prints true
```

```javascript
const people = [
  {id: 1, name: 'Bob'},
  {id: 2, name: 'Mary'}
];
console.log(people.find(person => person.name === 'Mary').id); // Prints 2
```
