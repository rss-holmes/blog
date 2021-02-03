---
title: 'Nested/Inner functions in python'
date: '2020-11-13'
spoiler: The function within the function.
---


A function which is defined within another function is called a nested/inner function

- nested functions are able to access variables from the enclosing function scope

- variables declared within the nested function is not accessible from anywhere outside

- the nested function can only be accessed within the scope of the outer function and not anywhere else

[**python**]

def outerFunction(*text*):

text = text

def innerFunction():

print(text)

innerFunction()

*if* __name__ == '__main__':

outerFunction('Hey!')

innerFunction() *#=> this will throw an error as the innerfunction is not defined in the global scope*

[**end**]

- Inner functions are used so that they can be protected from everything happening outside the function