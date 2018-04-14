# Angular Advanced Topics
- Router
- Pipes
- Forms
- Directives

---

# Router
- Core responsibilities:
  - Map URL into app state
  - Provide transitions from one URL to another (state to another)
- Supports lazy loading of certain paths

---

# Router - Basics
Route declarations

_app.module.ts_
```typescript
import { NgModule } from '@angular/core';
*import { RouterModule } from '@angular/router';

*const routeConfig = [
*  {
*    path: 'todos',
*    component: TodosComponent
*  }
*];

@NgModule({
  declarations: [
    AppComponent, TodosComponent, TodoItemComponent
  ],
  imports: [
    BrowserModule,
*   RouterModule.forRoot(routeConfig),
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

---

# Router - Basics
`<router-outlet></router-outlet>` declares the placeholder for routed component tree

_app.component.html_
```html
<h1>App works!</h1>
<router-outlet></router-outlet>
```

---

# Router - More advanced routes

_app.module.ts_
```typescript
const routeConfig = [
  {
    path: 'todos',
*   children: [
*     {
*       path: '',
*       component: TodosComponent
*     },
*     {
*       path: ':index',
*       component: EditTodoItemComponent
*     }
*   ]
  }
];
```

declares routes `todos/` and `todos/:id`. `:id` is named placeholder for path parameter that can be accessed within the component.

---

# Router - Redirects
- By default matching of URLs is done with _startsWith_ algorithm
- `pathMatch: 'full'` can be used to set the algorithm to full matching
- Redirects can be done with `redirectTo`

_app.module.ts_
```typescript
const routeConfig = [
  {
*   path: ''
*   pathMatch: 'full',
*   redirectTo: 'todos/'
  },
  {
    path: 'todos',
    component: TodosComponent
  }
];
```
---

# Router - Accessing Route Parameters
- Route parameters can be accessed via `ActivedRoutes` instances `params` value as observable.
- Every time the parameters change, new event will be sent.
- Makes it possible not to load the component again and again from scratch

```typescript
@Component({})
export class EditTodoItemComponent {
  constructor(route: ActivatedRoute) {
    route.params.subscribe((params) => {
      this.index = +params['index']; // + converts string to number in JavaScript
    });
  }
```

---

# Router - Fragments & Query Parameters
- Fragments (`example.com#key=value`) and query parameters (`example.com?key=value`) are shared by all routes
- Can be accessed just like path parameters:

```typescript
@Component({})
export class MyComponent {
  constructor(route: ActivatedRoute) {
    route.queryParams.subscribe((params) => { // or .fragment
      this.key = +params['key'];
    });
  }
```

---

# Router - Navigating
- Two ways to navigate between states:
 - Imperatively from within the components code: `this.router.navigateByUrl('todos/1')` or `this.router.navigate(['todos', 1])`
 - Declaratively from within the template: `<div [routerLink]=['todos', 1]>`
- URLs starting with `/` are absolute navigations, others relative
- `../` also works for accessing the "parent" URL

_app.component.ts_
```typescript
export class AppComponent {
  constructor(`private router: Router`) {
*   this.router.navigate(['todos']);
  }
}
```

_app.component.html_
```html
<a [routerLink]="['todos', 3]">
  Todo 3
</a>
```

---

# Router - Guards
- Multiple guards available for each route:
  - `CanActivate` to mediate navigation to a route
  - `CanActivateChild` to mediate navigation to a child route
  - `CanDeactivate` to mediate navigation away from the current route
  - `Resolve` to perform route data retrieval before route activation
  - `CanLoad` to mediate navigation to a feature module loaded asynchronously
- The `Can*` guards can return either boolean or promise (resolving to a boolean value) to allow or prevent navigation

---
# Router - Guards
- Usage:

_app.module.ts_
```typescript
const routeConfig = [
  {
    path: 'todos',
*   canActivate: [AuthGuard],
    component: TodosComponent
  }
];
```

_auth-guard.ts_
```typescript
import { CanActivate, Router,
  ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';

@Injectable()
export class AuthGuard implements CanActivate {
  constructor(private router: Router, private authService: AuthService) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
    return this.authService.checkLogin(state.url);
  }
```

---

# Router - URL Strategies
- Two strategies for URL formation:
  - `PathLocationStrategy`: HTML5 _pushState_ style (`example.com/todos/1`) (default)
  - `HashLocationStrategy`: Hash URLs (`example.com/#/todos/1`)
- Setting the strategy:

```typescript
@NgModule({
  imports: [
    BrowserModule,
    RouterModule.forRoot(routeConfig, `{ useHash: true }`)
  ],
  declarations: [ AppComponent ],
  bootstrap: [ AppComponent ]
})
export class AppModule {}
```

---

# Pipes
- Simple display-value transformations
- Similar concept as _filters_ in AngularJS
- Angular provides few common ones, e.g.: `UpperCasePipe`, `LowerCasePipe` and `DatePipe`

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
- Must be declared in `NgModule` declaration to be available in templates

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
- Angular provides common form functionalities:
  - Two-way data binding
  - Change tracking
  - Validation
  - Error handling
- Angular forms can be either model- or template-driven
  - Only template-driven forms are covered here, as they are enough for most use cases
- To enable forms, we need to add `FormsModule` to imports of our `NgModule`:

```typescript
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [
    BrowserModule,
*   FormsModule
  ]
})
export class AppModule { }
```

---

# Forms - Template-driven Forms
- Forms declared in templates rather than in component
- Each input inside form element is attached by default
- Each input needs to have (unique) name attribute set

```html
<form>
  <input type="text" name="name" [(ngModel)]="name" />
  <input type="email" name="email" [(ngModel)]="email" />
<form>
```

---

# Forms - Using Template-local Variables
Forms export `FormControl` as `ngModel` for each input to be bound to template-local variable

```html
<form>
  <input type="text" [(ngModel)]="name" `name="name"` `#nameModel="ngModel"` required />
  <input type="email" [(ngModel)]="email" `name="email"` `#emailModel="ngModel"` required />
  <span [hidden]="`nameModel.valid && emailModel.valid`">
    We have an invalid input.
  </span>
<form>
```
(`#nameModel` and `#emailModel` are called template-local variables)

---

# Forms - CSS Classes
- CSS classes are attached automatically by framework

![Control CSS Classes](angular-advanced-topics/control-css-classes.png "Control CSS Classes")

```css
.ng-invalid[required] {
  border: 1px solid red;
}
```

---

# Forms - NgForm
- Forms exports `ngForm` which can be bound into template-local variable
- Contains combined information about all the input controls inside the form

```html
<form `#myForm="ngForm" (submit)="submitForm(myForm)"`>
  <input type="text" [(ngModel)]="name" name="name" #name="ngModel" />
  <input type="email" [(ngModel)]="email" name="email" #email="ngModel" />

  <button type="submit" `[disabled]="myForm.invalid"`>Submit</button>
<form>
```

---

# Forms - Accessing Form Inside Component
- Template-driven forms can be accessed from component with `@ViewChild('myForm')` annotation:

```typescript
import { ViewChild } from '@angular/core';
import { FormGroup } from '@angular/forms';

@Component(..)
export class MyComponent {
  @ViewChild('myForm')
  private myForm: FormGroup;
}
```

---

# Directives
- There are three kinds of directives in Angular:
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
