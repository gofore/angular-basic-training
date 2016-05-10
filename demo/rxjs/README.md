# RxJS demo

Create an observable sequence that produces a value after each period and subscribe to it:
```javascript
var observable = Rx.Observable.interval(500).timeInterval();
var subscription = observable.subscribe(
    x => console.log(x),
    e => console.log('onError: ' + e.message),
    () => console.log('onCompleted'));
// => {value: 0, interval: 504}
// => {value: 1, interval: 500}
// => {value: 2, interval: 497}
// => {value: 3, interval: 507}
// => {value: 4, interval: 498}
// => ...
```
Dispose the subscription:
```javascript
subscription.dispose();
```

Add filtering for uneven values:
```javascript
var observable = Rx.Observable.interval(500).timeInterval().take(5)
    .filter(x => x.value % 2 === 0);
observable.subscribe(x => console.log(x));
```

Map only the values from the value interval object:
```javascript
var observable = Rx.Observable.interval(500).timeInterval().take(5)
    .filter(x => x.value % 2 === 0)
    .map(x => x.value);
```

Buffer and accumulate the result:
```javascript
var observable = Rx.Observable.interval(500).timeInterval()
    .filter(x => x.value % 2 === 0)
    .map(x => x.value)
    .bufferWithCount(5)
    .map(arr => arr.reduce((acc, cur) => acc + cur));
```

Concatenate and merge Observables:
```javascript
var o = Rx.Observable.fromArray([1,2,3,4,5]);
var o2 = Rx.Observable.fromArray([6,7,8,9]);
o.concat(o2).subscribe(x => console.log(x));
o.merge(o2).subscribe(x => console.log(x));
```
Flatmap example:

Create Observable from mousemove event and take value emitted every 500 ms
```javascript
var pos = Rx.Observable.fromEvent(document, 'mousemove')
    .map(e => {return {x:e.clientX, y:e.clientY}}).throttle(500);
var s = pos.subscribe(pos => {console.log('x: ' + pos.x + ' y: ' + pos.y)});    
```  
