---
title: 'Dunder/Underscore Methods in Python'
date: '2020-11-22'
spoiler: Sprinkle some magic in your functions.
---

Underscores in python have a lot of significance when used in conjunction with variables or functions.

**Single** underscore **before** a **variable name** (Pseudo private variables)

- as a general convention variables with a leading underscore are **considered to be private**.
- an underscore before a variable has no pythonic significance.The python interpreter will treat it **exactly similar** to one without a leading underscore.
- variables with a leading underscore can still be directly accessed and mutated **outside the class scope**

```python
class Test:

    def __init__(self):
        self.foo = 11
        self._bar = 23
```

---

**Single** underscore **before** a **function name** (Pseudo private function)

- as a general convention variables with a leading underscore are **considered to be private.**
- an underscore before a variable has no pythonic significance.The python interpreter will treat it **exactly similar** to one without a leading underscore.
- functions with a leading underscore can still be directly accessed and mutated **outside the class scope**.
- another quirk of such functions are that these functions are not imported when importing from a module using *

```python
class Test:

		def _some_private_function(a,b):
				# do something
```

---

**Single** underscore **after** a **variable name**(Reusing reserved keywords)

- an underscore after a variable is ofter used to reuse a reserved keyword(ex- class, object) which python doesnt allow to use normally.

```python
def make_object(name, class_):
		# here we use an underscore to create a class_ named variable
		# which comes pretty close to the class keyword 
    pass
```

---

**Double** underscore **before** a **variable name** (Name mangling)

- **Name Mangling** is a feature provided by the python interpreter wherein the interpreter **changes the name** of the variable in a way that makes it **harder to create collisions** when the class is extended later.
- putting double underscore before a class variable directs the python interpreter to mangle the name of the variable to prevent name collisions in the future.

Let us take an example to understand this -

1. Lets declare a class called **Test** with different kinds of variables

    ```python
    class Test:
        __init__(self):
            self.foo = 11
            self._bar = 23
            self.__baz = 23

    # in this example if you print out all the variables of the class(using dir(Test()))
    # then foo and _bar will remain unchanged however the __baz variable will be renamed 
    # to prevent name mangling for when the class is inherited mostly __baz becomes _Test__baz
    ```

    If you print out the variables of this class then foo and _bar would remain unchanged however __baz would be mangled by the interpreter to something like _Test__baz

2. Lets extend this class to another class called **ExtendedTest**

    ```python
    class ExtendedTest(Test):
        def __init__(self):
        super().__init__()
        self.foo = 'overridden'
        self._bar = 'overridden'
        self.__baz = 'overridden'

    # in this example also if you try to access the __baz attribute in an instance of 
    # ExtendedTest class it will give you an attribute error as it will be mangled as
    # _Extended_Test__baz
    ```

    If you try to access the __baz variable from an instance of the ExtendedTest class then you will get an error since it would be name mangled to _Extended_Test__baz.

3. To access mangled variables we need to use **private functions**

    ```python
    class ManglingTest:

        __init__(self):
            self.__mangled = 'hello'

        def get_mangled(self):
            return self.__mangled

    ManglingTest().get_mangled() # => 'hello'
    # using a private function mangled functions or attributes can be accessed
    ```

---

**Double** underscore **before and after** a **function name** (Magic Methods)

- in python there are some very special functions wherein the name starts and ends with double underscore.These are calles dunder/magic/special methods
- dunder methods allow us to emulate the behavior of built-in types.These help us extend the functionality of custom classes.Some common dunder methods are -
    - `__init__` is the most famous dunder method which is called when a class is instantiated.
    - `__add__` is called when we use the arithmetic operator + between 2 objects of a class.
    - `__repr__` is called when we try to get the string representation of an object of a class.
    - `__call__` is used to make an object of a class callable.

```python

class CustomClass : 
      
    def __init__(self, string): 
        self.string = string  
           
    def __repr__(self): 
        return 'Object: {}'.format(self.string) 
          
    def __add__(self, other): 
        return self.string + other

# Driver Code 
if __name__ == '__main__': 
      
    # object creation will use the __init__ magic method
    string1 = CustomClass('Hi') 
      
    # concatenate String object and a string, this will use __add__ magic method for concatenating the strings
    print(string1 + ' Gods of Code')
```