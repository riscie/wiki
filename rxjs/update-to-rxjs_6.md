![](./0.png)
# update to rxjs 6.x.x


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

* [update to rxjs 6.x.x](#update-to-rxjs-6xx)
	* [`do` is now named `tap`](#do-is-now-named-tap)
	* [`.pipe(...)` is used to chain operators](#pipe-is-used-to-chain-operators)
	* [imports are simpler](#imports-are-simpler)
	* [creating obseravbles](#creating-obseravbles)

<!-- /code_chunk_output -->


## `do` is now named `tap`
* reason: do is a reserved js keyword

## `.pipe(...)` is used to chain operators

**Before**
```typescript
import 'rxjs/add/operator/map'; 
import 'rxjs/add/operator/debounceTime'; 
import 'rxjs/add/operator/distinctUntilChanged'; 
//import ...

this.$someSubject 
            .do(console.log)
            .debounceTime(800) 
            .map(value => parseFloat(value)) 
            .filter(value => !isNaN(value))
            .subscribe(result => {
                // ...
            }) ;
```

**Now**
```typescript
import { tap, debounceTime, map, filter } from 'rxjs/operators'; 

this.$someSubject 
    .pipe(
        tap(console.log),
        debounceTime(this.inputDebounceTime), 
        map(value => parseFloat(value)), 
        filter(value => !isNaN(value)), 
    ).subscribe(result => {
                // ...
    }) ;
```

## imports are simpler
* there are only two locations where we need to import from
```typescript
// operators
import { map, tap, filter } from 'rxjs/operators';

// helpers, schedulers, types, ...
import { BehaviorSubject } from 'rxjs';
```

## creating obseravbles
* before: many ways to create an observable (.create, of, ...)
* now: only `of`
```typescript
import { of } from 'rxjs';

of([ 1, 2, 3 ])
    .subscribe(val => //...)
```
