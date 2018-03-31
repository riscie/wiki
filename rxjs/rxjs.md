# rxjs

* rxjs is the js implementation of ReactiveX
* there are many more languages for wich ReactiveX [is available](http://reactivex.io/languages.html)


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output -->

* [rxjs](#rxjs)
	* [Introduction](#introduction)
	* [Implementing an Observer](#implementing-an-observer)
	* [Implementing an Observable](#implementing-an-observable)
	* [Operators](#operators)
	* [Imports](#imports)

<!-- /code_chunk_output -->


## Introduction
rxjs is: *An API for **asynchronous programming** with **observable streams***
  
**asynchronous programming**
* code that will wait for something to happen, than react to it
* could be: waiting for a button click, an http request, mouse movements, ...

**observable streams**
* think of an observable stream like array, but instead of it being in-memory, the stream gets populated over time
* in the future, the observable stream will get new data. rxjs helps us to observe to these changes and react to them.
* unlike an array, which has a size limitation, a stream can continue infinately into the future

## Implementing an Observer
### Creating Observer using a class (which implements the Observer Interface)
* Shows how to create an Observable from a number array on line 3 and 4.
* Line 6 - 18 declare a class which implements the Observer Interface.
* (This is a very formal way to create an observer.)

```typescript {.line-numbers}
import { Observable, Observer } from "rxjs";

let numbers = [1, 2, 40, 66, 98];
let source = Observable.from(numbers);

class MyObserver implements Observer<number> {
    next(value) {
        console.log(`value: ${value}`);
    }

    error(err) {
        console.log(`error: ${err}`);
    }

    complete() {
        console.log('complete');
    }
}

source.subscribe(new MyObserver());
/* Output:
    value: 1
    value: 2
    value: 40
    value: 66
    value: 98
    complete
*/
```

### Creating a simpler Observer
* A simple way to build an observer
* subscribe will take the three methods which we passed, and provide the Observable with an observer which uses our method implementations
```typescript {.line-numbers}
import { Observable } from "rxjs";

let numbers = [1, 2, 40, 66, 98];
let source = Observable.from(numbers);

source.subscribe(
    value => console.log(`value: ${value}`),
    err => console.log(`error: ${err}`),
    () => console.log('complete')
);
/* Output:
    value: 1
    value: 2
    value: 40
    value: 66
    value: 98
    complete
*/
```

## Implementing an Observable
### Using Observable.create()
* Here we use `Observable.create()` to implement our own observerable
* For each item within numbers, we call the `.next()` method on all our observers
* At the end we call the `.complete()` method.

```typescript {.line-numbers}
import { Observable } from "rxjs";

let numbers = [1, 2, 40, 66, 98];
let source = Observable.create(observer => {
    for (let n of numbers) {
        observer.next(n);
    }
    observer.complete();
});


source.subscribe(
    value => console.log(`value: ${value}`),
    err => console.log(`error: ${err}`),
    () => console.log('complete')
);
/* Output:
    value: 1
    value: 2
    value: 40
    value: 66
    value: 98
    complete
*/
```

### A simple error throwing example
* This example shows how to let the observers know about an error
* (Note that `.complete()` is never called)
```typescript {.line-numbers}
import { Observable } from "rxjs";

let numbers = [1, 2, 40, 66, 98];
let source = Observable.create(observer => {
    for (let n of numbers) {
        if (n === 66){
            observer.error(`66 was not suspected!`)
        }
        observer.next(n);
    }
    observer.complete();
});


source.subscribe(
    value => console.log(`value: ${value}`),
    err => console.log(`error: ${err}`),
    () => console.log('complete')
);
/* Output:
    value: 1
    value: 2
    value: 40
    error: 66 was not suspected!
*/
```

## Operators
* Are chainable
* Return an Observable

## Imports
Instead of

```typescript {.line-numbers}
import { Observable } from "rxjs";
```
we should use more fine grained imports like so

```typescript {.line-numbers}
import { Observable } from "rxjs/Observable";
import "rxjs/add/operator/map"
import "rxjs/add/operator/filter"
//...
```
to reduce our final bundle size.

## Sources
* [rxjs documentation](http://reactivex.io/rxjs/)
* [Pluralsight - Getting Started with Reactive Programming Using RxJS](https://app.pluralsight.com/library/courses/reactive-programming-rxjs-getting-started/table-of-contents)