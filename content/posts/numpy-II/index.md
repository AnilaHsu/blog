---
title: "Numpy | Array & Matrix Operations"
description: ""
date: 2021-07-24
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

In Numpy, array is used to represent general-purpose N-dimensional arrays, and matrix is used specifically for linear algebra calculations. Both two-dimensional array and matrix can be used to represent matrices, but there are some differences between the two when multiplication is performed.
  <!--more-->
The operation of Array is to operate on each element, and the operation of Matrix is to operate on the matrix.

## Constant multiplication of List and Numpy array

We can see that when a list performs a constant multiplication operation, the original list is repeatedly concatenated by N times. To perform individual operations on each element, a for loop iteration needs to be used, because the list has the characteristics of heterogeneous data types. And ndarray will directly multiply each element by N multiples during the multiplication of constants, which is the performance of ndarray homogeneous data type.

```python
list_1 = [[1, 2], [3, 4]]
x = np.array(list_1)

print(list_1*3)
print('\n')
print()
print('\n')
print(x*3)

```

```python
# output
# print(list_1*3)
[[1, 2], [3, 4], [1, 2], [3, 4], [1, 2], [3, 4]]

# print(x) 
[[1 2]
 [3 4]]

# print(x*3)
 [[ 3  6]
 [ 9 12]]
```
[]()

## Quantity product (dot product) and vector product (cross product) of Numpy array

We can see that the result of Numpy's array operation using * is the product of quantities, while the result of operation using the function dot() is the product of vectors:

```python

list_1 = [[1, 2], [3, 4]]
list_2 = [[5, 6], [7, 8]]
x = np.array(list_1)
y = np.array(list_2)

print('x * y =',x*y)
print('\n')
print('np.dot(x, y) =',np.dot(x,y))
```

```python
# output
# array using *
x * y =
 [[ 5 12]
 [21 32]]

# array using method dot()
np.dot(x, y) =
 [[19 22]
 [43 50]]
```

[]()

## Transpose of Numpy array

When transposing a one-dimensional array through Numpy array, the result is the same:

```python
z = np.array([1, 2, 3, 4])

print(z)
print(z.T)
```

```python
# output
# print(z)
[1 2 3 4]
# print(z.T)
[1 2 3 4]
```
[]()

## Multiplication of List and Numpy matrix
Similarly, when a matrix performs a constant multiplication operation, each element is multiplied by a multiple of the constant:

```python
list_1 = [[1, 2], [3, 4]]
x = np.matrix(list_1)

print(x)
print('\n')
print(x*3)
```

```python
# output
# print(x)
[[1 2]
 [3 4]]

# print(x*3)
[[ 3  6]
 [ 9 12]]
```

[]()

## Quantity product (dot product) and vector product (cross product) of Numpy matrix
 
Unlike arrays, when a matrix is operated on with *, the result is a vector product, while a multiply() operation results in a quantitative product:

```python
list_1 = [[1, 2], [3, 4]]
list_2 = [[5, 6], [7, 8]]
x = np.matrix(list_1)
y = np.matrix(list_2)

print('x * y =\n',x*y)
print('\n')
print('np.dot(x, y) =\n',np.dot(x, y))
```

```python
# output
# matrix using * 
x * y =
 [[19 22]
 [43 50]]

# matrix using multiply()
np.multiply(x, y) = 
 [[ 5 12]
 [21 32]]
```


## transpose of Numpy matrix

When transposing a one-dimensional array through Numpy array, the result is not the same as the transposing result of array:

```python
list_3 = [1, 2, 3, 4]
z = np.matrix(list_3)

print(z)
print(z.T)
```

```python
# output
# print(z)
[[1, 2, 3, 4]]
# print(z.T)
[[1]
 [2]
 [3]
 [4]]
```