---
title: 'Stacks and Queues in Python'
date: '2020-11-30'
spoiler: Some archaic datastructures in python.
---

- supports fast LIFO semantics for inserts and deletes
- doesnt allow for random access to the objects
- takes O(1) time for insertion and deletion
- are used for depth first search on a tree or graph data structure

```python
# lists can be a decent stack structure if used only for insertion and deletion
# from one end
# python lists are implemented as dynamic arrays internally and thus might be
# required to resize its storage space when elements are added or deleted
# the list overallocated so that not every push or pop needs resizing giving an amortized O(1) performance
# on the otherside because of this it gives an O(1) complexity random access
# to get the O(1) performance elements have to be deleted and added to the end of a list
# using append and pop.If we delete from the start or middle then elements will be shifted
# leading to O(n) complexity

s = []
s.append('eat') # => {'eat'}
s.append('sleep') # => {'eat','sleep'}
s.append('code') # => {'eat','sleep','code'}
s.pop() # => code
s.pop() # => sleep
s.pop() # => eat
s.pop() # error

# collections.deque is another implementation in python which can be used as stacks
# it is a double-ended queue which supports adding and removing elements from both
# ends in O(1) complexity
# they can serve as both stacks and queues
# is implemented as a doubly linked list and thus gives O(1) complexity for adding and
# removing objects but gives O(n) complexity for accessing random elements in the middle

from collections import deque

s = deque()
s.append('eat') # => {'eat'}
s.append('sleep') # => {'eat','sleep'}
s.append('code') # => {'eat','sleep','code'}
s.pop() # => code
s.pop() # => sleep
s.pop() # => eat
s.pop() # error

# queue.LifoQueue provides a stack structure which can be used for parallel computing
# provides locking semantics which supports multiple producers and consumers
# should be used in case of parallel computing as normal use may give unneeded overhead

from queue import LifoQueue

s = LifoQueue()
s.append('eat') # => {'eat'}
s.append('sleep') # => {'eat','sleep'}
s.append('code') # => {'eat','sleep','code'}
s.pop() # => code
s.pop() # => sleep
s.pop() # => eat
s.pop() # error
```