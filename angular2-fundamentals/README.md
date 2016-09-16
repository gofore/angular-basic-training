# Angular 2 Fundamentals
- Background
- Architecture
- Components
- Templates
- Component Lifecycle Hooks
- Two-way Data Binding
- Services
- Asynchronous and Server-side Communication

---

# Background
- Based on successful Angular 1
- First beta on December 2015
- First release candidate on April 2016
- Initial release on 14th of September 2016

---

# Angular 2 & TypeScript
- Available for JavaScript, Dart and **TypeScript**
- TypeScript is de facto standard and recommended by many A2 core team members
- Framework code is distributed in modules such as `@angular/core` and `@angular/common`

---

# Architecture
- Tree structure of components
  - Each component has a template in which more components can be used
  - Components can also use services (singletons) to e.g. share data

![Component tree](angular2-fundamentals/component-tree.png "Component tree")

---

# File Structure
- Split to domains (_todo_ & _other_domain_)
- File naming _name.type.file_type_:
  - _todo.component.ts_
  - _user-service.component.ts_
```
project
│  package.json
└──src
    │  app.component.ts
    |  app.module.ts
    │  index.html
    ├──todo
    │   └──components
    │       │   todos.component.html
    │       │   todos.component.ts
    │       │   todo.component.html
    │       │   todo.component.ts
    │   └──services
    │       │   todo.service.ts
    │   └──pipes
    └──other_domain
```

---

# NgModules
- Each app defines at least single _NgModule_ with `@NgModule` annotation
- Declares single unit of things relating to same thing
- Defines template compilation context

_app.module.ts_
```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent, MyComponent, MyDirective, CustomPipe],
  imports: [BrowserModule],
  providers: [UserService, LessonsService],
  bootstrap: [AppComponent]
})
export class AppModule {
}
```
---

# NgModules - Details

- `declarations` contains list of application build blocks, such as components, pipes and directives, with certain selector
- `imports` allows importing of other _NgModules_
  - For example `BrowserModule` imports browser-specific renderers and core directives such as `ngFor` and `ngIf`
- `providers` contains list of services for dependency injection
- `bootstrap` contains root element for the application (usually named `AppComponent`)

---

# Components
- Defines a build block for application UI
- Has a template that it binds data to
- Can contain other components

---

# Bootstrapping the Application

_main.ts_
```typescript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { enableProdMode } from '@angular/core';
import { AppModule } from './app/app.module';

if (false) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule);
```

---

# Components - `@Component` Annotation
Class with `@Component` annotation

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({})
class TodosComponent {
}
```

---

# Components - Templates
Template with `templateUrl`

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  `templateUrl: 'todos.component.html'`
})
class TodosComponent {
}
```

_todos.component.html_
```html
<div>
  <h1>My todos</h1>
</div>
```

---

# Components - Selector
Selector for usage in templates

_todos.component.ts_
```typescript
import {Component} from '@angular/core';
@Component({
  templateUrl: 'todos.component.html',
  `selector: 'todos'`
})
class TodosComponent { }
```

_app.component.html_
```html
<todos></todos>
```

---

# Components - Data Binding
Binding data from the component to the template.

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'todos',
  templateUrl: 'todos.component.html'
})
class TodosComponent {
  `private todoTask: string = 'Do the laundry';`
}
```

_todos.component.html_
```html
<div>
  <h1>`{{todoTask}}`</h1>
</div>
```

---
# Components - Iterating over Array
Showing an property array in the template.

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'todos',
  templateUrl: 'todos.component.html'
})
class TodosComponent {
  `private todos: any[] = [{name: 'Do the laundry'}, {name: 'Clean my room'}];`
}
```

_todos.component.html_
```html
<div `*ngFor="let todo of todos"`>
  {{todo.name}}
</div>
```

---

# Components - Event Handlers
Registering handler code for events.

_todos.component.html_
```html
<div *ngFor="let todo of todos">
  <div `(click)="openTodo(todo)"`>{{todo.name}}</div>
</div>
```

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'todos',
  templateUrl: 'todos.component.html'
})
class TodosComponent {
  todos: any[] = [{name: 'Do the laundry'}, {name: 'Clean my room'}];  

* openTodo(todo: Todo) {
*   // Open todo
* }
}
```

---
# Components - Inputs
Passing data from parent component to child.

_todos.component.html_
```html
<div *ngFor="let todo of todos">
  <todo `[item]="todo"`></todo>
</div>
```

_todo.component.ts_
```typescript
import {Component, Input} from '@angular/core';

@Component({
  selector: 'todo'
})
class TodoComponent {
* @Input() item: Todo;
}
```


---

# Components - Outputs
Passing events from child component to parent component.

_todo.component.ts_
```typescript
import {Component, Output, EventEmitter} from '@angular/core';

@Component({
  selector: 'todo',
  templateUrl: 'todo.component.html'
})
class TodoComponent {
* @Output() prioritized: EventEmitter<number>;

* prioritize(priority: number) {
*   this.rate.emit(priority);
* }
}
```

_todos.component.html_
```html
<div *ngFor="let todo of todos">
  <todo [item]="todo" `(prioritized)="currentPriority = $event"`></todo>
  {{currentPriority}}
</div>
```

---

# Components - DI with Constructor Parameters
Pass services (and other injectables) for usage in the component.

_todos.component.ts_
```typescript
import {Component} from '@angular/core';
*import {BackendService} from './backend.service';

@Component({
  selector: 'todos',
  templateUrl: 'todos.component.html'
})
class TodosComponent {
* private backendService: BackendService;
* constructor(backendService: BackendService) {
*   this.backendService = backendService;
* }
}
```

---

# Components - Visibility
Store dependency as property with visibility.

_todos.component.ts_
```typescript
import {Component} from '@angular/core';
import {BackendService} from './backend.service';

@Component({
  selector: 'todos',
  templateUrl: 'todos.component.html'
})
class TodosComponent {
  constructor(`private` backendService: BackendService) {}
}
```

---

# Components - Inline Styles
Inline styles to be used within the template. Scoped for just this element by A2.

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'todos',
  templateUrl: 'todos.component.html',
  `styles: ['.active-todo { background-color: yellow; }']`
})
class TodosComponent {
}
```

Styles only visible within this component (_todos.component.html_)

---

# Components - Styles from URL
Style files to be used within the template. Scoped for just this element by A2.

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'todos',
  templateUrl: 'todos.component.html',
  `styleUrls: ['todos.component.css']`
})
class TodosComponent {
}
```

---

# Component-template bindings
- There are different types of interaction between the component and template
  - Bind data to be shown with `{{property name}}`
  - Attributes with `[attribute name]`
  - Event handlers with `(event name)`
  - Templates with `*template name`


_todos.component.html_

```html
<h2>Todo `{{name}}`</h2>
<div `*ngFor`="let todo of todos">
  <todo `[data]`="todo" `(click)`="openTodo(todo)"></todo>
</div>
```

---

# Component Lifecycle Hooks
- For hooking into certain lifecycle events
- Interface for each hook
- Example hooks: `ngOnInit`, `ngOnChanges` and `ngOnDestroy`

```typescript
import {Component, OnInit} from '@angular/core';

export class MyComponent `implements OnInit` {
  `ngOnInit() { ... }`
}
```

---

# Two-way Data Binding
- Combining the `()` (output) and `[]` (input) as `([])` gives us two-way data binding
- `ngModel` allows us to bind input field into components property two-way:

```html
Name: <input type="text" `[(ngModel)]="name"` />
```

which is just sugar for

```html
Name: <input type="text" `[ngModel]="name" (ngModelChange)="name = $event"` />
```

---

# Services
- Application-wide singletons
- Used to store state, communicate with backends etc.
- Examples:
  - `UserService`
  - `BackendService`
- Declaring:

_user.service.ts_
```typescript
import {Injectable} from '@angular/core';

*@Injectable()
export class UserService {
}
```

---

# Services - Using Other Services
Inject other services to be used inside this service.

_user.service.ts_
```typescript
import {Injectable} from '@angular/core';

@Injectable()
export class UserService {
  constructor(`backendService: BackendService`) {
  }
}
```

---

# Asynchronous and Server-side Communication
- Asynchronous things are modeled as Observables (covered on advanced topics) in A2
  - For now, we only need to know that there is `subscribe` method
- For AJAX requests, there is `Http` service with support for GET, POST, PUT, DELETE, HEAD and PATCH requests

```typescript
import {Component} from '@angular/core';
import {Http} from '@angular/http';

@Component({...})
export class MyComponent {
  private filteredData: any[];
  constructor(private http: Http) {
  }

  getData() {
*    this.http.get('https://example.com/mydata').subscribe(data => {
      // Do stuff with data
      this.filteredData = data.filter(item => item.id > 100);
    })
  }
}
```
