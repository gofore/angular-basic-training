# Angular 2 Fundamentals
- npm Modules
- File Structure
- Architecture
- NgModules
- Components
- Templates
- Component Lifecycle Hooks
- Two-way Data Binding
- Services
- Asynchronous and Server-side Communication

---

# Angular 2 npm Modules
- Framework code is distributed as npm modules:
  - `@angular/common`: Common utilities (pipes, structural directives etc.)
  - `@angular/core`: Core functionality (always needed)
  - `@angular/forms`: Form handling
  - `@angular/http`: HTTP requests
  - `@angular/platform-*`: Platform-specific modules (platforms: browser, server, webworker)
  - `@angular/router`: Routing functionality
  - `@angular/upgrade`: NgUpgrade to upgrade from A1 -> A2

---

# File Structure
- [Angular 2 style guide](https://angular.io/styleguide) declares set of rules
- [Codelyzer](https://github.com/mgechev/codelyzer) (tslint plugin) lets you lint against them
- File naming **_name.type.filetype_**:
  - _my.module.ts_
  - _todo.component.ts_
  - _user.service.ts_
  - _json.pipe.ts_
  - _yellow-background.directive.ts_

---

# Architecture
- App needs to have at least one **module**
- Module has one root **component**
- Component can have child components

---

# NgModules
- Each app has single root _NgModule_
- Declared with `@NgModule` annotation
- Declares single unit of things relating to same thing
- Defines template compilation context

_app.module.ts_
```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

---

# NgModules - Details
- `declarations` contains list of application build blocks, such as components, pipes and directives, with certain selector
- `imports` allows importing of other _NgModules_
  - For example `BrowserModule` imports browser-specific renderers and core directives such as `ngFor` and `ngIf`
- `providers` contains list of services for dependency injection
- `bootstrap` contains root element for the application (usually named `AppComponent`)

---

# Booting the application
- Browser executes (compiled) _main.ts_
- _main.ts_ is responsible for setting up our application
- Root module provided for Angular `bootstrapModule`

_main.ts_
```typescript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { enableProdMode } from '@angular/core';
import { environment } from './environments/environment';
import { AppModule } from './app/app.module';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule);
```

---

# Components
- Build blocks of application UI
- Has a template that it binds data to
- Can contain other components
- Class with `@Component` annotation

_todos.component.ts_
```typescript
import { Component } from '@angular/core';

@Component({})
class TodosComponent { }
```

---

# Components
- Two parameters are mandatory for `@Component`:
  - template with `template` (inline template) or `templateUrl` (separate file)
  - selector (should always start with `app` prefix)
- Components need to be declared in NgModule's declarations to be available

_todos.component.ts_
```typescript
import { Component } from '@angular/core';

@Component({
  `templateUrl: 'todos.component.html'`,
  `selector: 'app-todos'`
})
class TodosComponent { }
```

_app.module.ts_
```typescript
@NgModule({
  declarations: [AppComponent, `TodosComponent`]
  ...
})
export class AppModule { }
```

---

# Templates
- Plain HTML with few Angular specific additions*

_todos.component.html_
```html
<h1>My todo application</h1>
```

- Selectors can be used to add other components

_app.component.html_
```html
<h2>Todos</h2>
<app-todos></app-todos>
```

---

# Template Syntax
- Five additions to HTML
  - **Data binding** with _property name_ inside _double curly braces ({{}})_

  ```typescript
  {{property}}
  ```

  - **Attribute binding** with _attribute name_ inside _square brackets ([])_

  ```typescript
  <input [disabled]="property" />
  ```

  - **Event binding** with _event name_ inside _parenthesis_

  ```typescript
  <div (click)="clickHandler()"></div>
  ```

  - **Structural directives** with _asterisk ( * ) followed by directive name_

  ```html
  <div *ngIf="showItem">Item</div>
  ```

  - **Template local variables** with _hash (#) followed by name_

  ```typescript
  <input #nameInput />
  ```

---

# Data Binding
Bind property from the component to the template

_app.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'app-component',
  templateUrl: 'app.component.html'
})
class AppComponent {
  `title: string = 'app works!';`
}
```

_app.component.html_
```html
<h1>`{{title}}`</h1>
```

---

# Attribute Binding
Bind value from component into HTML attribute

_app.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'app-component',
  templateUrl: 'app.component.html'
})
class AppComponent {
  `isDisabled: boolean = true;`
}
```

_app.component.html_
```html
<input [disabled]="isDisabled" />
```

---

# Attribute Binding for Components
- Attribute binding only works for native properties of HTML elements by default
- Same concept can also be used to pass data from parent component to child component

_parent.component.html_
```html
  <app-child `[foo]="todo"`></app-child>
```

_child.component.ts_
```typescript
import {Component, Input} from '@angular/core';

@Component({
  selector: 'app-child'
})
class ChildComponent {
* @Input()
* foo: string;
}
```

---

# Event Binding
- Register handler code for events
- `$event` contains the value of event

_todos.component.html_
```html
<input type="text" `(change)="handleNewValue($event)"`></input>
```

app.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({..})
class AppComponent {
* handleNewEvent(value: string) {
*   // Do something with value
* }
}
```

---

# Event Binding for Components
- Event binding only works for native events of HTML elements by default
- Same concept can also be used to pass data from child component to parent component

_parent.component.html_
```html
  <app-child `(change)="todo"`></app-child>
```

_child.component.ts_
```typescript
import {Component, Output} from '@angular/core';

@Component({
  selector: 'app-child'
})
class ChildComponent {
* @Output()
* change: EventEmitter<string> = new EventEmitter();
}
```

---

# Two-way Data Binding
- Two-way data binding with `ngModel` inside _banana-box syntax_: `[(ngModel)]`
- Data flow is bi-directional:
  - value from component is updated to input
  - when user modifies the value, it is updated to component

```html
Name: <input type="text" `[(ngModel)]="name"` />
```

which is just sugar for

```html
Name: <input type="text" `[ngModel]="name" (ngModelChange)="name = $event"` />
```

---

# Structural Directives
- Modify the structure of template
- Two most used are  `*ngIf` and `*ngFor`

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'app-todos',
  templateUrl: 'todos.component.html'
})
class TodosComponent {
  `todos: any[] = [{name: 'Do the laundry'}, {name: 'Clean my room'}];`
}
```

_todos.component.html_
```html
<div `*ngFor="let todo of todos"`>
  {{todo.name}}
</div>
```

---

# Components - Inline Styles
Inline styles to be used within the template. Scoped for just this element by A2.

_todos.component.ts_
```typescript
import {Component} from '@angular/core';

@Component({
  selector: 'app-todos',
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
  selector: 'app-todos',
  templateUrl: 'todos.component.html',
  `styleUrls: ['todos.component.css']`
})
class TodosComponent {
}
```

---

# Component Lifecycle Hooks
- For hooking into certain lifecycle events
- Interface for each hook
- Example hooks: `ngOnInit` (interface `OnInit`), `ngOnChanges` (interface `OnChanges`) and `ngOnDestroy` (interface `OnDestroy`)

```typescript
import {Component, OnInit} from '@angular/core';

export class MyComponent `implements OnInit` {
  `ngOnInit() { ... }`
}
```

- `ngOnInit` should be used for initialization of data for testability

---

# Services
- Module-wide singletons
- Used to store state, communicate with backends etc.
- Examples: `UserService`, `BackendService`
- Declaring:

_user.service.ts_
```typescript
import {Injectable} from '@angular/core';

*@Injectable()
export class UserService {
}
```

- Need to be registered for NgModule

_app.module.ts_
```typescript
@NgModule(
  ...
* providers: [UserService]
)
export class AppModule() {}
```

---

# DI with Constructor Parameters
Services can be accessed by declaring them inside constructor parameters.

_todos.component.ts_
```typescript
import {Component} from '@angular/core';
*import {BackendService} from './backend.service';

@Component({
  selector: 'app-todos',
  templateUrl: 'todos.component.html'
})
class TodosComponent {
* constructor(private backendService: BackendService) {
* }

  initializeData() {
    this.backendService.makeRequest();
  }
}
```

---

# Asynchronous and Server-side Communication
- Asynchronous things are modeled as Observables (covered later) in A2
  - For now, we only need to know that there is `subscribe` method
- For AJAX requests, there is `Http` service with support for GET, POST, PUT, DELETE, HEAD and PATCH requests

```typescript
import {Component} from '@angular/core';
import {Http} from '@angular/http';

@Component({...})
export class MyComponent {
  filteredData: any[];
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
