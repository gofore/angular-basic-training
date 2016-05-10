# RxJS demo

- Create an observable sequence that produces a value after each period and subscribe to it
```javascript
var observable = Rx.Observable.interval(500).timeInterval();
var subscription = observable.subscribe(x => console.log(x));
// => {value: 0, interval: 504}
// => {value: 1, interval: 500}
// => {value: 2, interval: 497}
// => {value: 3, interval: 507}
// => {value: 4, interval: 498}
// => ...
```
- Dispose the subscription
```javascript
subscription.dispose();
```
- Add filtering for uneven values
```javascript
var observable = Rx.Observable.interval(500).timeInterval().take(5)
    .filter(x => {return x.value % 2 === 0});
observable.subscribe(x => console.log(x));
```
- Map only the values
```javascript
var observable = Rx.Observable.interval(500).timeInterval().take(5)
    .filter(x => {return x.value % 2 === 0})
    .map(x => {return x.value});
```

- Buffer and accumulate the result
```javascript
var observable = Rx.Observable.interval(500).timeInterval()
    .filter(x => {return x.value % 2 === 0})
    .map(x => {return x.value})
    .bufferWithCount(5)
    .map(arr => {return arr.reduce((acc, cur) => {return acc + cur})});
```

- Mousemove
```javascript
var pos = Rx.Observable.fromEvent(document, 'mousemove')
    .map(e => {return {x:e.clientX, y:e.clientY}});
var s = pos.subscribe(pos => {console.log('x: ' + x + ' y: ' + y)});    
```  
