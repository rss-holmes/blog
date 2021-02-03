---
title: 'Classes and OOPS in python'
date: '2020-11-13'
spoiler: How to go object oriented in python.
---

## Dunder Methods

```python
class my_class(object):

	def __init__(self):
		pass
	
	def __repr__(self):
		# this returns a string which can be used for showing the string representation
		# of the class in debuggers or system outputs.
		# the repr function is also used as a fallback for when the str function is not
		# present
	
		return self.__class__.__name__
	
	def __str__(self):
		# this returns a string which can be used for showing the class when in is printed*
		# anywhere using print statement*
	
	def __unicode__(self):
		# used in python 2.7 instead of the str class to return a unicode string representation*
		# should be implemented instead of str in python2.x as the str method returns bytes*
	
```

## Custom Exception Methods ****

- should be done to make the error handling more readable
- all errors(existing and custom defined) inherit from the Exception class

```python
class CustomError(Exception):

	def __init__(self, *args):
		self.custom_msg = args[0]
		
	def __str_(self):
		print("Calling custom error")
		print("Message sent by user :".format(self.custom_msg))
	
class ProfileDoesNotExist(APIException):

	# here APIException ships with drf and is the base rest exception
	status_code = 400
	default_detail = 'The requested profile does not exist.'

	# to raise an exception
	raise CustomError
```

## **Cloning objects**

- pythons built in collections can be copied by using the respective factory functions in python
- however this created shallow copies , meaning that the copies are only one level deep

```python
a = [(1,2,3),(4,5,6),(7,8,9),(10,11,12),(13,14,15)]
b = list(a) #(this will make a copy of a and point b to it)
c = a #(this will point c to a)
a.append((16,17,18)) #(this doesnt change b but changes c)
a[0] =
```

## **All about Classes**

- when a class variable on an instance is overriden, it creates an instance variable with the same name as the class variable which shadows the instance variable

```python
## ======================================================================================================== ##

# Class variables vs Instance variables

class CountedObject:

	num_instances = 0

	def __init__(self):
		self.__class__.num_instances
	
	def __init__(self):
	
		# this is a wrong implementation as it would make and update a
		# shadow variable num_instances which will ge linked to the instance
		self.num_instances += 1

CountedObject.num_instances # => 0 this will access the class variable
CountedObject().num_instances # => 1 this will access the instance variable
CountedObject().num_instances # => 2
CountedObject().num_instances # => 3
CountedObject.num_instances # => 3

## ======================================================================================================== ##

# Class method vs Static method vs normal methods

class MyClass:

	def method(self):
	
		# can access and change the instance state
		# can also access and change the class state
		# through self.__class__
		
		return 'instance method called', self
	
	@classmethod
	def classmethod(cls):
	
		# since this method only has access to the cls parameter, it can only
		# access and change the class state and not the instance state
		# class methods are often used as factory methods to create different
		# types of instances of the same class.It is also used for defining
		# alternate constructors.

		return 'class method called', cls
	
	@staticmethod
	def staticmethod():
	
		# since this method doesnt have access to either the cls or the self
		# parameters , it can access neither the class or the instance state
		# these methods are mainly used to namespace the method or provide
		# functionality which will be universal across classes or instances
		# these methods are generally used as a helper method inside another
		# instance or class method
		
		return 'static method called'

MyClass.classmethod() # => gives the output of the classmethod
MyClass.staticmethod() # => gives the output of the staticmethod
MyClass.method() # => throws an error as the instance hasnt been created

obj = MyClass()

obj.method() # => gives the output of the method
MyClass.method(obj) # => gives the output of the method
obj.classmethod() # => gives the output of the classmethod
obj.staticmethod() # => gives the output of the staticmethod

## ======================================================================================================== ##

# Class Getters and Setters
# all properties and functions of a class can be found in the __dict__ dictionary of the class
# man.temperature becomes man.__dict__['temperature']

class Celsius:

	def __init__(self, initTemp = 0):
		self._temperature = initTemp
		def to_fahrenheit(self):
		return (self.temperature * 1.8) + 32
	
	@property
	def temperature(*self*):
		print("Getting value")
		return self._temperature
	
	@temperature.setter
	def temperature(*self*, *value*):
	
		if value < -273:
			raise ValueError("Temperature below -273 is not possible")
		
		print("Setting value")
		self._temperature = value

# Method 2

# temperature = property()
# temperature = temperature.getter(get_temperature)
# temperature = temperature.setter(set_temperature)

# Method 3

# temperature = property(get_temperature,set_temperature)

# The attribute temperature is a property object which
# provides interface to the private variable _temperature
# The getter setter replaces the lookup into the __dict__ dictionary for the specific properties
# Even after this the _temperature variable is still accessible from the class instance
# Thus if anyone directly changes it then the data will be mutated directly ie not through
# the getters and setters.
# The private variable needs to be different from the property object name otherwise the
# instantiation goes into a recursive loop
```