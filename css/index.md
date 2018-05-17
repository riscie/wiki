![](./0.png)
# css 



<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output -->

* [css](#css)
	* [overflow](#overflow)
	* [overflow ellipsis `...`](#overflow-ellipsis)

<!-- /code_chunk_output -->


## overflow
* the **overflow** property specifies what to do in the case a content is too large to fit the container box. 
* applies to **block, inline-block and table elements**.

### values
* **visible** - content will be rendered outside the box
* **hidden** - content gets clipped, no scrollbars
* **scroll** - content gets clipped with scrollbars if needed
* **auto** - browser determines how to handle. generally scrollbars appear


### syntax
```css
div {
  overflow: hidden;
}
```` 

## overflow ellipsis `...`
* This will show as much text as possible ending with three dots: `'...'`.


```css
div {
    white-space: nowrap; 
    overflow: hidden;
    text-overflow: ellipsis;
}
```
