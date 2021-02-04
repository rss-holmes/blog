---
title: 'String Formatting in Python'
date: '2020-12-02'
spoiler: Popular ways to format strings in python.
---

### Old Method

```python
'Hello,%s' % name # => prints the name in a string format
'%x' % errno # => prints errno in a hexadecimal format
'Hey %s, there is a 0x%x error!' % (name, errno) # => prints name in string and errno in hex format
'Hey %(name)s, there is a 0x %(errno)x error!' % {"name": name, "errno": errno } # => dictionary format of sending data
```

### New Method

```python
'Hello, {}'.format(name)
'Hey {name}, there is a 0x{errno:x}error!'.format(name=name, errno=errno)
```

### String Literal Method (from python 3.6)

```python
#  string literal way of string formatting (starting from python 3.6)
name = 'bob'
error = 34
f'Hello, {name}!, theres is an error:{error:x}'
```