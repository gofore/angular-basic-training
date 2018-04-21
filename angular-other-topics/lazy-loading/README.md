# Lazy Loading

---
# Background
- Web applications are used more and more with mobile devices
- Many countries still have slow internet connections
- Load time has a huge impact on user experience


---
# Lazy Loading
- Traditional apps load everything in the beginning
- Lazy loading means only loading parts after initial page load

---
# Default Bundling

![Build without lazy loading](angular-other-topics/lazy-loading/build-without-lazy-loading.png)

Chunks are:
- _polyfills.bundle.js_: Polyfills required to run the app (see _polyfills.ts_)
- _main.bundle.js_: Actual application code along with vendor code (Angular)
- _styles.bundle.css_: Styles for the application
- _inline.bundle.js_: Tiny webpack loader to load the app

---
# Custom Chunks

![Build with lazy loading](angular-other-topics/lazy-loading/build-with-lazy-loading.png)

---
# Angular Module Loading
- By default, modules are loaded eagerly when the application starts
- Modules can loaded lazily by the router with `loadChildren`

---
# Angular Module Loading
Main bundle is loaded when application starts

![Main Module](angular-other-topics/lazy-loading/main.PNG)
---

# Angular Module Loading
A feature module is loaded when it is routed to the first time
```html
<a [routerLink]="['feature']">Click me</a>
```
![Lazy Loaded Module](angular-other-topics/lazy-loading/chunk.PNG)

---
# Angular CLI
- Angular CLI has built-in support for lazy loading
- Works for both development and production builds
- If `loadChildren` is detected chunks will be generated -> no extra configuration required

---
# Router Configuration

_app.module.ts_
```javascript
const routes: Routes = [
  {
    path: '',
    component: AppComponent
  },
  {
    path: 'todos',
    loadChildren: './todos/todos.module#TodosModule'
  }];

@NgModule({
  ...
  imports: [
    ...
    RouterModule.forRoot(routes)    
  ],
})
```

---
# Router Configuration

_todos.module.ts_
```javascript
const routes: Routes = [
  {
    path: '',
    component: TodosComponent
  },
  {
    path: ':id',
    component: EditTodoComponent
  }
];

@NgModule({
  ...
  imports: [
    ...
    RouterModule.forChild(routes)
  ],
})
export class TodosModule {}
```

---
# Demo

[https://github.com/RoopeHakulinen/angular-lazy-loading](https://github.com/RoopeHakulinen/angular-lazy-loading)

---

# Analyzing Bundle Sizes

```bash
{
  "scripts": {
     ...
    "analyze": "ng build --prod --stats-json && webpack-bundle-analyzer dist/stats.json"
  }
}
```

```bash
npm install webpack-bundle-analyzer --save-dev
npm run analyze
```