![](./0.png)
# c#

### index


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output --> 

* [c#](#c)
	* [value vs. reference types](#value-vs-reference-types)

<!-- /code_chunk_output -->


## value vs. reference types
All C# types fall into these four categories
* **Value types**
* **Reference types**
* Pointer types
* Generic type parameters

### Value Types: 
* bool
* byte
* char
* decimal
* double
* enum
* float
* int
* long
* sbyte
* short
* struct
* uint
* ulong
* ushort

### Reference Types:
* string
* class / object
* array
* delegate
* interfaces
* ...

### Difference
The fundamental difference between the two is how they are handled in memory.
**Value Types**
* The content is a simple value. (eg. content of the value type *int* is 32 bits of data)
* The assignment of a value type always **copies** the instance.
* Occupies precisley the memory required to store their fields

**Reference Types**
* Has two parts: an **object** and the **reference** to that object
* The content is a reference to an object that contains the value
* Can be assigned **null**, indicating that the reference point to no object
* Require separate allocations of memory for the reference and object.