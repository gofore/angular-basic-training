# Angular & TypeScript Training - Gofore Oy

## Slides
Available at [angular-basic-training.herokuapp.com](http://angular-basic-training.herokuapp.com/)

---

## Agenda - Day 1
#### Morning
- Introduction to SPAs
- TypeScript & Tooling
- Angular Fundamentals

#### Afternoon
- Angular Fundamentals (continued)
- Angular Advanced Topics

---

## Agenda - Day 2
#### Morning
- Reactive Programming with Angular

#### Afternoon
- Testing

---

# Introduction to SPAs & Angular

## Agenda
- What is a SPA?
- Real-life examples
- Technical overview

---

# What Is a SPA?
- "A single-page application (SPA) is a web application or web site that fits on a single web page with the goal of providing a more fluid user experience similar to a desktop application." - Wikipedia
- Browser fetches executable code that makes asynchronous calls for actual data to be shown
- Data is visualized and/or manipulated and stored back on server asynchronously

---

![SPA flow](spa-intro/spa-flow.png "SPA flow")

---

# Real-life Examples
- [Google search](http://www.google.com)
- [Facebook](http://facebook.com)
- [Twitter](http://twitter.com)

---

# AngularJS
- Revolutionary on its own time
- MVC framework with dependency injection
- Emphasis on testability (decouples DOM manipulation from app logic)
- Two-way data binding

---

# Angular
- Built by 20 core developers working for Google & lots of open source devs
- Built with TypeScript (ES6 and Dart versions available)
- Releases:
    - First beta 1/2016
    - first RC 5/2016
    - 2.0.0 9/2016 
    - 4.0.0 3/2017
    - 5.0.0 11/2017
    - 6.0.0 4/2018
- Complete rewrite of AngularJS
- Not just another web framework, complete platform
- -> Not just for SPAs, but also for desktop and mobile applications
- [Documentation](https://angular.io/docs/ts/latest/api/)

---

# Angular - Cool Parts
- [Ahead-of-Time compilation](https://angular.io/docs/ts/latest/cookbook/aot-compiler.html) - Boost build times by compiling offline
- [Lazy loading](http://angularjs.blogspot.fi/2016/08/angular-2-rc5-ngmodules-lazy-loading.html) - Only load parts of code when needed
- [Universal apps](https://github.com/angular/universal) - render A2 app as HTML on server
- [Progressive Web Apps (PWA)](https://mobile.angular.io/) - Offline web application & push notifications
- [Web workers support](https://angular.io/docs/ts/latest/api/#!?apiFilter=worker) - Boost up performance with threading
- [Migration from AngularJS](https://angular.io/docs/ts/latest/guide/upgrade.html) - Migrating from AngularJS to Angular is possible
- Any rendering target (browser, mobile, desktop) - Build apps to all platforms
  - [Electron](http://electron.atom.io/) for desktop
  - [Ionic](https://ionicframework.com/docs/) (works on top of [Apache Cordova](https://cordova.apache.org/)) for hybrid mobile apps
  - [NativeScript](https://www.nativescript.org/) for native UI mobile apps
