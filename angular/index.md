![](./0.png)
# angular

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output -->

* [angular](#angular)
	* [functions vs. pipe](#functions-vs-pipe)

<!-- /code_chunk_output -->


## functions vs. pipe
This will get called **many times more often**
```html
<!-- using a function -->
<tr *ngFor="let player of players">
    <td>{{getScore(player)}}</td>
</tr>
```

than this
```html
<!-- using a pipe -->
<tr *ngFor="let player of players">
    <td>{{player | rating}}</td>
</tr>
```

### why?
* angular will remember the input value of the pipe and **cache** the output
* if the input value has not changed, it will use the cached value

### careful with object-inputs in pipes!
* if a pipe takes an object as an input and inside this object a primitive changes, angular will reevaluate the output
* it's a good idea to **keep pipe inputs as primitive as possible**
* or use **immutable objects**

### source
* [ngconf 2018 - Increasing Performance - more than a pipe dream - Tanner Edwards](https://www.youtube.com/watch?v=I6ZvpdRM1eQ)