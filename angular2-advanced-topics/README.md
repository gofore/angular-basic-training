# Angular 2 Advanced Topics
- Router
- Pipes
- Directives
- Forms
- Dependency Injection
- Change detection

---

# Router - Basics
Maps URLs to components

_app.component.ts_
```typescript
import {Component} from '@angular/core';
import {ROUTER_DIRECTIVES} from '@angular/router';
import {TodoComponent} from './todo.component';
import {TodosComponent} from './todos.component';

@Component({
*  directives: [ROUTER_DIRECTIVES]
})
*@Routes([
*  {path: '/', component: TodosComponent},
*  {path: '/todos/:id', component: TodoComponent}
*])
export class AppComponent {
  constructor(private router: Router) {
*   this.router.navigate(['/']);
  }
}
```

(see next slide)
---

# Router - Basics (continued)

_app.component.html_
```html
<router-outlet></router-outlet>
```

_index.ts_
```typescript
import {bootstrap} from '@angular/platform-browser-dynamic';
import {ROUTER_DIRECTIVES} from '@angular/router';
import {AppComponent} from './app.component';

bootstrap(AppComponent, [ROUTER_PROVIDERS]);
```

---

# Router - Navigating
Navigating to routes:
- In component with `router.navigate([url])`
- In template with `a` with directive `[routerLink]=[url]`

_app.component.ts_
```typescript
import {Component} from '@angular/core';
import {Routes, Router} from '@angular/router';
import {TodosComponent} from 'todos.component';
import {TodoComponent} from 'todo.component';

@Component({ ... })
@Routes([
  {path: '/', component: TodosComponent},
  {path: '/todos/:id', component: TodoComponent}
])
export class AppComponent {
  constructor(`private router: Router`) {
*   this.router.navigate(['/todos/3']);
  }
}
```
(see next slide)

---

# Router - Navigating (continued)
_app.component.html_
```html
<a [routerLink]="['/todos/3']">
  Todo
</a>
```

---

# Router - Lifecycle Hooks
- Supplement component lifecycle hooks
- E.g.: `CanActivate`, `OnActivate` and `CanDeactivate`

```typescript
import {OnActivate} from '@angular/router';

export class TodosComponent `implements OnActivate` {
*   onActivate() {
*     // Route activated
*   }
  }
}
```
---

# Router - `CanDeactivate` Lifecycle Hook
- Not yet implemented!
- E.g. "Unsaved data will be lost" confirmations:
- Can either return boolean _false_ or observable emitting _false_ to prevent navigating out

```typescript
import { CanDeactivate } from '@angular/router';

export class TodosComponent `implements CanDeactivate` {
*  routerCanDeactivate(): any {
*    if (...) return true;
*    return this.dialog.confirm('Discard changes?');
*  }
}
```

---

# Router - URL Parameters
- `onActivate` will be called on activation with `routeSegment` as param
- `routeSegment.getParam` can be used to access params

_todos.component.ts_
```typescript
@Component({...})
*@Routes([
*  {path: '/todos/:id', component: TodoComponent}
*])
class TodosComponent { }
```

_todo.component.ts_
```typescript
@Component({...})
class TodoComponent implements OnActivate {
  `id: number;`
  onActivate(`routeSegment: RouteSegment`) {
    `this.id = routeSegment.getParam('id');`
  }
}
```


```typescript
this.router.navigate(['/todos/1']);
```

---

# Router - Query Parameters
- Used for optional and complex parameters

_todos.component.ts_
```typescript
@Component({...})
*@Routes([
*  {path: '/todos/:id', component: TodoComponent}
*])
class TodosComponent { }
```

_todo.component.ts_
```typescript
@Component({...})
class TodoComponent implements OnActivate {
  `query: number;`
  onActivate(`routeSegment: RouteSegment`) {
    `this.query = routeSegment.getParam('query');`
  }
}
```


```typescript
this.router.navigate(['/todos/1', {query: 'foo'}]); // URL: '/todos/1?query=foo'
```

---

# Router - URL Strategies
- Two strategies for URL formation:
  - `PathLocationStrategy`: HTML5 _pushState_ style (`example.com/todos/1`)
  - `HashLocationStrategy`: Hash URLs (`example.com/#/todos/1`)
- Setting the strategy:

```typescript
import {provide} from '@angular/core';
import {ROUTER_PROVIDERS, LocationStrategy, HashLocationStrategy} from '@angular/common';

bootstrap(AppComponent, [
  ROUTER_PROVIDERS,
  `provide(LocationStrategy, {useClass: HashLocationStrategy})`
]);
```

---

# Routers - Nested Routers
- Child components can define their own routes:

_app.component.ts_
```typescript
@Component({...})
*@Routes([
*  {path: '/todo-lists', component: TodoListsComponent}
*])
class AppComponent { }
```

```typescript
@Component({...})
*@Routes([
*  {path: '/', component: TodosComponent},
*  {path: '/todos/:id', component: TodoComponent},
*])
class TodoListsComponent { }
```

---

# Pipes
- Simple display-value transformations
- Similar concept as _filters_ in Angular 1
- Angular 2 provides few common ones, e.g.: `UpperCasePipe`, `LowerCasePipe` and `DatePipe`

_my.component.html_
```html
<div>{{user.name` | uppercase`}}</div> <!-- JOHN DOE -->
```

_my.component.ts_
```typescript
import {UpperCasePipe} from '@angular/common';

@Component({
* pipes: [UpperCasePipe]
})
class MyComponent {}
```

---

# Pipe arguments

```html
<div>{{user.birthDay | date`:'fullDate'`}}</div> <!-- Friday, April 15, 1988 -->
```

---

# Multiple pipes
- Pipes can be piped (like in UNIX)

```html
<div>{{user.birthDay` | date:'fullDate' | uppercase`}}</div> <!-- FRIDAY, APRIL 15, 1988 -->
```

---

# Custom Pipes
- Declared with `@Pipe` annotation
- `PipeTransform` interface with `transform` method
- `transform` takes the value as first argument and the optional arguments as rest of parameters

```typescript
import {Pipe, PipeTransform} from '@angular/core';

*@Pipe({name: 'exponential'})
*export class ExponentialPipe implements PipeTransform {
*  transform(value: number, exponent: string): number {
    let exp = parseFloat(exponent);
    return Math.pow(value, isNaN(exp) ? 1 : exp);
  }
}
```

```html
<div>{{10 | exponential:3}}</div> <!-- 1000 -->
```

---

# Forms
- Angular 2 provides common form functionalities:
  - Two-way data binding
  - Change tracking
  - Validation
  - Error handling

---

# Forms - Template-driven Forms
Forms declared in templates rather than in component

```html
<form>
  <input type="text" [(ngModel)]="name" />
  <input type="email" [(ngModel)]="email" />
<form>
```

---

# Forms - NgControl
`ngControl` directive adds control that tracks validity, dirtiness and visiting

```html
<form>
  <input type="text" [(ngModel)]="name" `ngControl="name"` `#name="ngForm"` />
  <input type="email" [(ngModel)]="email" `ngControl="email"` `#email="ngForm"` />
  <span [hidden]="`name.valid && email.valid`">
    We have an invalid input.
  </span>
<form>
```
(`#name` and `#email` are called template-local variables)

---

# Forms - NgControl & CSS Classes
![Control CSS Classes](angular2-advanced-topics/control-css-classes.png "Control CSS Classes")

```css
.ng-invalid[required] {
  border: 1px solid red;
}
```

---

# Forms - NgForm
Form automatically wraps the controls inside it

```html
<form #myForm="ngForm">
  <input type="text" [(ngModel)]="name" ngControl="name" #name="ngForm" />
  <input type="email" [(ngModel)]="email" ngControl="email" #email="ngForm" />

  <button (click)="submitForm(myForm)" `[disabled]="myForm.invalid"`>Submit</button>
<form>
```

---

# Dependency Injection
- Dependency Injection (DI) is a common pattern in modern frameworks to provide component with its dependencies
- Using DI the component can just rely on the dependencies to be provided for it
- Makes components easier to test, more solid and flexible

---

# Dependency Injection in Angular 2
The root dependency injector is created for root component (`AppComponent` in this case) with `bootstrap` method

```TypeScript
bootstrap(AppComponent);
```
`bootstrap` also accepts another parameter, which is an array of providers that would be registered:

```TypeScript
bootstrap(AppComponent, `[UserService, BackendService]`);
```
The providers would be now injectable to anywhere and also singleton within the whole application.
This is discouraged as the providers should be registered only where needed with `providers` metadata like

```typescript
@Component({
  selector: 'users',
* providers: [UserService]
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

# Directives - Example
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

# Directives - Events
- We can use `host` property to define events with their corresponding handlers

```typescript
import { Directive, ElementRef, Input } from '@angular/core';
@Directive({
    selector: '[myHighlight]',
*   host: {
*     '(mouseenter)': 'onMouseEnter()',
*     '(mouseleave)': 'onMouseLeave()'
*   }
})
export class HighlightDirective {
    private el:HTMLElement;
    constructor(el: ElementRef) { this.el = el.nativeElement; }
*   onMouseEnter() { this.highlight("yellow"); }
*   onMouseLeave() { this.highlight(null); }
    private highlight(color: string) {
      this.el.style.backgroundColor = color;
    }
}
```

---

# Directives - Bind Value from Host Component
- We can bind host components property to our directive by declaring an `@Input` annotation:

```typescript
@Input('myHighlight') highlightColor: string;
```
- Now we can pass the property like

```html
<span `[myHighlight]="color"`></span>
```

---

# Zone.js
- Implements concept of zones (inspired by Dart) in JavaScript
- "A Zone is an execution context that persists across async tasks"
- In practice makes it possible to track the asyncronous calls made (HTTP, timers and event listeners) within the zone
- Angular 2 uses zones internally to track changes in state

---

# Change Detection - General
- Idea: project the internal state into UI (DOM in web)
- The internal state consists of JavaScript primitives such as objects, arrays, strings etc.
- Change only possible on asynchronous events such as timeouts or HTTP request

---

# Change Detection - Angular 2
- Zone.js makes it possible to track all possible change sources
- Components change detector checks the bindings (like `{{name}}` and `(click)`) defined in its template on change
- Bindings are propagated from the root to leaves in the depth first order
- Change detection graph is directed tree and can't contain cycles -> improved performance, predictability and debugging

---

![Change detection](angular2-advanced-topics/change-detection.png "Change detection")

---
# Change Detection - Simplified Implementation
```typescript
// very simplified version of actual source
class ApplicationRef {
    changeDetectorRefs: ChangeDetectorRef[] = [];

    constructor(private zone: NgZone) {
      this.zone.onTurnDone
        .subscribe(() => this.zone.run(() => this.tick());
    }

    tick() {
      this.changeDetectorRefs
        .forEach((ref) => ref.detectChanges());
    }
}
```
---

# Change Detection - Strategies
- Change detection strategy describes which strategy will be used the next time change detection is triggered
- Angular 2 has six change detection strategies:
  - **CheckOnce**: After calling _detectChanges_ the mode of the change detector will become _Checked_.
  - **Checked**: Change detector should be skipped until its mode changes to _CheckOnce_.
  - **CheckAlways**: After calling _detectChanges_ the mode of the change detector will remain _CheckAlways_.
  - **Detached**: Change detector sub tree is not a part of the main tree and should be skipped.
  - **OnPush**: Change detector's mode will be set to _CheckOnce_ during hydration.
  - **Default**: Change detector's mode will be set to _CheckAlways_ during hydration.
- Setting the strategy:
```typescript
@Component({`changeDetection: ChangeDetectionStrategy.OnPush`})
```

---

# Change Detection - Angular 2 Performance
- Change detection is one of the key functionalities of Angular 2 and thus it is highly optimized
- CDs get VM optimized monomorphic classes generated for them at runtime
- Change detection is always single-pass (stable) because of uni-directional top-to-bottom flow
- More optimizations possible with _immutables_ and _observables_
