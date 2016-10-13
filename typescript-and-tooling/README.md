# Tooling and TypeScript


---

# Tooling

- Traditionally web pages have been just static HTML, CSS and maybe some simple JS for dropdowns etc.
- Nowadays massive SPAs require something more advanced and thus the need for tooling
- Some basic needs for tooling:
  - Compiling ES6/TypeScript -> ES5 and LESS/SASS -> CSS
  - Combining multiple source files into single bundle file for faster loading
  - Running test suites
  - Optimizations (minification, Dead code elimination, tree shaking)

---

# Node.js & npm

- Node.js is JavaScript interpreter built on top of Chrome's V8 JavaScript engine
- npm (node package manager) is the package manager for Node
  - More packages than on any other package manager for any other language: over 270k (May 2016) ([modulecounts.com](http://www.modulecounts.com/))
- Usage on command line:

```shell
# Execute JavaScript file
node my-js-file.js
```

---

# npm project configuration: package.json
- Declares dependencies, development-time dependencies and commands available
- Can be generated with `npm init`

```shell
npm install # install dependencies (--production for only "dependencies")
npm build # build codes (eg. TypeScript -> JavaScript)
npm start # and run start script
```

```json
{
  "name": "My project",
  "author": "My company",
  "scripts": {
    "build": "my-build.sh",
    "start": "my-webserver.sh"
  },
  "dependencies": {
    "@angular/core": "2.0.0",
    "@angular/forms": "2.0.0",
    "@angular/http": "2.0.0"
  },
  "devDependencies": {
    "typescript": "^2.0.0",
    "jasmine": "^2.5.0"
  }
}
```

---

# Webpack

- Widely used module bundler for web projects
- Configurable with loaders and plugins
- Handles both source code and assets like images and stylesheets
- Allows you to write modular applications

---

# Angular CLI

- Command-line interface for Angular 2 development
- Supports generation of project, components, services, pipes etc.
- Recommended by the core team as basis for Angular 2 application
- Usage with `ng` command:

```shell
ng new PROJECT_NAME
cd PROJECT_NAME
ng serve
```

Angular app now running on `localhost:4200`.


---

# JSON
- JavaScript Object Notation (JSON) is lightweight data-interchange format
- Meant to be easy for both, humans and machines
- Key-value pairs, where values can be any JavaScript primitives (except functions)

```json
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "friends": [
    {"name": "Jane Doe"},
    {"name": "John Doe Jr."}
  ]
}
```

---

# ES6
- Newest version of EcmaScript standard that is the basis for JavaScript
- Published 2015 (called also ES2015 because of that)
- Provides a lot of improvements for writing JavaScript in scale

---

# ES6 - Key Features
- `let` and `const` to replace `var`
- Arrow functions
- Multiline strings
- Modules
- Enhancements on basic types such as `includes()` for string and `find()` for array


---

# Problem: Variable Hoisting

Totally unexpected behavior called variable hoisting:

```typescript
if (true) {
  var a = 0;
}
console.log(a);
```

---

# Let and const
- `let` and `const` work as usual
- `let` declares variable (eg. `let myVar = 'asd';`)
- `const` declares constant **reference** (!= constant value)

```javascript
const input = [0, 1, 2, 3, 4];
input = []; // Uncaught TypeError: Assignment to constant variable.
input.push(5); // Works, as input is just the reference
```

For immutable objects & arrays there are libraries such as [_Immutable.js_](https://facebook.github.io/immutable-js/).

**Rule of thumb: Always use `const` if possible, `let` otherwise.**

---

# Problem: `this` not lexical

```html
<button>Do something</button>
```

```typescript
var user = {
    data: 'foo',
    clickHandler: function (event) {
      console.log(this.data);
    }
};

document.querySelector('button').addEventListener('click', user.clickHandler); // undefined
document.querySelector('button').addEventListener('click', user.clickHandler.bind(user)); // foo
```

---

# Arrow functions
- Lexical `this` and nicer syntax
- Traditional function (for each `func(2)` returns 3):

```javascript
const func = function (param1) {
  return param1 + 1;
};
```

- Arrow function:

```javascript
const func = (param1) => {
  return param1 + 1;
};
```

- Parenthesis can be omitted if only one parameter:

```javascript
const func = param1 => {
  return param1 + 1;
};
```

- Curly brackets and `return` can be emitted if returning simple value:

```javascript
const func = param1 => param1 + 1;
```

---

# Multiline strings
ES5 string:
```javascript
const str = "SO LOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOOONG MULTILINE" +
"STRING" +
"!"
```

ES6 multiline string with backticks (`):
```javascript
const str = `MULTILINE
STRING
!´
```

Also supports string interpolation
```javascript
const firstName = 'John';
const lastName = 'Doe';
const str = `${lastName}, ${firstName}´;
//Doe, John
```

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

# TypeScript

- Built by Microsoft to build JavaScript in scale
- Initial release in October 2012
- Typed superset of JavaScript -> Any valid JS is valid TypeScript
- Advantages:
  - Type system on top of JavaScript to catch errors already on compile-time
  - Angular 2 is written in TypeScript

---

# Typing
- Provides the same types as in JavaScript: `number`, `string`, `boolean`, `null`, `undefined` and `object`.
- Also some "extra" types such as `any`, `void` and `enum`
- Arrays like `number[]`, `string[]` and `any[]`
- `any` is basically anything like `number`, `string` or `any[]`
- Example:

```typescript
let luckyNumber: number = 50;
let name: string = 100; // TypeError: Can't assign number to string
const anything: any = {}; // Anything accepts any type
```

---

# Interfaces
- Interfaces to declare the acceptable object structures
- Can have optional properties (declared with `?` before `:`)

```typescript
interface Person {
    firstName: string;
    lastName?: string;
}

greeter(person: Person): string => {
    return "Hello, " + person.firstName + " " + person.lastName;
}

greeter({ firstName: "Jane", lastName: "User" });
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

greeter(person: Person) => {
    return "Hello, " + person.firstName + " " + person.lastName;
}

const student = new Student("Jane", "M.", "User");
greeter(student);
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
