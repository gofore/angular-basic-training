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
@Routes([
 {path: '/crisis-center', component: CrisisListComponent},
 {path: '/heroes',        component: HeroListComponent},
 {path: '/hero/:id',      component: HeroDetailComponent}
])
export class AppComponent implements OnInit {
 constructor(private router: Router) {}

 ngOnInit() {
   this.router.navigate(['/crisis-center']);
 }
}
```

```html
<router-outlet></router-outlet>
```
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
# Custom Pipes
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
# Dependency Injection in Angular 2
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
# Events for Directives
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
# Bind Value from Host Component
- We can bind host components property to our directive by declaring an `@Input` annotation:

```typescript
@Input('myHighlight') highlightColor: string;
```
- Now we can pass the property like

```html
<span [myHighlight]="color"></span>
```

---

# Zone.js
- Zone.js implements concept of zones (inspired by Dart) in JavaScript
- "A Zone is an execution context that persists across async tasks"
- In practice makes it possible to track the asyncronous calls made (HTTP, timers and event listeners) within the zone
- Angular 2 uses zones internally to track changes in state

---
# Change detection in general
- Task of change detection is to project the internal state into UI (which is the DOM in case of web)
- The internal state can consist of any JavaScript primitives such as objects, arrays, strings etc.
- Change can only happen when something asynchronous has happened (event, timeout or HTTP request triggered)

---

# Change detection in Angular 2
- Zone.js makes it possible to track all the possible change sources
- Every component gets a change detector responsible for checking the bindings (like `{{name}}` and `(click)`) defined in its template
- Change detectors propagate bindings from the root to leaves in the depth first order
- Change detection graph is directed tree and can't contain cycles which leads makes it more performant and predictable and easier to debug

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
- Setting the strategy is done with `changeDetection` property of `@Component` annotation:
```typescript
@Component({`changeDetection: ChangeDetectionStrategy.OnPush`})
```

---

# Angular 2 change detection performance
- Change detection is one of the key functionalities of Angular 2 and thus it is highly optimized
- Each change detector has monomorphic class generated at runtime to be VM optimized
- Change detection is always single-pass (stable) because of uni-directional top-to-bottom flow
- Even more optimizations are possible with immutables and observables
