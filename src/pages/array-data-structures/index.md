---
title: 'Array Data Structures in Python'
date: '2020-11-05'
spoiler: How many ways can you maintain list of data.
---


- arrays store information in adjoining blocks of memory and are thus considered contiguous data structures (as opposed to linked data structures like linked lists)
- a proper array implementations guaratntees a constant o(1) access time
- numpy provides a lot of fast array implementations

```python
a1 = [1,'one', 'three'] # mutable multitype array => list
a2 = (1, 'one', 3.0) # immutable multitype array => tuple
a3 = array.array('f', (1.0, 1.5, 2.0)) # mutable single type array => array
a4 = 'abcd' # string is an immutable array of unicode characters => string
a5 = bytes ((0,1,2,3)) # bytes is an immutable array of single bytes => byte
a6 = bytearray ((0,1,2,4)) # bytearray is a mutable array of single bytes => bytearray
```