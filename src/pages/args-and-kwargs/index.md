---
title: 'Args and Kwargs in Python'
date: '2020-11-02'
spoiler: Best way to pass variables across functions.
---

- this method is helpful when you dont know how many arguments your function is going to require beforehand

```python
a = list(1,2,3,4,5)
b = tuple(9,8,7,6,5)
c, d, *e = a

print(a) # => prints the list a
print(b) # => prints the tuple b
print(c) # => prints the number 1
print(d) # => prints the number 2
print(e) # => prints (3,4,5)
print(*a) # => prints the numbers 1,2,3,4,5
print(*b) # => prints the numbers 9,8,7,6,5
```