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
