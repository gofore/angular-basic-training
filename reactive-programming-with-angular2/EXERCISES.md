# Exercise 4 - RxJS

Open [JS Bin template](http://jsbin.com/kepoyac/2/edit?html,js,console) for RxJS Observable exercise

# Exercise 4.1 Map and Filter
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

# Exercise 4.2 Reduce
Given the following stream of integers:
```javascript
Rx.Observable.range(0,10);
```
Calculate the sum of all items

# Exercise 4.3 Merge and Concat
Given the following sources of integers:
```javascript
var source1 = Rx.Observable.interval(500).take(10);
var source2 = Rx.Observable.interval(500).take(10);
```
Compare the outputs of merge and concat operators

# Exercise 4.4 Buffer
Given the following source of integers:
```javascript
Rx.Observable.interval(100).take(100);
```
Calculate the sum of items emitted within one second

# Exercise 4.5 Double Click
From the mouse click stream:
```javascript
Rx.Observable.fromEvent(document, 'click');
```
Use stream operators to detect double clicks (two clicks within 250 ms)

Tips:
- [debounceTime operator](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/debounce.md)
- [buffer operator](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/buffer.md)

# Exercise 4.6 GitHub Search with Observables

Implement an app that uses GitHub's [search-repositories API](https://developer.github.com/v3/search/#search-repositories)

Requirements:
- Input field for search
- Search button
- Show a list of results with links to repositories. Use:
  - item.full_name
  - item.html_url

# Exercise 4.7 Async Pipe

Store your search result into an Observable (instead of subscribing it) and use async pipe in the template to show the result.

- [Angular 2 pipes](https://angular.io/docs/ts/latest/guide/pipes.html)

# Exercise 4.8 Show Search Results Instantly

- Show search results instantly when typing
- No need for search button anymore

# Exercise 4.9 Improve the Search
- Add the following improvements:
  - Don’t hit the search endpoint on every key stroke
  - Don’t hit the search endpoint with the same query params for subsequent requests
  - Deal with out-of-order responses
  - Don't crash on errors
  - Don't hit the search endpoint on empty query (implement this after the error handling)
