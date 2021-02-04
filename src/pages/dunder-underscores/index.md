---
title: 'Dunder/Underscore Methods in Python'
date: '2020-11-22'
spoiler: Sprinkle some magic in your functions.
---

### Single Underscores before variables or function names (Pseudo private variables)

A common convention is to put **a single** **underscore before functions** **or variables** which are  **considered to be private** inside a class.However this has **no pythonic significance** and the variables can still be directly accessed and mutated outside the class scope.

```python
class Test:

    def __init__(self):
        self.foo = 11
        self._bar = 23

		def _some_private_function(a,b):
				# do something

# The underscore in _bar variable and the  _some_private_function method has no particular pythonic significance.
# It is just used to signify that it is a private variable or function
```

### Single underscores after variables (Reusing reserved keywords)

A common usecase for putting **a single underscore after a variable name** is for setting the name to a **reserved keyword** in python.This helps **reuse reserved keywords (like class, object)** to name our variables if it suits our scenario.

```python
def make_object(name, class_):
		# here we use an underscore to create a class_ named variable which comes pretty close to the class keyword 
    pass

# a trailing underscore is often attached to a variable to bypass the python reserved words namings
# as class is a reserved word in python the nearest we can name a variable is class_
```

### Double underscores before variables or function names (Name Mangling)

```python
class Test:
    __init__(self):
        self.foo = 11
        self._bar = 23
        self.__baz = 23

# in this example if you print out all the variables of the class(using dir(Test()))
# then foo and _bar will remain unchanged however the __baz variable will be renamed 
# to prevent name mangling for when the class is inherited mostly __baz becomes _Test__baz

class ExtendedTest(Test):
    def __init__(self):
    super().__init__()
    self.foo = 'overridden'
    self._bar = 'overridden'
    self.__baz = 'overridden'

# in this example also if you try to access the __baz attribute in an instance of 
# ExtendedTest class it will give you an attribute error as it will be mangled as
# _Extended_Test__baz

class ManglingTest:

    __init__(self):
        self.__mangled = 'hello'

    def get_mangled(self):
        return self.__mangled

ManglingTest().get_mangled() # => 'hello'
# using a private function mangled functions or attributes can be accessed
```

### Double underscores before and after  function names (Magic Methods)

```python

################################################

# my_module.py

def external_func():
    return 23

def _internal_func():
    return 42

# when a function inside a module is declared as starting with _ 
# it is not imported when importing from the module using *
# ie from module import * wont pull in _internal_func

################################################

class Test:
    __init__(self):
        self.foo = 11
        self._bar = 23
        self.__baz = 23

# in this example if you print out all the variables of the class(using dir(Test()))
# then foo and _bar will remain unchanged however the __baz variable will be renamed 
# to prevent name mangling for when the class is inherited mostly __baz becomes _Test__baz

class ExtendedTest(Test):
    def __init__(self):
    super().__init__()
    self.foo = 'overridden'
    self._bar = 'overridden'
    self.__baz = 'overridden'

# in this example also if you try to access the __baz attribute in an instance of 
# ExtendedTest class it will give you an attribute error as it will be mangled as
# _Extended_Test__baz

# name mangling applies to any variable or function be it inside a class or
# declared globally

class ManglingTest:

    __init__(self):
        self.__mangled = 'hello'

    def get_mangled(self):
        return self.__mangled

ManglingTest().get_mangled() # => 'hello'
# using a private function mangled functions or attributes can be accessed

# *************Name mangling is not applied when the function or attribute 
# starts and ends with __ ie like __foo__ because they are reserved in python
# for special cases
```