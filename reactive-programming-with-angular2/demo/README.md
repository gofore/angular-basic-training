# RxJS demo

Create an observable sequence that produces a value after each period and subscribe to it:
```javascript
var observable = Rx.Observable.interval(2000).take(20);
var subscription = observable.subscribe(
  x => console.log('onNext: ' + x),
  e => console.log('onError: ' + e.message),
  () => console.log('onCompleted'));

// => "onNext: 0"
// => "onNext: 1"
// => "onNext: 2"
// => "onNext: 3"
// => "onNext: 4"
// => "onCompleted"
```
Unsubscribe the subscription:
```javascript
subscription.unsubscribe();
```

Add filtering for uneven values:
```javascript
var observable = Rx.Observable.interval(2000).take(20)
  .filter(x => x % 2 === 0);
observable.subscribe(x => console.log(x));
```

Use map to multiply every value by 10:
```javascript
var observable = Rx.Observable.interval(2000).take(20)
  .filter(x => x % 2 === 0)
  .map(x => x * 10);
```

Buffer values:
```javascript
var o = Rx.Observable.interval(500).take(50)
  .filter(x => x % 2 === 0)
  .map(x => x + 10)
  .bufferCount(5);
```
Accumulate the buffered array
```javascript
var observable = Rx.Observable.interval(500)
  .filter(x => x % 2 == 0)
  .map(x => x * 10)
  .bufferCount(5)
  .map(buf => buf.reduce((acc, cur) => acc + cur));
```

Flatmap example:
```javascript
var o = Rx.Observable.range(1,10);
var o2 = o.flatMap(x => Rx.Observable.range(x,2));
o2.subscribe(x => console.log(x));
```

Observable from event: Create Observable from mousemove event and take value emitted every 2000 ms:
```javascript
var move = Rx.Observable.fromEvent(document, 'mousemove')
  .throttleTime(500);
var pos = move.map(e => {
  return {x: e.clientX, y: e.clientY};
});
var s = pos.subscribe(e => console.log('x: ' + e.x + ' y: ' + e.y));
```

Wait for several asynchronous operations to finish: Fork join
```javascript
var o1 = Rx.Observable.create(observer => {
  setTimeout(() => {
    observer.next(1);
    observer.complete();
  }, 5000);
  console.log('o1 started');
});
var o2 = Rx.Observable.create(observer => {
  setTimeout(() => {
    observer.next(2);
    observer.complete();
  }, 3000);
  console.log('o2 started');
});
Rx.Observable.forkJoin([o1, o2]).subscribe(arr => {
  console.log(arr[0] + arr[1]);
});
```
