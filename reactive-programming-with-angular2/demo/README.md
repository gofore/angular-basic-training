# RxJS demo

Create an observable sequence that produces a value after each period and subscribe to it:
```javascript
const observable = Rx.Observable.interval(2000).take(20);
const subscription = observable.subscribe(
  x => console.log(x),
  e => console.log('onError: ' + e.message),
  () => console.log('onCompleted'));

// => 0"
// => 1"
// => 2"
// => 3"
// => 4"
// => "onCompleted"
```
Unsubscribe the subscription:
```javascript
subscription.unsubscribe();
```

Add filtering for uneven values:
```javascript
const observable = Rx.Observable.interval(2000).take(20)
  .filter(x => x % 2 === 0);
observable.subscribe(x => console.log(x));
```

Use map to multiply every value by 10:
```javascript
const observable = Rx.Observable.interval(2000).take(20)
  .filter(x => x % 2 === 0)
  .map(x => x * 10);
```

Buffer values:
```javascript
const o = Rx.Observable.interval(500).take(50)
  .filter(x => x % 2 === 0)
  .map(x => x + 10)
  .bufferCount(5);
```

Accumulate the buffered array
```javascript
const observable = Rx.Observable.interval(500)
  .filter(x => x % 2 === 0)
  .map(x => x * 10)
  .bufferCount(5)
  .map(buf => buf.reduce((acc, cur) => acc + cur));
```

Observable from event: Create Observable from mousemove event and take value emitted every 2000 ms:
```javascript
const move = Rx.Observable.fromEvent(document, 'mousemove')
  .throttleTime(500);
const pos = move.map(e => {
  return {x: e.clientX, y: e.clientY};
});
const s = pos.subscribe(e => console.log('x: ' + e.x + ' y: ' + e.y));
```

Flatmap example:
```javascript
const o = Rx.Observable.range(1,10);
const o2 = o.flatMap(x => Rx.Observable.range(x,2));
o2.subscribe(x => console.log(x));
```

Wait for several asynchronous operations to finish: Fork join
```javascript
const o1 = Rx.Observable.create(observer => {
  setTimeout(() => {
    observer.next(1);
    observer.complete();
  }, 5000);
  console.log('o1 started');
});
const o2 = Rx.Observable.create(observer => {
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
