---
title: "Python - Important Concepts of Function"
description: ""
date: 2021-07-10
draft: false
categories: 
- Python Note
tags:
- Python Function
cover:
    image: "photo/cover.jpeg"
    relative: true
---

## Basic understanding of the function

A function is a subroutine in a program. The concept is somewhat similar to a mathematical function, which is responsible for handling a function in the code.

<!--more-->

For example, when we are learning Python, the most commonly used `print()` is actually a function. `print()` is a built-in function in Python, and we can also define a function by ourselves to create a program or function that we want to execute.

In this way, it is convenient for us to execute this function to process when we need this program, which can not only reduce repeated code, but also make our program look more structured. If we want to modify the code later, it will compare convenient.

## Custom function

So how do we customize a function, we can first take a look at the structure of the function:

```python
def function_name(parameter 1, parameter 2,...parameter n):
    function content
    return value
```

In the function, we use `def` at the beginning, followed by the function name, and return the result of the program execution with the `return`, we call the result the return value.


In a function, the parameters are equivalent to the function's **input**, and the return value is the function's **output**.
The act of executing a function is called **function call**. When calling a function, we can execute the function by writing the function name, and if there are parameters, we need to give parameters. 


```python
def calc_add(x,y):
	return x + y		

calc_add(4,9)
```

```python
# output
9
```

## The variable scope of the function


In a program, variables all have a valid range. According to the location of the variable, it will affect the value accessed. It can be divided into **global variable** and **regional variable**.

In Python, global variables can be accessed as long as they are in the same Python file, while local variables are values that can only be accessed within the scope of the function, and cannot be accessed outside the function.

You can refer to the following example, the x value of the local variable is printed within the scope of the function, and the x value of the global variable is printed outside the function:

```python
# global variable
x = 100

def calc_num():
	# local variable
	x = 20
	return x

print(calc_num())    
print(x)  

```

```python
# output     
20           # print out the local variable x = 20
100          # print out the global variable x = 100
```

If you want to get the value of a global variable through a function, you can use **global** in the function, refer to the example:

```python
# global variable
x = 100

def calc_num():
	global x
	return x

print(calc_num())    
print(x)  
```

```python
# output     
100          # print out the global variable x = 100
100          # print out the global variable x = 100
```

If you want to modify the value of the global variable through a function, you can also use **global** in the function and modify it, and it will be modified together with the global variable. Refer to the example:

```python
# global variable
x = 100

def calc_num():
	global x
	# local variable
	x = 20
	return x

print(calc_num())    
print(x)  
```

```python
# output     
20          # # print out the local variable x = 20
20          # # print out the local variable x = 20
```

> Avoid modifying the value of the global variable in the function when it is not necessary, because you will never know whether the global variable is used in other places in the program to perform operations, and after modifying its value in the function, it is easy to cause a **Side Effect** or **Bug** of the program.

## Function return value

The function needs a return value. If no return is added at the end of the function, it will only execute the contents of the function and return None.
example:

```python
def calc_num():
	x = 20

print(calc_num())    
```

```python
# output     
None         # because there is no return value
```

## Function parameter

There are three main types of parameters for functions, namely Default parameters, Keyword parameters, and Arbitrary parameters.
Generally speaking, when we call functions, we usually pass values to function parameters as references, as follows:

```python
def say_hello(name):
    print('Hello '+ name)
    
say_hello('Anila')
```

```python
# output
Hello Anila
```


### 1. Default parameters

If no value is passed when calling the function, we can provide the function reference through the default parameter, which is to give the function default value. In the case of no value passed, the function will be executed according to the default value.

```python
def say_hello(name = 'Andy'):
    print('Hello '+ name);
    
sayhello()
```

```python
# output
Hello Andy
```

### 2. Keyword arguments

Sometimes, the functions we define want to force some parameters to be passed using keyword parameters. Through the keyword parameters, we can specify the parameter name with the value we want to pass in, so that we can also input the value of the parameter without order.

```python
def say_hello(name,age):
    print('Hello '+ name + ' you are '+ age + ' years olds.');

say_hello(age = '18', name='Anila')
```

```python
# output
Hello Anila you are 18 years olds.
```

### 3. Arbitrary arguments (`*args` and `**kwargs`)

Sometimes, when designing a function, we are not sure about the number of parameters to be passed into the function. When inputting multiple parameters of an indeterminate number, we can add `*` before the parameter name to pass in at one time. Multiple arguments, and the result is output as a tuple.

```python
def info(*temp):
		print(temp)

info('Toby','Andy','Anila')
```

```python
#output
('Toby','Andy','Anila')
```

We can also use `**` to convert parameter data into Dictionary type.

```python
def info(**temp):
		print(temp)

foo(name = 'Toby',age= 2)
```

```python
# output
{'name':'Toby', 'age':2}
```

### 4. The concept of function passing by value

The transfer of Python values is mainly based on the method of pass by reference, that is to say, the reference of objects is passed, and some data types of these passed reference objects are mutable, and some data types are immutable.

Variable data types such as **list**, **array**. Immutable data types such as **int**, **string**, etc.,

When the variable data type and immutable data type pass the value to the function for simple operation, the original reference value will not be changed, but if the variable data type is passed to the function, its own value is used. method to change the value of itself, it will be changed along with the value of the original reference.

The immutable data type int does not change the original reference value when passing a value to a function to perform a simple operation. Refer to the example:

```python
x = 100
def calc_num(x):
		result = x*2
		return result

print(calculate_number(x))
print(x)
```

```python
# output
200        # print the variable x = 200
100        # print the original variable x = 100
```

The mutable data type list passes a value to a function to perform a simple operation without changing the original reference value. Reference example:

```python
x = [1,2,3,4]
def calc_num(x):
		result = x*2
		return result

print(calculate_number(x))
print(x)
```

```python
# output
[1, 2, 3, 4, 1, 2, 3, 4]
[1, 2, 3, 4]
```

The mutable data type list passed a value to a function using its own method (changing itself directly) will change the value of the original reference:

```python
x = [1,2,3,4]
def calc_num(x):
    x.append(5)
    return x
    
print(calc_num(x))
print(x)
```

```python
# output
[1,2,3,4,5]
[1,2,3,4,5]

```
＊Note: `append()` does not return a value, but directly changes the list of x


### 5. Function without parameters

If no reference value is given to the variable in the function, the function will take the value according to the nearest hierarchy:

```python
x = 100
def calc_num():
		result = x*2
		return result

print(calc_num())
print(x)
```

```python
# output
200
100
```

```python
x = [1,2,3,4]
def calc_num():
    x.append(5)
    return x
    
print(calc_num())
print(x)
```

```python
# output 
[1,2,3,4,5]
[1,2,3,4,5]
```

＊Note: When using a function, try to avoid confusion with repeated variable names inside the function and outside the function.

## Anonymous function

Finally, let's introduce the anonymous function in the function. The so-called anonymous function does not need to define the name of the function, and the content of the function is written directly in the place to be executed.

In Python, **lambda** is used to create an anonymous function, and the parameter name and execution content are followed by the lambda, and the value of the parameter can be given to execute to execute the content of the function. Refer to the example:

```python
sum = (lambda x,y:x+y)
print(sum((4,5)))

# or

print((lambda x,y:x+y)(4,5))
```

```python
# output
9
9
```

In this example, `lambda x, y:` is equivalent to `def function_name(x, y)`, and the return `x+y`, which is the basis of the inverse function spelling.
There are a few things to pay attention to in anonymous functions:

- lambda is just an expression, not a block of code, so it can only encapsulate limited logic in it.

- A lambda function has its own namespace and cannot access parameters outside its own parameter list or in the global namespace.

## Conclusion

Functions are very useful functions in program writing, especially when writing more complex projects. It not only has greater flexibility to adjust the program, but also can understand the structure of the actual project more clearly.

 Hope this article can help you who are learning programming!