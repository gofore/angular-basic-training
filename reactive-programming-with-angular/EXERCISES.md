# RxJS Exercises

Exercises are done in [JS Bin](http://jsbin.com/negukijoqe/edit?html,js,console)
[RxJS documentation](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html)

# Exercise 1 Map and Filter
Given the following stream of objects:
```javascript
Rx.Observable.from([
  {name: 'first', id: 1},
  {name: 'second', id: 2},
  {name: 'bob', id: 3},
  {name: 'john', id: 4},
  {name: 'foo', id: 5},
  {name: 'bar', id: 6},
  {name: 'aa', id: 7}
]);
```
1. Filter items which have the name length shorter than 4
2. From filtered stream, map only the item id's to a stream and subscribe to it

# Exercise 2 Reduce
Given the following stream of integers:
```javascript
Rx.Observable.range(0,10);
```
Calculate the sum of all items

# Exercise 3 Scan
Use scan operator to calculate the accumulated value after each integer emitted:
```javascript
Rx.Observable.interval(500).take(10);
```

# Exercise 4 Merge and Concat
Given the following sources of integers:
```javascript
const source1 = Rx.Observable.interval(500).take(10).map(x => x + 10);
const source2 = Rx.Observable.interval(500).take(10);
```
1. Merge streams
2. Concatenate streams
3. What's the difference between combined streams?

# Exercise 5 Buffer
Given the following source of integers:
```javascript
Rx.Observable.interval(100).take(100);
```
Calculate the sum of items emitted every second (Tip: check out bufferTime -operator)

# Exercise 6 Mouse Position
Log mouse position to console on every 500 ms
```javascript
Rx.Observable.fromEvent(document, 'mousemove');
```
[Mousemove Event Documentation](https://developer.mozilla.org/en-US/docs/Web/Events/mousemove) (Tip: check out throttleTime -operator)

# Exercise 7 GitHub Search with Observables
In this exercise series we implement an app that uses GitHub's [search-repositories API](https://developer.github.com/v3/search/#search-repositories). Start by cloning the exercise seed:
```bash
git clone https://github.com/gofore/angular2-exercises.git angular2-search
cd angular2-search
git checkout search
npm install
ng serve
```
- Open http://localhost:4200 and check that the search works
- Open the code in your IDE

# Exercise 8 Async Pipe

Store your search result into an Observable (instead of subscribing it) and use async pipe in the template to show the result.

- [Angular pipes](https://angular.io/docs/ts/latest/guide/pipes.html)

# Exercise 9 Show Search Results Instantly

- Show search results instantly when typing
- No need for search button anymore

# Exercise 10 Improve the Search
- Add the following improvements:
  - Don’t hit the search endpoint on every key stroke
  - Don’t hit the search endpoint with the same query params for subsequent requests
  - Deal with out-of-order responses
  - Don't crash on errors
  - Don't hit the search endpoint on empty query (implement this after the error handling)

# (Exercise 11 Todo REST API)

- Modify your todo service to use the given [Todo REST API](https://github.com/gofore/todo-backend). Use it to:
  - List items (GET http://gofore-todo.herokuapp.com/todo-lists/:listId)
  - Add items (POST http://gofore-todo.herokuapp.com/todo-lists/:listId)
  - Modify items (PUT http://gofore-todo.herokuapp.com/todos/:itemId)
- Bonus: Add removal of items
- Bonus: Sort todo list by item id
