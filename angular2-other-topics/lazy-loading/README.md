# Lazy Loading

---
# Lazy Loading
Speeds up application load time by splitting it into multiple bundles and loading them on demand

---
# Background
- Applications are used more and more with mobile devices
- Many countries still have slow internet connections
- Load time effects a lot on user experience

---
# Angular 2 Module Loading
- By default, modules are loaded eagerly when the application starts
- They can also be loaded lazily by the router
- Lazy loading: Module is loaded when it is routed to the first time

---
# Angular 2 Module Loading
Main bundle is loaded when application starts

![Main Module](angular2-other-topics/lazy-loading/main.PNG)
---

# Angular 2 Module Loading
A feature module is loaded when it is routed to the first time
```html
<a [routerLink]="['feature']">Click me</a>
```
![Lazy Loaded Module](angular2-other-topics/lazy-loading/chunk.PNG)

---
# Simple & Easy
- Easy to configure with the router
- Angular-CLI supports it by default

---
# Router Configuration
- AppModule _routerConfig_
- loadChildren property defines module location and module class, separated by #

```javascript
const routeConfig = [
  {
    path: '',
    redirectTo: 'MainComponent'
  },
  {
    path: 'feature',
    loadChildren: 'app/feature/feature.module#FeatureModule'
  }];
```

---
# Router Configuration
- FeatureModule _routerConfig_

```javascript
const routeConfig = [
  {
    path: '',
    component: FeatureComponent
  }
];
```
