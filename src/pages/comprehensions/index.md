---
title: 'Comprehensions in python'
date: '2020-11-19'
spoiler: How to quickly work with lists and dictionaries.
---

# Comprehensions

```python
# simple comprehensions
[expression for item in collection if condition]

# list comprehension
[x for x in range (0,3)]

# set comprehension
{ x for x in range (0,3) }

# dictionary comprehension
{ x: x*x for x in range (0,3) }
```