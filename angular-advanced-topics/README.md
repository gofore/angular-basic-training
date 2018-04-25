# Angular Advanced Topics
- Router
- Forms
- Pipes
- Directives

---

# Router
- Core responsibilities:
  - Map URL into app state
  - Provide transitions from one URL to another (state to another)
- Supports lazy loading of certain paths

---

# Router Basics
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

# Router Basics
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

declares routes `todos/` and `todos/:index`. `:index` is named placeholder for path parameter that can be accessed within the component.

---

# Redirects
- By default matching of URLs is done with _startsWith_ algorithm (and everything starts with path `'''`)
- `pathMatch: 'full'` can be used to set the algorithm to full matching
- Redirects can be done by using `redirectTo` instead of specifying `component`

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

# Accessing Route Parameters
- Route parameters can be accessed via `params` field of `ActivatedRoute` as observable
- Once parameters change new event will be sent.
- Makes it possible to reuse the same component instead of instantiating a new one

```typescript
@Component({})
export class EditTodoItemComponent {
  constructor(route: ActivatedRoute) {
    this.route.params.subscribe(params => {
      this.index = params.index; 
    });
  }
```

---

# Fragments & Query Parameters
- Fragments (`example.com#key=value`) and query parameters (`example.com?key=value`) are shared by all routes
- Can be accessed like path parameters:

```typescript
@Component({})
export class MyComponent {
  constructor(route: ActivatedRoute) {
    this.route.queryParams.subscribe(params => { // or .fragment
      this.key = params.key;
    });
  }
```

---

# Router - Navigating
Two ways to navigate between states:
- Imperatively inside a component: 

```typescript
this.router.navigateByUrl('todos/1')
```

- Declaratively inside a template:
 
```typescript
<a [routerLink]="todos/1"></a>
```

---

# Navigation syntax
- Parts of paths can be composed easily with the array syntax
- Array syntax concatenates the parts with `/`
- So the below are the same:

```typescript
this.router.navigateByUrl('todos/1/tasks');
this.router.navigate(['todos', 1, 'tasks']);
```

```html
<a [routerLink]="'todos/1/tasks'"></a>
<a [routerLink]="['todos', 1, 'tasks']"></a>
```

---

# Absolute vs. Relative Navigation

- If the path starts with a slash (`/`), it is an absolute navigation.
- Example:
```typescript
// Assuming we are now in example.com/todos/1
this.router.navigateByUrl('tasks'); // example.com/todos/1/tasks
this.router.navigateByUrl('/tasks'); // example.com/tasks
```
- `../` can be used to go one level up
```typescript
// Assuming we are now in example.com/todos/1/tasks
this.router.navigateByUrl('../'); // example.com/todos/1
```

---

# Router Guards
- Guards allow changing the behaviour of routing for certain routes
- Guards can be registered for navigation into and out of route
- Guards can cancel the routing and instead redirect user to somewhere else
- Example use cases:
 - Authentication
 - Preloading data for component
 - Logging
 - "You have unsaved changes. Are you sure you want to leave?"

---

# Guard Types
- Multiple guards available for each route:
  - `CanActivate` to mediate navigation to a route
  - `CanActivateChild` to mediate navigation to a child route
  - `CanDeactivate` to mediate navigation away from the current route
  - `Resolve` to perform route data retrieval before route activation
  - `CanLoad` to mediate navigation to a feature module loaded asynchronously
- The `Can*` guards can return either boolean or promise (resolving to a boolean value) to allow or prevent navigation

---
# Guard Example
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

# Router URL Strategies
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

# Forms
- Angular provides highly advanced form management functionality:
  - Two-way data binding
  - Change tracking
  - Validation
  - Error handling

---

# Template-driven vs. Reactive Forms
- Angular forms can be either template-driven or reactive
- Both have their own `NgModule`: `FormsModule` and `ReactiveFormsModule`
- Both modules are in `@angular/forms` npm package
- The underlying principles are the same but the usage is different
- Both can be used within a single application, even on single component
- Only template-driven forms are covered in this training, as they are enough for most use cases

---

# Reactive Forms
- As the name suggests, based on reactive programming (covered tomorrow)
- Form controls defined in component instead of template
- Require more boilerplate code
- For complex use cases 
- More easy to test

---

# Reactive Forms Example

```typescript
import { Component } from '@angular/core';
import { FormGroup, FormBuilder, Validators } from '@angular/forms';

export class TodosComponent {
    todosForm: FormGroup;
    
    constructor(private fb: FormBuilder) {
        this.todosForm = this.fb.group({
          name: ['', Validators.required],
          done: [false],
        });
    }
}
```

```html
<form [formGroup]="todosForm">
    <input class="form-control" formControlName="name">
    <input type="checkbox" formControlName="done">
</form>
```

---

# Template-driven Forms
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

# Using Template-local Variables
Forms export `FormControl` as `ngModel` for each input to be bound to template-local variable

```html
<form>
  <input [(ngModel)]="name" `name="name"` `#nameModel="ngModel"` required />
  <input [(ngModel)]="email" `name="email"` `#emailModel="ngModel"` required />
  <div *ngIf="`nameModel.invalid || emailModel.invalid`">
    Either name or email is invalid
  </div>
<form>
```

---

# CSS Classes
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

# Pipes
- Simple display-value transformations
- Similar concept as _filters_ in AngularJS
- Angular provides few commonly needed pipes, e.g.: `uppercase`, `lowecase` and `date`

```html
<!-- user.name is initially "john doe" -->
<div>{{user.name` | uppercase`}}</div> <!-- JOHN DOE -->
```

---

# Pipe arguments
- Pipes can take arguments that modify its behavior:
```html
<div>{{user.birthDay | date`:'fullDate'`}}</div> <!-- Friday, April 15, 1988 -->
<div>{{user.birthDay | date`:'yyyy-MM-dd HH:mm a z':'+0900'`}}</div> <!-- 1988-04-15 05:03 PM GMT+9 -->
```

---

# Multiple pipes
- Pipes can be piped (like in UNIX)

```html
<div>{{user.birthDay` | date:'fullDate' | uppercase`}}</div>
<!-- FRIDAY, APRIL 15, 1988 -->
```

---

# Custom Pipes
- Declared with `@Pipe` annotation
- `PipeTransform` interface with `transform` method
- `transform` takes the value as first argument and the optional arguments after it
- Must be declared in `NgModule` declaration to be available in templates

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({ name: 'exponential' })
export class ExponentialPipe implements PipeTransform {
  transform(value: number, exponent: number): number {
    return Math.pow(value, exponent);
  }
}
```

```html
<div>{{10 | exponential:3}}</div> <!-- 1000 -->
```

---

# Generating a Pipe
Browse to root of the project and run:

```shell
ng generate pipe capitalize 
```

or abbreviated one:

```shell
ng g p capitalize
```

This will:
- Create a file called `capitalize.pipe.ts` in the root of `app/` folder along with the test stub
- add it as declaration in `AppModule` so it is available in the templates

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
