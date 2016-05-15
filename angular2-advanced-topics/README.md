# Angular 2 Advanced Topics
- Router
- Pipes
- Directives
- Forms
- Dependency Injection
- Change detection

---

# Router

**TBD**

```typescript
@Component({ ... })
*@Routes([
*  {path: '/crisis-center', component: CrisisListComponent, name: 'CrisisCenter'},
*  {path: '/heroes',        component: HeroListComponent,   name: 'Heroes'},
*  {path: '/hero/:id',      component: HeroDetailComponent, name: 'Hero'}
*])
export class AppComponent implements OnInit {
  constructor(private router: Router) {}

  ngOnInit() {
*   this.router.navigate(['/crisis-center']);
  }
}
```

```html
<router-outlet></router-outlet>
```

```html
<a [routerLink]="CrisisCenter">
  Crisis Center
</a>
```

---

# Exercise 2.1

---

# Pipes
- Simple display-value transformations
- Similar concept as _filters_ in Angular 1
- Angular 2 provides few common ones, e.g.: `UpperCasePipe`, `LowerCasePipe` and `DatePipe`

_my.component.html_
```html
<div>{{user.name` | upperCase`}}</div> <!-- JOHN DOE -->
```

_my.component.ts_
```typescript
import {UpperCasePipe} from '@angular/common';

@Component({
  pipes: [UpperCasePipe]
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

# Exercise 2.2

---

# Forms

---

# Exercise 2.3

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

# Events for Directives
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

# Bind Value from Host Component
- We can bind host components property to our directive by declaring an `@Input` annotation:

```typescript
@Input('myHighlight') highlightColor: string;
```
- Now we can pass the property like

```html
<span `[myHighlight]="color"`></span>
```

---

# Exercise 2.4

---

# Zone.js
- implements concept of zones (inspired by Dart) in JavaScript
- "A Zone is an execution context that persists across async tasks"
- In practice makes it possible to track the asyncronous calls made (HTTP, timers and event listeners) within the zone
- Angular 2 uses zones internally to track changes in state

---

# Change detection in general
- Idea: project the internal state into UI (DOM in web)
- The internal state consists of JavaScript primitives such as objects, arrays, strings etc.
- Change only possible on asynchronous events such as timeouts or HTTP request

---

# Change detection in Angular 2
- Zone.js makes it possible to track all possible change sources
- Components change detector checks the bindings (like `{{name}}` and `(click)`) defined in its template on change
- Bindings are propagated from the root to leaves in the depth first order
- Change detection graph is directed tree and can't contain cycles -> improved performance, predictability and debugging

---

![Change detection](angular2-advanced-topics/change-detection.png "Change detection")

---
# Simplified implementation
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

# Change detection strategies
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

# Angular 2 change detection performance
- Change detection is one of the key functionalities of Angular 2 and thus it is highly optimized
- CDs get VM optimized monomorphic classes generated for them at runtime
- Change detection is always single-pass (stable) because of uni-directional top-to-bottom flow
- More optimizations possible with _immutables_ and _observables_
