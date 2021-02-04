---
title: 'Record Structs and Data transfer objects in Python'
date: '2020-11-29'
spoiler: Efficient way to transfer data in python.
---

```python
from typing import NamedTuple

# there is another named tuple type present in typing which has a different syntax
# and a support for type checking of the property variables

class Car(NamedTuple):
	color : str
	mileage: float
	automatic: bool

car1 = Car(color='red', mileage=3812.4, automatic=True)

# serializes structs are intended primarily as a data exchange format rather than holding
# data in memory. In some cases, packing primitive data into structs may use less memory
# than keeping it in other data types.It can be used to handle binary data stored in files or
# coming/sending in from network connections

from struct import Struct

MyStruct = Struct('i?f')
data = MyStruct.pack(23, False, 42.0) #=> b'x17x00x00x00x00x00x00x00x00x00(B'
MyStruct.unpack(data) #=> (23, False, 42.0)
```