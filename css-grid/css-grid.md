![](./0.png)
# css-grid

## index

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

* [css-grid](#css-grid)
	* [index](#index)
	* [basic setup](#basic-setup)
	* [explicit vs. implicit tracks](#explicit-vs-implicit-tracks)
	* [grid-auto-flow](#grid-auto-flow)

<!-- /code_chunk_output -->

## basic setup

```html
<div class="container">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
    <div class="item">5</div>
    <div class="item">6</div>
    <div class="item">7</div>
    <div class="item">8</div>
    <div class="item">9</div>
    <div class="item">10</div>
</div>

<style>
    .container {
        display: grid;
    }
</style>
```

## explicit vs. implicit tracks
**explicit tracks**
* rows or columns which we defined via css
* the lines of explicit tracks are **dashed** in the firefox devtools *(1 in image)*
* the lines of explicit tracks are **solid**, where the grid does end. *(2 in image)*
* explicit tracks are defined like this (two columns, three rows):

```css
grid-template-columns: 80px 160px;
grid-template-rows: 50px 80px 80px;
```
**implicit tracks**
* rows or columns which we **did not** defined via css
* the lines of explicit tracks are **dotted** in the firefox devtools *(3 in image)*
* implicit tracks are defined like this (every implicit rows will be 60px):

```css
grid-auto-rows: 60px;
```

![](./1.png)

## grid-auto-flow
* By default adding more items than defined via `grid-template-rows` will create new (implicit) **rows**
* With the rule `grid-auto-flow: column;` we allow the grid to create new **columns** instead of new rows
