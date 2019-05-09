# Angular Fundamentals
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

# Angular npm Modules
- Framework code is distributed as npm modules:
  - `@angular/animations`: Advanced animations functionality
  - `@angular/common`: Common utilities (pipes, structural directives etc.)
  - `@angular/compiler`: Ahead-of-Time compiler
  - `@angular/core`: Core functionality (always needed)
  - `@angular/forms`: Form handling
  - `@angular/language-service`: Language service for Angular templates for better IDE support
  - `@angular/platform-*`: Platform-specific modules (platforms: browser, server, webworker)
  - `@angular/router`: Routing functionality
  - `@angular/service-worker`: Service worker functionality
  - `@angular/upgrade`: NgUpgrade to upgrade from AngularJS -> Angular

  - `@angular/http`: Deprecated HTTP client

---

# Coding Style
- [Angular style guide](https://angular.io/styleguide) declares set of rules
- [Codelyzer](https://github.com/mgechev/codelyzer) (TSLint plugin) checks for 
- File naming **_name.type.filetype_**:
  - _app.module.ts_
  - _todos.component.ts_
  - _todos.component.html_
  - _todos.component.scss_
  - _user.service.ts_
  - _json.pipe.ts_

---

# Architecture
- App needs to have at least one **module**
- Module has one root **component**
- Component can have child components

---

# NgModules
- Each application has single root _NgModule_
- _NgModule_ is a class with `@NgModule` annotation
- Declares collection of related elements (components, services etc.)

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
- `exports` allows declaring what is exported from this module (covered later)
- `providers` contains list of services for dependency injection
- `bootstrap` contains root element(s) for the application (usually named `AppComponent`)

---

# Booting the application
- We need to tell Angular to start our application
- This is done by providing root module `bootstrapModule`
- This will go through the `bootstrap` array and checks for those selectors in HTML

_main.ts_
```typescript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app/app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```

_index.html_
```html
<html>
    <body>
        <app-root></app-root>
    </body>
</html>
```

---

# Modules in Large Applications
- Modules are meant to offer possibility to divide the functionality in logical modules
- For example one could have `CustomerModule`, `AdminModule` and `BillingModule`
- To access `exports` of another module, it needs to be `imported` to the module:

_app.module.ts_
```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
*import { HttpClientModule } from '@angular/common';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, `HttpClientModule`],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

---

# Exporting 
- Exporting allows declaring the public API provided by the module
- Consider company-specific UI module where `ListComponent` utilizes `ListRowComponent`:

_ui.module.ts_
```typescript
import { NgModule } from '@angular/core';
import { ListComponent } from './list.component';
import { ListRowComponent } from './list-row.component';

@NgModule({
  declarations: [ListComponent, ListRowComponent],
  imports: [],
  exports: [ListComponent],
  providers: []
})
export class UiModule { }
```

---

# Modules as Exports
- Directives `ngIf` and `ngFor` are defined actually in `CommonModule`
- But that is not imported anywhere?
- Actually, `BrowserModule` is exporting `CommonModule` as seen in [source](https://github.com/angular/angular/blob/5.2.9/packages/platform-browser/src/browser.ts#L89)
- -> Modules can be exported as part of module
- If `ListModule` and `TableModule` were separate modules:

_ui.module.ts_
```typescript
import { NgModule } from '@angular/core';
import { ListModule } from './list.module';
import { TableModule } from './table.module';

@NgModule({
  declarations: [],
  imports: [],
  exports: [ListModule, TableModule],
  providers: []
})
export class UiModule { }
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
- Two parameters are mandatory for `@Component` annotation:
  - template with `template` (inline template) or `templateUrl` (separate file)
  - selector (should always start with application specific prefix like the default: `app`)
- Components need to be declared in NgModule's declarations to be available in templates

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

# Generating a Component
Browse to root of the project and run:

```shell
ng generate component todos 
```

or abbreviated one:

```shell
ng g c todos
```

This will:
- Create a folder called `todos` with the component, template, styles and test file
- Add the component to `declarations` of your `AppModule`

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

# Angular Template Syntax
- **Data binding** with _property name_ inside _double curly braces ({{}})_

```typescript
{{property}}
```

- **Structural directives** with _asterisk ( * ) followed by directive name_

```html
<div *ngIf="showItem">Item</div>
```

- **Attribute binding** with _attribute name_ inside _square brackets ([])_

```typescript
<input [disabled]="property" />
```

- **Event binding** with _event name_ inside _parenthesis_

```typescript
<div (click)="clickHandler()"></div>
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
import { Component } from '@angular/core';

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

# Structural Directives
- Modify the *structure* of the template
- Two most used are  `*ngIf` and `*ngFor`

_todos.component.ts_
```typescript
import { Component } from '@angular/core';

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

# Attribute Binding
Bind value from component into HTML attribute

_app.component.ts_
```typescript
import { Component } from '@angular/core';

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

# Special Attribute Bindings
- Classes and styles have special syntax available:
```html
<div [class.active]="isActive"></div>
<div [style.display]="isShown ? 'block' : 'none'"></div>
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
import { Component, Input } from '@angular/core';

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
- The actual event can be referenced with `$event`
- The events are basic [DOM events](https://www.w3schools.com/jsref/dom_obj_event.asp) (like `click`, `mouseover`, `change` and `input`)

_todos.component.html_
```html
<input type="text" `(change)="handleChanged($event.target.value)"`></input>
```

_todos.component.ts_
```typescript
import { Component } from '@angular/core';

@Component({..})
class TodosComponent {
* handleChanged(value: string) {
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
import { Component, Output } from '@angular/core';

@Component({
  selector: 'app-child'
})
class ChildComponent {
* @Output()
* change = new EventEmitter<string>();
}
```

---

# Two-way Data Binding
- Two-way data binding with `ngModel` inside _banana-box syntax_: `[(ngModel)]`
- Data flow is still unidirectional, though:
  - value from component is updated to input on change
  - when user modifies the value, it is updated to component

```html
Name: <input type="text" `[(ngModel)]="name"` />
```

which is just sugar for

```html
Name: <input type="text" `[ngModel]="name" (ngModelChange)="name = $event"` />
```

---

# CSS Encapsulation
- Component can have styles applied:
    - Inline in annotation (field `styles` in `@Component`)
    - In external files (field `styleUrls`)
- These styles are scoped for component -> No other component can get affected by them

_todos.component.html_
```html
<div class="todos"></div>
```

_todos.component.css_
```css
.todos {
    background-color: red;
}
```

Does not affect styles here:

_clients.component.html _
```html
<div class="todos"></div>
```

---

# Components - Inline Styles

_todos.component.ts_
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-todos',
  templateUrl: 'todos.component.html',
  `styles: ['.active-todo { background-color: yellow; }']`
})
class TodosComponent {
}
```

---

# Components - Styles From File

_todos.component.ts_
```typescript
import { Component } from '@angular/core';

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
import { Component, OnInit } from '@angular/core';

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
import { Injectable } from '@angular/core';

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

# Generating a Service
Browse to root of the project and run:

```shell
ng generate service todo 
```

or abbreviated one:

```shell
ng g s todo
```

This will:
- Create a file called `todos.service.ts` in the root of `app/` folder along with the test stub
- *not* add it as provider in `AppModule` so you must do it yourself!

---

# DI with Constructor Parameters
- Angular implements concept called `Dependency Injection`
- In DI you can just ask for the dependencies as constructor parameters as follows:

_todos.component.ts_
```typescript
import { Component } from '@angular/core';
*import { BackendService } from './backend.service';

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

Angular will then pass the singleton instance of `BackendService` to the component when creating it.

---

# Asynchronous and Server-side Communication
- Asynchronous things are modeled as Observables (covered later) in Angular
  - For now, we only need to know that there is `subscribe` method
- For AJAX requests, use `HttpClient` service found in `@angular/common` 

```typescript
import { Component } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Component({...})
export class MyComponent {
  filteredData: any[];
  constructor(private httpClient: HttpClient) {
  }

  getData() {
*    this.httpClient.get('https://example.com/mydata').subscribe(data => {
      // Do stuff with data
      this.filteredData = data.filter(item => item.id > 100);
    })
  }
}
```
