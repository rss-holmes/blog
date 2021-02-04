---
title: 'Named Tuples in Python'
date: '2020-11-28'
spoiler: Memory efficient data storage.
---

- named tuples are memory efficient shortcut to defining an immutable class in python manually
- namedtuples can be subclassed and even new methods can be added to it
- can be used to bring in structure to the code.Instead of passing around dictionaries which are too small in size to write into a full blown class , a namedtuple can be created

```python
from collections import namedtuple

Car = namedtuple('Car', ['color', 'mileage'])
Car = namedtuple('Car', 'color, mileage')

my_car = Car('red', 34.33)
my_car = Car(color='red', mileage=34.33)

# this will create an object of type car which will have the 2 attributes

my_car.color # => red
my_car.mileage # => 34.33
my_car[0] # => red
my_car[1] # => 34.33
color , mileage = my_car

print(my_car)

# subclassing named tuples to increase functionality
# to add new methods in the subclass

class MyCarWithMethods(Car):

	def hexcolor(self):
	
		if self.color == 'red':
			return '#ff0000'
		else:
			return '#000000'

# to add new fields in the subclass

ElectricCar = namedtuple('ElectricCar', Car._fields + ('charge',))

# helper functions

my_car._asdict() # => OrderedDict([('color', 'red'), ('mileage', 3812.4)])
json.dumps(my_car._asdict()) # => '{"color": "red", "mileage": 3812.4}'
my_car._replace(*color*='blue') # => creates a shallow copy of the var with replaced field
Car._make(['red', 999]) # => make a new variable of the instance
```