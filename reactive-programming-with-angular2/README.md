# Reactive Programming With Angular 2

---

# Background

- Reactive programming is programming with asynchronous data streams
- JavaScript is asynchronous by design
  - HTTP requests
  - Timeouts
  - UI events (clicks, key presses, etc.)

---

# Callbacks

- Traditionally solved by registering _callback_ functions to be executed upon completion of task
- E.g. `setTimeout` that calls callback function once given amount of milliseconds has passed

```javascript 
window.setTimeout(function() {
	// Executed after one second
}, 1000);
```

---

# Problem: Messy Code

- Using callbacks quickly leads to messy code with multiple nested functions that is hard to follow and rationalize

```javascript
getData(function(x){
    getMoreData(x, function(y){
        getMoreData(y, function(z){
            ...
        });
    });
});
```

- For more information google for _callback hell_

---

# Solution: Promises

- _Promise_ is a promise of providing a value later
- Implemented in ES6
- Promise constructor takes single argument that is a function with two parameters:
  - `resolve`: function to be called when we want to indicate success
  - `reject`: function to be called when we want to indicate failure
- Both functions allow arguments that are provided for promise consumer

```javascript
new Promise(function(resolve, reject) {
  if (...) resolve(x);
  else if (...) resolve(y);
  else reject();
});
```

---

# Promises are Resolved or Rejected

- Promises are consumed by calling `then` on them. `then` takes two arguments: success and failure handler

```javascript
somethingReturningPromise().then(
  (value) => { // Resolved
    // Handle success case
  },
  (value) => { // Rejected
    // Handle reject case, e.g. show an error note
  });
```

---

# Problem: Stream Handling and Disposability

- Promises don't work for streams, they are just to subscribe for single events
- Promises can't be cancelled

---

# Solution: Observables

- Generalization of promises for streams
- Idea:
  - You subscribe to stream of events so that your handler gets invoked every time there is a new item
  - Streams can also be manipulated with traditional array conversion functions such as _map_ and _filter_
  - You can merge, concat and do other operations on streams to produce new streams from the existing ones
  - Observables can be disposed and some code (e.g. cleaning) be executed on disposal
- To be included in _EcmaScript standard_ in future (as of May 2016 they're still in phase 1 out of 4 before reaching standardization)

---

# RxJS

- _Reactive Extensions'_ implementation for JavaScript
- A set of libraries for representing asynchronous data streams with Observables and modifying them with various stream operations
- Allow observables to also be created from maps and arrays
- Works with Promises

---

# Observables in Angular 2

- RxJS used
- Observables used extensively instead of promises
  - E.g. HTTP requests can be merely seen as single events (there is only one response) but they are implemented as observables for convenience
- Change detection with observables
  - Background: Angular 2 change detection is based on a directed tree where all the components get updated starting from the root when a change occurs.
  - If Angular 2 component depends only on its input properties, and they are observable, change detection can skip component's subtree until one of its input properties emits an event
  - This can bring performance benefits e.g. if you use observables in some gigantic table

---

# Observables in action

- Observable vs Promise
- Filter, Map
- Dispose
- Fork join

---
