---
title: 'Closures in python'
date: '2020-11-19'
spoiler: Do functions have a memory ?
---

# Closures

A Closure is a function object that remembers values in enclosing scopes even if they are not present in memory.

It is a record that stores a function together with an environment: a mapping associating each free variable of the function

(variables that are used locally, but defined in an enclosing scope) with the value or reference to which the name was bound

when the closure was created.A closure—unlike a plain function—allows the function to access those captured variables through

the closure’s copies of their values or references, even when the function is invoked outside their scope.

[**python**]

def outerFunction(*text*):

text = text

def innerFunction():

print(text)

*# now we need to return a closure object instead of calling the innerfunction directly*

*return* innerFunction

*if* __name__ == '__main__':

myFunction = outerFunction('Hey!') *# since the outerfunction returns a closure object which is kinda like a function we need to call the closure*

myFunction() *# thus we separated the creation and calling of the closure object*

[**end**]

- closures are generally used as callback functions and provide some level of data hiding(encapsulation)

- it helps prevent the declaration of a lot of global variables

- in case of fewer number of functions closures can be used to achieve encapsulation but in case of a larger

number of functions classed should be used to achieve the same result

- classes and closures both help in encapsulation and reducing global variables

- closures is just a simple extension of the inner/nested function concept wherein we