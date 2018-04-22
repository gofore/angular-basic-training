# RxJS Exercises

# Basics
Exercises are done in [JS Bin](http://jsbin.com/negukijoqe/edit?html,js,console)
[RxJS documentation](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html)

## Exercise 1 - Map and Filter
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

## Exercise 2 - Reduce
Given the following stream of integers:
```javascript
Rx.Observable.range(0,10);
```
Calculate the sum of all items.

## Exercise 3 - Scan
Use scan operator to calculate the accumulated value after each integer emitted:
```javascript
Rx.Observable.interval(500).take(10);
```

## Exercise 4 - Merge and Concat
Given the following sources of integers:
```javascript
const source1 = Rx.Observable.interval(500).take(10).map(x => x + 10);
const source2 = Rx.Observable.interval(500).take(10);
```
1. Merge streams
2. Concatenate streams
3. What's the difference between combined streams?

## Exercise 5 - Buffer
Given the following source of integers:
```javascript
Rx.Observable.interval(100).take(100);
```
Calculate the sum of items emitted every second (Tip: check out bufferTime -operator)

## Exercise 6 - Mouse Position
Log mouse position to console on every 500 ms
```javascript
Rx.Observable.fromEvent(document, 'mousemove');
```
[Mousemove Event Documentation](https://developer.mozilla.org/en-US/docs/Web/Events/mousemove) (Tip: check out throttleTime -operator)



# GitHub Search with Observables
In this exercise series we implement an app that uses GitHub's [search-repositories API](https://developer.github.com/v3/search/#search-repositories). 

Start by cloning the application:
```bash
git clone https://github.com/gofore/angular-github-search.git
cd angular-github-search
npm install
npm start
```
- Open `http://localhost:4200` and check that the search works
- Open the code in your IDE

*While doing the exercise, try not to unnecessary poll the API because there is a temporary request limit by GitHub.*

## Exercise 1 - Improving the Structure
The implementation isn't really utilizing full power of reactive programming. For example you should never subscribe twice. Use [`switchMap`](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#instance-method-switchMap) to remove the another `subscribe`.

Tip: `switchMap` is like traditional `map` but it will wait for the value from the Observable  
Tip: Remember to import the `switchMap`:
```typescript
import 'rxjs/add/operator/switchMap';
```

## Exercise 2 - Async Pipe
Actually, we don't want to subscribe manually at all. Let's let Angular do it instead. Use `async` pipe in the template to show the values of our Observable.

Tip: [Async Pipe](https://angular.io/api/common/AsyncPipe)
Tip: Once ready, you should not have `.subscribe()` anywhere
Tip: The type for items should be `Observable<GithubItem[]>`

## Exercise 3 - Improve the Search
Improve the search:
- Don’t hit the search endpoint on every key stroke but instead only after 300ms delay
- Don’t hit the search endpoint with the same query params for subsequent requests
- Don't crash on errors (empty search will result in error so write something and then delete it to test)
- Don't hit the search endpoint on empty query (implement this after the error handling)

Tip: See [RxJS documentation](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html) for existing operators.
Tip: Remember to import the operators:
```typescript
import 'rxjs/add/operator/<operator name>';
```

## Exercise 4 - Use RxJS 5.5 Syntax
Migrate your implementation to use RxJS 5.5 pipeable syntax:

```typescript
import { map } from 'rxjs/operators/map';
import { filter } from 'rxjs/operators/filter';

source.pipe(
    map(i => i * 2),
    filter(i => i < 4)
)
```

# Todo REST API
Modify your todo service to use the given [Todo REST API](https://github.com/gofore/todo-backend). Use it to:
 - List items (GET http://gofore-todo.herokuapp.com/todo-lists/:listId)
 - Add items (POST http://gofore-todo.herokuapp.com/todo-lists/:listId)
 - Modify items (PUT http://gofore-todo.herokuapp.com/todos/:itemId)
- Bonus: Add removal of items
- Bonus: Sort todo list by item id
