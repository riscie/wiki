![](./0.png)
# rxjs
* rxjs is the js implementation of ReactiveX
* there are many more languages for wich ReactiveX [is available](http://reactivex.io/languages.html)

## index

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

* [rxjs](#rxjs)
	* [index](#index)
	* [Introduction](#introduction)
	* [Implementing an Observer](#implementing-an-observer)
		* [Creating Observer using a class (which implements the Observer Interface)](#creating-observer-using-a-class-which-implements-the-observer-interface)
		* [Creating a simpler Observer](#creating-a-simpler-observer)
	* [Implementing an Observable](#implementing-an-observable)
		* [Using Observable.create()](#using-observablecreate)
		* [A simple error throwing example](#a-simple-error-throwing-example)
	* [MouseEvent example](#mouseevent-example)
	* [Operators](#operators)
		* [flatmap](#flatmap)
	* [Imports](#imports)
	* [Sources](#sources)

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

```ts
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

```ts
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

```ts 
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

```ts 
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

## MouseEvent example
* Moves an orange ball to the point where the mouse is, with a 300ms delay

```ts 
import { Observable } from "rxjs";

let circle = document.getElementById('circle');

let source = Observable.fromEvent(document, "mousemove")
    .map((e: MouseEvent) => ({x: e.clientX, y: e.clientY}))
    .delay(300);

let onNext = value => {
    circle.style.left = `${value.x}px`;
    circle.style.top = `${value.y}px`;
};

source.subscribe(
    onNext,
    err => console.log(`error: ${err}`),
    () => console.log('complete')
);
```
```html 
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        #circle {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background-color: orange;
            position: absolute;
        }
    </style>
</head>

<body>
    <div id="circle"></div>
    <script src="app.js"></script>
</body>
</html>
```


## Operators
* Are chainable
* Return an Observable

### flatmap
* Merges a an *observable sequence of observable sequences* into a single observable sequence


Let's look at the following example: 
* **map** transforms items emitted by an Observable by applying a function to each item. 
* **flatmap** however applies a specified function to each emitted item and this function in turn returns an Observable for each item. flatMap then merges all these sequences to make a new, single sequence.

```ts 
import { Observable } from "rxjs";

let visitors = ["Namita", "Amit", "Rohit", "Neetika"];
let source = Observable.from(visitors)
    .map(v => Observable.of('Hello ' + v));

source.subscribe(v => console.log(v));
/* Output 1:
    ScalarObservable {_isScalar: true, value: "Hello Namita", scheduler: null}
    ScalarObservable {_isScalar: true, value: "Hello Amit", scheduler: null}
    ScalarObservable {_isScalar: true, value: "Hello Rohit", scheduler: null}
    ScalarObservable {_isScalar: true, value: "Hello Rohit", scheduler: null}
 */

source.flatMap(v => v)
    .subscribe(v => console.log(v));
/* Output 2:
    Hello Namita
    Hello Amit
    Hello Rohit
    Hello Neetika
 */
```

It should be noted, that in the above example, we could use **mergeAll** instead of **flatMap**, because we are not transforming the elements further with flatMap:

```ts 
import { Observable } from "rxjs";

let visitors = ["Namita", "Amit", "Rohit", "Neetika"];
let source = Observable.from(visitors)
    .map(v => Observable.of('Hello ' + v));

source.mergeAll()
    .subscribe(v => console.log(v));
```

## Imports
Instead of

```ts 
import { Observable } from "rxjs";
```
we should use more fine grained imports like so

```ts 
import { Observable } from "rxjs/Observable";
import "rxjs/add/operator/map"
import "rxjs/add/operator/filter"
//...
```
to reduce our final bundle size.

## Sources
* [rxjs documentation](http://reactivex.io/rxjs/)
* [Pluralsight - Getting Started with Reactive Programming Using RxJS](https://app.pluralsight.com/library/courses/reactive-programming-rxjs-getting-started/table-of-contents)
* [rxjs marbles](http://rxmarbles.com)
* [The introduction to Reactive Programming you've been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)