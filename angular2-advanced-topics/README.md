# Angular 2 Advanced Topics

---
# Router

---
# Pipes
- Pipes are simple display-value transformations that can be applied inside a template
- There are quite multiple pipes already implemented within Angular 2 itself such as `UpperCasePipe`, `LowerCasePipe` and `DatePipe`
- Pipes can be applied with the pipe character (`|`):
```html
<div>{{user.name | upperCase}}</div> <!-- JOHN DOE -->
```
- Pipes can have also arguments which are provided to it with the following syntax
```html
<div>{{user.birthDay | date:'fullDate'}}</div> <!-- Friday, April 15, 1988 -->
```
- It is also possible to pipe the pipes (like UNIX commands)
```html
<div>{{user.birthDay | date:'fullDate' | uppercase}}</div> <!-- FRIDAY, APRIL 15, 1988 -->
```
---
# Custom pipes
- Custom pipes are declared with `@Pipe` annotation
- They implement the `PipeTransform` interface by implementing method `transform` which takes the value as first argument and the optional arguments as rest of parameters
```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({name: 'exponential'})
export class ExponentialPipe implements PipeTransform {
  transform(value: number, exponent: string): number {
    let exp = parseFloat(exponent);
    return Math.pow(value, isNaN(exp) ? 1 : exp);
  }
}
```
- This pipe could now used as follows:
```html
<div>{{10 | exponential:3}}</div> <!-- 1000 -->
```
---
# Forms

---
# Dependency Injection
- Dependency Injection (DI) is a common pattern in modern frameworks to provide component with its dependencies
- Using DI the component can just rely on the dependencies to be provided for it
- This leads to components to be easier to test, more solid and flexibility

---
# Dependency injection in Angular 2
- The root dependency injector is created for root component (`AppComponent` in this case) with `bootstrap` method

```TypeScript
bootstrap(AppComponent);
```
- Bootstrap also accepts another parameter, which is an array of providers that would be registered:

```TypeScript
bootstrap(AppComponent, [UserService, BackendService]);
```
The providers would be now injectable to anywhere and also singleton within the whole application.
This is discouraged as the providers should be registered only where needed with `providers` metadata like

```typescript
@Component({
  selector: 'users',
  template: ``,
  providers:[UserService]
})
export class UsersComponent { }
```
This way the `UserService` would now be injectable to any child component of this component and not in whole application scope

---
# Directives
- There are three kinds of directives in Angular 2:
  - Components, which are basically selectors with templates and inputs and outputs
  - Structural directives, `ngFor` and `ngIf` are examples of these
  - Attribute directives
- Here we talk about attribute directives which can be used to decorate the elements

---
# Example
-  A directive for setting background color to yellow

```typescript
import { Directive, ElementRef, Input } from '@angular/core';
@Directive({
    selector: '[myHighlight]'
})
export class HighlightDirective {
    constructor(el: ElementRef) {
       el.nativeElement.style.backgroundColor = 'yellow';
    }
}
```

```html
<span myHighlight></span>
```

---
# Events for directives
- We can use `host` property to define events with their corresponding handlers

```typescript
import { Directive, ElementRef, Input } from '@angular/core';
@Directive({
    selector: '[myHighlight]',
    host: {
      '(mouseenter)': 'onMouseEnter()',
      '(mouseleave)': 'onMouseLeave()'
    }
})
export class HighlightDirective {
    private el:HTMLElement;
    constructor(el: ElementRef) { this.el = el.nativeElement; }
    onMouseEnter() { this.highlight("yellow"); }
    onMouseLeave() { this.highlight(null); }
    private highlight(color: string) {
      this.el.style.backgroundColor = color;
    }
}
```
---
# Bind value from host component
- We can bind host components property to our directive by declaring an `@Input` annotation:

```typescript
@Input('myHighlight') highlightColor: string;
```
- Now we can pass the property like

```html
<span [myHighlight]="color"></span>
```
