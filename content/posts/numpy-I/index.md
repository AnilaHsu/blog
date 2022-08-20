---
title: "NumPy - How to create and use NumPy"
description: ""
date: 2021-07-13
draft: false
categories: 
- Data Analytics
- Python
tags:
- Python Numpy
cover:
    image: "cover.jpeg"
    relative: true
---

## Foreword

In data science, we often need to deal with a large amount of data to process, calculate and analyze the data. In order to efficiently process various data, we often import Python packages for use to reduce Complex code during data processing.

<!--more-->

Frequently used function libraries such as Numpy, Scipy, Pandas, and Matplotlib, these four are very convenient tools for data preprocessing and data visualization. Among them, Numpy is an important foundation for related packages such as Pandas, SciPy, Scikit learn, etc. It will be very helpful for learning other data science related packages in the future.

This article will mainly introduce the basic methods of Numpy. Without further ado, let's get started.

## Knowledge of NumPy

NumPy is the abbreviation of Numerical Python. The bottom layer is implemented in C and Fortran languages. It is a suite of scientific computing and data science. It can not only handle the operations of dimensional arrays and matrices, but also provide a large number of mathematical function libraries for array operations. And the processing speed is very fast, it has the following important features:

- Provides a high-performance multi-dimensional array math library
- Can integrate C/C++ and Fortran code
- Has the property of equivalence data type
- Parallel processing capability to quickly manipulate arrays
- Replacing Python List with NumPy Array

### 1. Install

When using a Python third-party library, we can install Numpy through the command of pip (Python Package Index) in the terminal, or use pip3 if the Python 3.x version is used.

```python
pip3 install numpy
```

### 2. Import

Import Numpy through import, and the np below is a name that can be freely specified, which can be specified using as, and the abbreviation of the package name is conventionally used. 

```python
import numpy as np
```

### 3. Ndarray and List

ndarray is a sequence type very similar to list, in fact ndarray is Numpy's array data type, and list is Python's built-in data type.

The biggest difference between ndarray and list is that list has the characteristic of storing heterogeneous data types. In list, each element is a complete Python object with its own category and information. Therefore, when calculating homogeneous data, it is necessary to Through the for loop iteration, the objects inside are taken out one by one for operation.

Numpy's ndarray has the characteristics of homogeneous data type, that is, every element in the ndarray must have the same data type, so when calculating, the performance and syntax of ndarray are compared to the built-in ones. list is faster and more concise.

## Creation of Ndarray

In Numpy, an ndarray consists of array objects. After importing into Numpy, we can create numeric arrays in two ways. The first way is to use the array() method in Numpy to convert a list to an ndarray, and the second way is to use Numpy's diversification functions to create an ndarray.

### **1. Convert list to ndarray**

- **np.array()**

    We can use np.array() to convert a single-level list into a one-dimensional array of numbers:

    ```python
    list_1=[0, 1, 2, 3, 4, 5]
    data = np.array(list_1)
    print(data)
    print(type(data))
    print(data.dtype)
    ```

    ```python
    # output
    [1 2 3 4 5]               #  data array value
    <class 'numpy.ndarray'>   #  the type of the data array
    int64                     #  The type of the objects in the data array
    ```
    []()

    np.array() can also convert a two-level list into a two-dimensional array of numbers:

    ```python
    list_2=[[0, 1, 2, 3, 4], [5, 6, 7, 8, 9]]
    data = np.array(list_2)
    print(data)
    print(type(data))
    print(data.dtype)
    ```

    ```python
    # output
    [[0 1 2 3 4]              #  data array value
     [5 6 7 8 9]]
    <class 'numpy.ndarray'>   #  the type of the data array
    int64                     #  The type of the objects in the data array
    ```

    > Use print('\n') to print blank columns, that is, to perform a newline.

### 2. Create Ndarray using Numpy diversification functions
In Numpy, many methods are also provided to allow us to quickly create arrays. After importing numpy as np, you can use the various methods provided by Numpy by using the "np. method name".
- **np.zeros()**

   You can use np.zeros() to quickly create an array with all elements 0, such as creating a one-dimensional numeric array with all elements 0, length 5, and type int:

    ```python
    # np.zeros()
    print(np.zeros(5,dtype=int))
    ```

    ```python
    # output
    [0 0 0 0 0]
    ```

- **np.ones()**

    We can also use np.ones() to quickly create an array with all elements of 1, such as creating a two-dimensional numeric array with all elements of 1, length 2x5, and type float:

    ```python
    # np.ones()
    print(np.ones((2, 5),dtype=float)) 
    ```

    ```python
    # output
    [[1. 1. 1. 1. 1.]
     [1. 1. 1. 1. 1.]]
    ```

- **np.full()：**

    In addition to 0 and 1, we can also create an array with all elements as specified values, such as creating a three-dimensional numeric array with all elements specified as 2, length 3x2x5, and type int:

    ```python
    # np.full()
    print(np.full((3, 2, 5), 2))
    ```

    ```python
    # output
    [[[2 2 2 2 2]
      [2 2 2 2 2]]

     [[2 2 2 2 2]
      [2 2 2 2 2]]

     [[2 2 2 2 2]
      [2 2 2 2 2]]]
    ```

- **np.random.random()**

    Or create an array with all random values between 0 and 1, such as creating a two-dimensional array of random values with elements between 0 and 1 and a length of 3x4:

    ```python
    # np.random.random()
    print(np.random.random((3, 4)))
    ```

    ```python
    # output
    [[0.78837773 0.78191722 0.38002785 0.17772392]
     [0.94851325 0.08781078 0.00229052 0.63474405]
     [0.51463031 0.91391908 0.1534682  0.88046589]]
    ```

- **np.ramdom.randint()**

    In addition to floating point numbers, you can also create an array with all elements of random integers in a specified range, such as creating a two-dimensional numeric array with all elements between 0 and 10 and a length of 3x4:

    ```python
    # np.ramdom.randint()
    print(np.random.randint(0, 10, (3, 4)))
    ```

    ```python
    # output
    [[9 7 1 5]
     [5 3 8 2]
     [9 2 9 2]]
    ```

- **np.arange()**

    To create a contiguous array of integers with a specified range, the np.arange() method is often used, such as creating a contiguous array of integers from 0 to 11:

    ```python
    # np.arange()
    print(np.arange(12))
    ```

    ```python
    # output
    [ 0  1  2  3  4  5  6  7  8  9 10 11]

    ```

    Alternatively, you can reshape arrays, such as 3x4 2D numeric arrays, with the reshape() function:

    ```python
    # Combine a 2D array of length 3x4
    print(np.arange(12).reshape(3, 4))
    ```

    ```python
    # output
    [[ 0  1  2  3]
     [ 4  5  6  7]
     [ 8  9 10 11]]
    ```

    > Querying the state of an object and executing the functions (such as functions, methods, properties) of objects separated by periods are the characteristics of object-oriented programming.

## Dimensions and number of elements

In Numpy, we can get the dimension and number of elements of the array through the ndim attribute and the size attribute. With these attributes, we can know the size of the data:

```python
data = np.random.randint(0, 10, (3, 4))
print(data)
print('\n')
print('dimension:',data.ndim)
print('shape:',data.shape)
print('number of elements:',data.size)
```

```python
# output
[[8 7 8 5]
 [8 3 3 9]
 [3 0 1 4]]

dimension: 2
shape: (3, 4)
number of elements: 12
```


## Array of different dimensions

As in the example above, we can create one-dimensional arrays, two-dimensional arrays, and even multi-dimensional arrays according to different needs, and arrays with different dimensions also have their own names, namely **scalar, vector, and matrix. , Tensor**.

### 1. Scalar
A scalar is a numeric value with no dimensions:

```python
scalar = np.array(1234)
print(scalar)
print('dimension:',scalar.ndim)
print('shape:',scalar.shape)
```

```python
# output
1234
dimension:0
shape:()
```

### 2. Vector
A vector refers to a numeric array with one dimension:

```python
vector = np.array([1, 2, 3, 4])
print(vector)
print('dimension:',vector.ndim)
print('shape:',vector.shape)
```

```python
# output
[1 2 3 4]
dimension:1
shape:(4,)
```

### 3. Matrix
A matrix is a numeric array with two dimensions:

```python
matrix = np.array([1, 2, 3, 4]).reshape(2, 2)
print(matrix)
print('dimension:',matrix.ndim)
print('shape:',matrix.shape)
```

```python
# output
[[1 2]
    [3 4]]
dimension:2
shape:(2, 2)
```

### 4. Tensor
Tensors refer to numeric arrays of three dimensions and more:

```python
tensor = np.array([1, 2, 3, 4]*3).reshape(3, 2, 2)
print(tensor)
print('dimension:',tensor.ndim)
print('shape:',tensor.shape)
```

```python
# output
[[[1 2]
    [3 4]]

    [[1 2]
    [3 4]]

    [[1 2]
    [3 4]]]
dimension:3
shape:(3, 2, 2)
```



## Random number seeds

The so-called random number refers to the random value with irregularity. In data analysis, it is mostly used to randomly separate the collected data or add random values to make the data inconsistent.

The result of random numbers may be different in each execution, but when performing data analysis, it is often necessary to verify later. In order to ensure the consistency of the results, we can set the random number seed to make the result of each random number execution. will not change.

The initial value of the random value is called the seed, and we can use np's random.seed() to set the seed:

```python
# seed setting
np.random.seed(0)

# Generate 10 random numbers with a normal distribution (mean 0 standard deviation 1)
print(np.random.randn(10))
```

```python
# output
[ 1.76405235  0.40015721  0.97873798  2.2408932   1.86755799 -0.97727788
  0.95008842 -0.15135721 -0.10321885  0.4105985 ]
```

> Python itself also provides the function of random numbers, but the random number function of Numpy is usually used in data analysis.





Nupmpy also provides many very useful methods and functions. If you want to know and learn more, you can refer to the official documents: [Numpy Documentation](https://numpy.org/) !