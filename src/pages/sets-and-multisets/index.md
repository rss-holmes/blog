---
title: 'Sets and Multisets in Python'
date: '2020-11-29'
spoiler: What are sets in python.
---

- as set is an unordered collection of objects that does not allow duplicate elements
- generally used to check mathematical functionality like union , intersection and inclusion
- membership tests are expected to run in o(1) time .Union , Intersection, Difference and Subset should take o(n) time
- frozenset is an immutable version of set
- frozensets are hashable and thus can be used as dictionary keys
- multisets are a type which allow to have multiple occurences of the same element.Thus it helps in keeping track of whether an element is part of a set and also how many times it is included in the set

```python
vowels = {1,2,3,4,5}
squares = {x*x for x in range(6)} # => {1,4,9,16,25,36}
empty_set = set()
vowels.intersection(squares) # => {1, 4}
vowels.add(6) # => {1,2,3,4,5,6}
len(vowels) # => 6
frozen_vowels = frozenset(vowels)
frozen_vowels.add(8) # => error

# declaring multisets
from collections import Counter

inventory = Counter()
loot = {"sword": 1, "bread": 3,}
more_loot = {"sword": 1, "apple": 3,}
inventory.update(loot) # => {"sword": 1, "bread": 3,}
inventory.update(more_loot) # => {"sword": 2, "bread": 3, "apple": 3}
```