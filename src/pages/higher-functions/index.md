---
title: 'Higher Functions in Python'
date: '2020-11-25'
spoiler: Bow to the higher order.
---

- functions are first class objects
- meaning of first class objects , the following can be done
- assign to variables
- stored in datastructures
- passed as arguments to other functions
- can be declared inside other functions
- returned from other functions
- lambdas are single line functions with just the return statement

```python
def yell(text):
    return text.upper() + '!'

# what the above statement does is essentially create a function name yell and then assign it to a variable named yell.
yell.__name__ # this will return yell which is the name of the function

bark = yell
bark.__name__ # this will return yell which is the name of the function the variable points to

del yell 

bark('tom') # both bark and yell store references to the function and thus even if yell is deleted , bark works

function_dict = {bark, yell}
for func in function_dict:
    print(func("woof"))

def greet(func):
    greeting = func('Hi, I am a Python program')
    print(greeting)

# functions that can accept functions as arguments are called higher order functions
# this ability helps us abstract behaviour into objects and pass them around
# the inbuilt map function is a classic higher order function

def speak(text):
    def whisper(t):
        return t.lower() + '...'
    return whisper(text)

# inner or nested functions give the ability to scope functions within another function
# here whisper does not exist outside the speak function

def get_speak_func(volume):

    def whisper(text):
        return text.lower() + '...'

    def yell(text):
        return text.upper() + '!'

    if volume > 0.5:
        return yell
    else:
        return whisper

speak_func = get_speak_func(0.7)
speak_func('Hello')

# functions can not only accept behaviours but they can also return behaviours based upon conditions

def make_adder(n):
    def add(x):
        return x + n
    return add

# when a variable of the outer function defines the functionality in the inner function
# it is said to enclose lexical scope and is this called a lexical closure or closure

class Adder:

    def __init__(self, n):
        self.n = n

    def __call__(self, x):
        return self.n + x

add_1 = Adder(3)
add_1(4) # => this will trigger the call method and return 7

# the same functionality expressed as a class
# the class needs to implement a __call__ method for it to be a callable
# the call function is called when the class instance is called

add = lambda x, y: x + y

# lambda functions are anonymous and thus can be used without binding to a variable
# thus it can be constructed and called in the same line

(lambda x, y: x + y)(5,3) # => this will give 8
```