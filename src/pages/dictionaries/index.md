---
title: 'Dictionaries in Python'
date: '2020-11-21'
spoiler: The complete guide to dictionaries.
---

### Keypoints

- dictionaries are indexed by keys that can be of **any hashable type**
- thus **strings** or **integers** or **tuples** (which contain hashable types in themselves) can be used as the index
- class attributes and variables in a stack frame are both stored internally in dictionaries
- specialized 3rd party based dictionaries exist - **skip lists** or **B-tree** based dictionaries

### Types of Dictionaries

```python
# Types of Dictionaries
import collections
from types import MappingProxyType

# in case the order of items in a dictionary is to be preserved use an ordered dict
d1 = collections.OrderedDict(one=1, two=2, three=3)

# in case a default value has to be set when accessing a key which is not present
# use a default dict.In this case if a key is not present and is trying to be accessed
# it will set the value to an empty list
d2 = collections.defaultdict(list)

# in case multiple dictionaries are present through which a particular key is to be searched
# use a chainmap to chain the existing dictionaries instead of creating a new dictionary
chain = collections.ChainMap(d1,d2)

# in case a readonly dictionary is required which cannot be manipulated by the end user
# but can be read by him we can use a mappingproxytype
writable = {'one': 1, 'two': 2}
read_only = MappingProxyType(writable)
```

```python

# Unique Elements in a dictionary
# when adding entries to a dictionary the keys are checked for equality and hashing equality
# if the value is unique or the hash is unique or both are unique then a new key is created
# and the value is mapped to that key.However in case there is equality and the hash is also
# the same then the key is considered to be duplicate and the value is mapped to the original
# key.
# Also when adding or updating a dictionary in case the value of an old key is being updated
# then only the value against the key is rewritten and not the key itself.This is done to
# conserve computation cost

{True: 'yes', 1: 'no', 1.0: 'maybe'} # => {True: 'maybe'}

class AlwaysEquals:

	def __eq__(self, other):
		return True

	def __hash__(self):
		return 1
	
# since hash(object) calles the __hash__ dunder method and object1 == object2 calls the
# __eq__ method , all the objects created from the above function will be equal and also
# have the same hash.Thus all the objects from this class if used as keys in a dictionary
# will start overriding each other

{AlwaysEquals(): 1, AlwaysEquals():2, AlwaysEquals():3} # => {<__main__.AlwaysEquals at 0x7fbae2eb2668>: 3}

```

### Sorting Dictionaries

```python
# Sorting Dictionaries

xs = { 'a': 4, 'b': 2, 'd': 1, 'c':3}
sorted(xs.items()) # => [('a', 4), ('b', 2), ('c', 3), ('d', 1)] # This will sort by the key
sorted(xs.items(), key=lambda x: x[1], reverse=False) # => [('d', 1), ('b', 2), ('c', 3), ('a', 4)] This will sort by the value rather than the key
```

### Merging 2 dictionaries

```python
# Merging 2 dictionaries into a 3rd dictionary

# dataset to work on
xs = {'a': 1, 'b': 2}
ys = {'b': 3, 'c': 4}
zs = {}

# merge 1
zs.update(xs)
zs.update(ys)

# merge 2
zs = dict(xs, **ys)

# merge 3 for python 3.5+
zs = {**xs, **ys}

# merge 4 for python 3.9+
zs = xs|ys
```