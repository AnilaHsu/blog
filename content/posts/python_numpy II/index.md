---
title: "Python -  Numpy 的陣列 & 矩陣運算"
description: ""
date: 2021-07-16
draft: false
categories: 
- Data Analytics
tags:
- Python Numpy
cover:
    image: "cover.jpeg"
    relative: true
---


## 前言

在 Numpy 中，array 用於表示通用的 N 維陣列，matrix 則特定用於線性代數計算。二維的 array 和 matrix 都可以用來表示矩陣，但二者在進行乘法運算的時後，有一些不同的地方。
Array 的運算是針對每一個元素進行運算，Matrix 的運算則是對矩陣進行運算。

## 一、List 與 Numpy array 的常數乘法運算

我們可以看到 list 在進行常數乘法運算時，會將原本 list 重複串接 N 倍，若要針對每個元素個別運算需要使用 for 迴圈迭代，因為 list 具備異質資料型態的特性。而 ndarray 在常數的乘法運算時則會直接將各個元素乘上 N 倍數，這是 ndarray 同質資料型態的表現。

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
# 輸出：
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

## 二、Numpy array 的數量積(點乘)與矢量積(叉乘)

我們可以看到 Numpy 的 array 使用 * 進行運算的結果為數量積，而使用函數 dot() 進行運算的結果為矢量積：

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
# 輸出：
# array 使用 *
x * y =
 [[ 5 12]
 [21 32]]

# array 使用函數 dot()
np.dot(x, y) =
 [[19 22]
 [43 50]]
```

[]()

## 三、Numpy array 的轉置

透過 Numpy array 來進行一維陣列的轉置時，結果是一樣的：

```python
z = np.array([1, 2, 3, 4])

print(z)
print(z.T)
```

```python
# 輸出：
# print(z)
[1 2 3 4]
# print(z.T)
[1 2 3 4]
```
[]()

## 四、List 與 Numpy  matrix 的乘法運算
同樣的，matrix 在進行常數乘法運算時，會將各個元素乘上常數的倍數：

```python
list_1 = [[1, 2], [3, 4]]
x = np.matrix(list_1)

print(x)
print('\n')
print(x*3)
```

```python
# 輸出：
# print(x)
[[1 2]
 [3 4]]

# print(x*3)
[[ 3  6]
 [ 9 12]]
```

[]()

## 五、Numpy matrix 的數量積(點乘)與矢量積(叉乘)

與 array 不同的是，在 matrix 使用 * 進行運算時，結果為矢量積，而使用 multiply() 進行運算的結果則為數量積：

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
# 輸出：
# matrix 使用 * 
x * y =
 [[19 22]
 [43 50]]

# matrix 使用 multiply()
np.multiply(x, y) = 
 [[ 5 12]
 [21 32]]
```

[]()

## 六、Numpy matrix 的轉置

透過 Numpy array 來進行一維陣列的轉置時，結果和 array 的轉置結果不相同：

```python
list_3 = [1, 2, 3, 4]
z = np.matrix(list_3)

print(z)
print(z.T)
```

```python
# 輸出：
# print(z)
[[1, 2, 3, 4]]
# print(z.T)
[[1]
 [2]
 [3]
 [4]]
```