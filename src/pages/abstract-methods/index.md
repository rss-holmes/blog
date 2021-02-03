---
title: 'Abstract Methods in Python'
date: '2020-10-20'
spoiler: Handling Abstract Methods in Python.
---

# Abstract Methods

```python
from abc import ABCMeta, abstractmethod

class Base(metaclass=ABCMeta):

	@abstractmethod
	def foo(self):
		pass
	
	@abstractmethod
	def bar(self):
		pass

class concrete(Base):

	def foo(self):
		pass

# this concrete class definition will produce errors at the time of instantion because
# it hasnt implemented the bar method which is declared as an abstract method
# in the Base class.Also the Base class cannot be instantiated since it is an Abstract class
```