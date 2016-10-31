# Reactive Programming With Angular 2

---

# Background
- Reactive programming is programming with asynchronous data streams
- JavaScript is asynchronous by design
  - HTTP requests
  - Timeouts
  - UI events (clicks, key presses, etc.)
  - _How to handle all this?_

---

# Callbacks
- Traditionally solved by registering _callback_ functions to be executed upon completion of task
- E.g. `setTimeout` that calls callback function once given amount of milliseconds has passed

```javascript
window.setTimeout(() => {
	// Executed after one second
}, 1000);
```

---

# Problem: Messy Code
- Using callbacks quickly leads to messy code with multiple nested functions that is hard to follow and rationalize

```javascript
getData((x) => {
    getMoreData(x, (y) => {
        getMoreData(y, (z) => {
            ...
        });
    });
});
```

- For more information google for _callback hell_

---

# Solution: Promises
- _Promise_ is a promise of providing a value later
- Promise constructor takes single argument that is a function with two parameters:
  - `resolve`: function to be called when we want to indicate __success__
  - `reject`: function to be called when we want to indicate __failure__

```javascript
new Promise((resolve, reject) => {
  if (...) resolve(x);
  else if (...) resolve(y);
  else reject();
});
```
  - Both functions allow arguments that are provided for promise consumer
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
- Promises don't work for streams, they are just to subscribe for __single events__
- Promises can't be __cancelled__
---

# Solution: Observables
- Generalization of promises for streams
- A way for representing __asynchronous event streams__
  - e.g. mouse clicks, WebSocket streams
- Can also be used for single events e.g. HTTP requests  
---

# RxJS
- _ReactiveX_ is a library for representing __asynchronous event streams__ with __Observables__ and modifying them with various __stream operations__
- _RxJS_ is a _ReactiveX_ implementation for JavaScript
- Angular 2 integrates with __RxJS 5__
---
### Idea:
_"In ReactiveX an observer subscribes to an Observable."_
- You subscribe to stream of events so that your handler gets invoked every time there is a new item
```javascript
observable.subscribe(item => doSomething(item));
```
---
Streams can be manipulated with traditional array conversion functions such as _map_ and _filter_
```javascript
  observable
    .filter(node => node.children.length > 2)
    .map(node => node.name);
```
You can _merge_, _concat_ and do other operations on streams to __produce new streams__ from the existing ones
```javascript
const resultStream = stream1.merge(stream2);
```
[ReactiveX operators](http://reactivex.io/documentation/operators.html)
---

Observables can also be created from e.g. __objects__, __maps__ and __arrays__
```javascript
Rx.Observable.of(42);
Rx.Observable.from([1,2,3,4]);
Rx.Observable.range(1,10);
```
---

# Subscribing
- Subscribe method takes three functions as arguments:
  - __onNext__: called when a new item is emitted
  - __onError__: called if observable sequence fails
  - __onComplete__: called when sequence is complete

  ```javascript
  observable.subscribe(
      next => doSomething(next), // onNext
      error => handleError(error), // onError
      () => done() // onComplete
  );
  ```  
---

# Unsubscribing (cancelling)
- Observable sequence subscriptions can be unsubscribed
  - E.g. observable that produces events that are saved into the memory
    ```javascript
    const eventSubscription = eventStream.subscribe(
          event => this.events.push(event)
    );
    ```
  - Sequence will not stop until unsubscribed
    ```javascript
    eventSubscription.unsubscribe();
    ```
---

# Observables in Action:
## DEMO
---

# Catching Errors
- Observables die on errors
- The way to survive from errors is by catching them and returning a new observable sequence
```javascript
    observable.catch((error) => {
        console.log(error);
        return Rx.Observable.of([1, 2, 3]);
    };
```
---

# Hot vs. Cold Observables
- Cold observables start running __upon subscription__
  - E.g. http request
- Hot observables are already producing values __before the subscription__ is active
  - E.g. mouse move events
---

# Observables in Angular 2
- Observables used extensively instead of promises
  - E.g. HTTP requests can be merely seen as single events (there is only one response) but they are implemented using observables
  ```typescript
      this.http.get('url/restapi/resource') // Returns observable
          .map((res:Response) => res.json()) // Converts response to JSON format
          .subscribe(
              data => { this.data = data}, // Success
              err => console.error(err), // Failure
              () => console.log('done') // Done
          );
  ```
---

# Observables in Angular 2
  - Changes in route parameters are propagated through an observable sequence
  ```typescript
      constructor(route: ActivatedRoute) {
          route.params.subscribe(params => this.index = +params['index'];);
      }
  ```
  - And much more!
---

# Exercises
[Open exercise instructions](https://github.com/gofore/angular2-training/blob/master/reactive-programming-with-angular2/EXERCISES.md)
