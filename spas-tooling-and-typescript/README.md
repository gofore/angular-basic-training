# Angular Basic Training - Gofore

## Slides
Available at [angular-basic-training.herokuapp.com](http://angular-basic-training.herokuapp.com/)

---

## Agenda - Day 1
#### Morning
- Introduction to SPAs
- TypeScript & Tooling
- Angular Fundamentals

#### Afternoon
- Angular Fundamentals (continued)
- Angular Advanced Topics

---

## Agenda - Day 2
#### Morning
- Reactive Programming with Angular

#### Afternoon
- Testing

---

# Introduction to SPAs
- What is a SPA?
- Real-life examples
- Technical overview

---

# What Is a SPA?
- "A single-page application (SPA) is a web application or web site that fits on a single web page with the goal of providing a more fluid user experience similar to a desktop application." - Wikipedia
- Browser fetches executable code that makes asynchronous calls for actual data to be shown
- Data is visualized and/or manipulated and stored back on server asynchronously

---

![SPA flow](spas-tooling-and-typescript/spa-flow.png "SPA flow")

---

# Real-life Examples
- [Google search](http://www.google.com)
- [Facebook](http://facebook.com)
- [Twitter](http://twitter.com)


---

# SPA Frameworks/Libraries
- Backbone.js
- Ember
- Meteor
- AngularJS
- Aurelia
- React
- Vue.js
- Angular
- And so many more...

---

# AngularJS
- Published 2010
- MVC (Model-View-Controller) framework with dependency injection
- Revolutionary on its own time
- Two-way data binding
- Emphasis on testability (decouples DOM manipulation from app logic)

---

# Angular
- Built by around 20 Google developers & lots of open source devs
- Built with TypeScript (ES2015 and Dart versions available)
- Complete rewrite of AngularJS
- Not just another web framework, complete platform
- Also for desktop and mobile development
- [Documentation](https://angular.io/docs/ts/latest/api/)

---

# Angular Release
- Major version every 6 months (April and October)
- Two version deprecation policy
- Even numbers (4, 6, ..) offer LTS (Long-Term Support)
- Releases:
    - 2.0.0-beta.0 1/2016
    - 2.0.0-rc.0 5/2016
    - 2.0.0 9/2016 
    - 4.0.0 3/2017
    - 5.0.0 11/2017
    - 6.0.0 4/2018
- More info in [Angular GitHub](https://github.com/angular/angular/blob/master/docs/RELEASE_SCHEDULE.md)

---

# Tooling

- Traditionally web pages have been just static HTML, CSS and maybe some simple JS for dropdowns etc.
- Nowadays massive SPAs require something more advanced and thus the need for tooling
- Some basic needs for tooling:
  - Compiling ES2015/TypeScript -> ES5 and LESS/SASS -> CSS
  - Combining multiple source files into single bundle file for faster loading
  - Running test suites
  - Optimizations (minification, Dead code elimination, tree shaking)

---

# JSON
- JavaScript Object Notation (JSON) is a lightweight data-interchange format
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

# Node.js & npm

- Node.js is JavaScript interpreter built on top of Chrome's V8 JavaScript engine
- Used for running development server, to run tests, to build production-optimized bundle, etc.
- npm (node package manager) is the package manager for Node
  - More packages than on any other package manager for any other language: over 270k (May 2016) ([modulecounts.com](http://www.modulecounts.com/))

---

# package.json - project configuration
- Declares dependencies, development-time dependencies and commands available
- Can be generated with `npm init`

```json
{
  "name": "Angular basic training",
  "author": "Gofore",
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

# Angular CLI

- Command-line interface for Angular development
- Recommended by the core team
- One of the core modules: `@angular/cli`
- Follows Angular core versioning as of _6.0.0_
- Abstracts away the bundling
- Uses Webpack internally

---

# Angular CLI Features
- Generate the project initially
- Run dev server
- Generate modules, components, services, tests, directives
- Generate production build
- Tests
- Update your dependencies
- Supports CSS preprocessors (SASS and LESS)

---

# Angular CLI Usage
- Generating an app
```shell
ng new PROJECT_NAME
```

- Run development server
```shell
ng serve # Available in localhost:4200
 ```

- Run tests
```shell
ng test
```

- Generate a component
```shell
ng generate component todos
```

---

# ES2015 (ES6)
- EcmaScript (ES) is the standard for JavaScript
- One of the newer EcmaScript standards
- Published 2015
- Also known as ES6 because last version was ES5
- Provides a lot of improvements for writing JavaScript in scale
- Not supported by older browsers (namely IE11)

---

# ES2015 - Key Features
- `let` and `const` to replace `var`
- Arrow functions
- Multiline strings
- Modules
- Enhancements on basic types such as `includes()` for string and `find()` for array

---

# Let and const
- `const` declares constant **reference** (not constant value)
- `let` declares variable (eg. `let myVar = 'asd';`)
- **Rule of thumb: Always use `const` if possible, `let` otherwise.**

```javascript
const input = [0, 1, 2, 3, 4];
input = []; // Uncaught TypeError: Assignment to constant variable.
input.push(5); // Works, as input is just the reference
```

For immutable objects & arrays there are libraries such as [_Immutable.js_](https://facebook.github.io/immutable-js/).

---

# Arrow functions
- Lexical `this` and more concise syntax

```javascript
// Traditional function
const increase = function (value) {
  return value + 1;
};

// Arrow function
const increase = (value) => {
  return value + 1;
};

// Parenthesis omitted
const increase = value => {
  return value + 1;
};

// Return value without curly braces
const increase = value => value + 1;
```

---

# String Literals
ES5 string:
```javascript
const str = firstName + ' ' + secondName.charAt(0) + '. ' + lastName; 
```

ES2015 multiline string with backticks:
```typescript
const str = ´${firstName} ${secondName.charAt(0)}. ${lastName}`;
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
import { square, squareSum } from './lib';
console.log(square(11)); // 121
console.log(squareSum(4, 3)); // 5
```

- Importing from npm modules

```javascript
import { Component, Input } from '@angular/core';
```

---

# TypeScript
- General-purpose programming language
- Built by Microsoft to build JavaScript in scale
- Initial release in October 2012
- Typed superset of JavaScript -> Any valid JS is valid TypeScript
- Advantages:
  - Static type system on top of JavaScript to catch errors already on compile-time
  - Angular & RxJS are written in TypeScript

---

# Typing
- Provides the same types as in JavaScript: `number`, `string`, `boolean`, `null`, `undefined` and `object`
- Arrays like `number[]`, `string[]` and `any[]`
- Also some "extra" types such as `any`, `void` and `enum`
- `any` is basically anything like `number`, `string` or `any[]`
- Types are marked after the name
- Examples:

```typescript
let luckyNumber: number = 50;
luckyNumber = 'nine'; // TypeError: Can't assign string to number

function increase(value: number): number {
    return value + 1;
}
```

---

# Interfaces
- Interfaces to declare the acceptable object structures
- Can have optional properties (declared with `?` before `:`)
- _Structural typing_ instead of _Nominal typing_ 

```typescript
interface Person {
    firstName: string;
    middleName?: string;
    lastName: string;
}

const greeter = (person: Person): string => "Hello, " + person.firstName + " " + person.lastName;

greeter({ firstName: "John", lastName: "Doe" }); // Hello, John Doe
```

---

# Classes
- Like in many other programming languages
- Member fields can be declared in constructor with visibility modifier (`private`, `protected`, `public`)

```typescript
class Student {
    fullName: string;
    constructor(private firstName: string, private lastName: string) {
        this.fullName = this.firstName + " " + this.lastName;
    }
    
    getFirstName() {
        return this.firstName;
    }
}

const student = new Student("John", "Doe");
console.log(student.getFirstName());
```

---

# Annotations (Decorators)
- Used to "decorate" classes and properties with additional functionality
- Like Java annotations
- Apply to next entity (class, field, method) after them

```typescript
@Component({
  template: 'my template'
})
class MyClass {
  @Input() myProperty: string;
}
```
