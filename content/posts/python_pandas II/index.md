---
title: "A useful way to access DataFrame data with Pandas"
description: ""
date: 2021-08-18
draft: false
categories: 
- Data Analytics
tags:
- Python Pandas
cover:
    image: "cover.jpeg"
    relative: true
---

## Foreword

This article will introduce the value method of DataFrame in Pandas, which is mainly divided into three parts:

- Basic data selection
- Methods of `DataFrame.loc`
- Methods of `DataFrame.iloc`
 
 <!--more-->
In the `.loc` and `.iloc` methods described below, the data selection rule is  
**first row and then column, and separate row and column data with commas.**

For example `[row, column]`, so when fetching data Pay special attention. So without further ado, let's see how these methods work ~~!

## Basic data selection

In the data structure of DataFrame, we can find a certain piece of qualified data by directly passing index and column. Then let's take df_student_index as an example to see how to filter and extract data in practice.

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94],'English':[70, 88, 67, 89, 97]}
df_student_index = pd.DataFrame(dict_student,index=['a','b','c','d','e'])
print(df_student_index)
```

```python
# print(df_student_index)
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
b  1100102   m       70    75       88
c  1100103   f       77    74       67
d  1100104   f       69    88       89
e  1100105   m       70    94       97
```

### 1. get the data of a specific column

Take out for column, we can specify the name of the **column** after the data, and use `.column name`, for example, to extract data whose field is English:

```python
# Get data as Series
print(df_student_index.English) 
```

```python
# output
a    70
b    88
c    67
d    89
e    97
Name: English, dtype: int64
```

We can also use Python's **list form** to retrieve single column data and specify the row name in the list.
For example, take out the data whose field is Chinese:

```python
# method 1
# Get data as Series 
print(df_student_index['Chinese'])   
# method 2
# Get data as DataFrame
print(df_student_index[['Chinese']]) 
```

```python
# output
# method 1
a    60
b    70
c    77
d    69
e    70
Name: Chinese, dtype: int64
# method 2
   Chinese
a       60
b       70
c       77
d       69
e       70
```

It can be found that when the data is enclosed in single-layer brackets, a series data type will be taken out; if it is enclosed in double-layer brackets, a DataFrame data structure will be taken out.

When we want to obtain the data of multiple columns, we also specify the row name in **serial form**, such as taking out the data whose fields are Chinese and English:

```python
# Get data as DataFrame
print(df_student_index[['Chinese', 'English']]) 
```

```python
# output
   Chinese  English
a       60       70
b       70       88
c       77       67
d       69       89
e       70       97
```

In the case of more columns, remember to use double square brackets to wrap the specified column name.

### 2. get the data of a specific row

If we want to take out the data of row, we can use the syntax of slice to obtain the data, for example:

```python
# Get data as DataFrame
print(df_student_index[0:3])
```

```python
# output
        ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
2  1100103   f    74       77       67
```

The value here is in line with Python's habit of closing on the left and opening on the right (including the left boundary, excluding the right boundary), just like Python's list and array value rules.

### 3. get part of the row value of a column

If we want to take consecutive row values of a column, we can specify the **column name** and **row index range** in the form of a list:

```python
# Get data as Series
print(df_student_index['ID'][0:3])  
```

```python
a    1100101
b    1100102
c    1100103
Name: ID, dtype: int64
```

When taking out continuous data, use **colon** to separate, and use **single-layer square brackets** to enclose the range of row. The value rule also conforms to Python's habit of closing on the left and opening on the right.

Or we want to take multiple discontinuous row values in a column, and also use the serial form to specify the **column name** and **row index**:

```python
# Get data as Series
print(df_student_index['ID'][[0,3]])  
```

```python
a    1100101
d    1100104
Name: ID, dtype: int64
```

Use **comma** to separate discontinuous data, and then use **double square brackets** to enclose the row range.

When we want to take a column, the value of a single row can be taken like this  `df['column'][[index]]`:

```python
# Get data as Series
print(df_student_index['ID'][[0]]) 
```

```python
a    1100101
Name: ID, dtype: int64
```

Take the value of a single row in a column, and use a single layer of square brackets to wrap the index of the row, and the result of a single value will be obtained:

```python
print(df_student_index['ID'][0]) 
```

```python
1100101
```

To obtain row data in a specific column, when using **colon** to obtain continuous data, the outer layer only needs to be enclosed in a single layer of square brackets, while using **comma** to obtain multiple discontinuous data, the outer layer is enclosed in double brackets.

## Use `.loc` for data selection

In addition to the above methods, we can also use `.loc` to fetch row and column data, or part of row and part of column data.

### 1.  take out the data of the row

loc is a method of taking out data based on **label**, so we can use `.loc['index label']` to obtain the row data. For example, we want to take out the data whose index label is a:

```python
# method 1
# Get data as Series
print(df_student_index.loc['a'])   
# method 2
# Get data as DataFrame
print(df_student_index.loc[['a']])
```

```python
# output
# method 1
ID         1100101
Sex              f
Chinese         60
Math            66
English         70
Name: a, dtype: object

# method 2
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
```

To get data of multiple consecutive rows, you can use `.loc['index label':'index label']` to separate index labels with **colons**. For example, to retrieve data whose index labels are a to c:


```python
# Get data as DataFrame
print(df_student_index.loc['a':'c'])  
```

```python
# output
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
b  1100102   m       70    75       88
c  1100103   f       77    74       67
```

It should be noted here that the value of `.loc['index label':'index label']` is different from Python's habit of closing on the left and opening on the right, but in the form of closing on the left and closing on the right.

To retrieve data of multiple discontinuous rows, you can use `.loc[['index label', 'index label',]]`, separate the index labels with commas, and wrap them with square brackets. For example, take out the data whose index labels are a and c:

```python
# Get data as DataFrame
print(df_student_index.loc[['a', 'c']])  
```

```python
# output
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
c  1100103   f       77    74       67
```

### 2. take out the data of the column

Since the row data can be taken out, of course the column data can also be taken out. When we fetch column data, we can use `.loc[:, ['column label']]` , which also conforms to the principle of **row first and then column, and separate row and column data with commas**.

For example, take out the data that contains all the columns and the row name is ID:

```python
print(df_student_index.loc[:,['ID']])
```

```python
# output
        ID
a  1100101
b  1100102
c  1100103
d  1100104
e  1100105
```

Or extract data containing all row`.loc[:, ['column label','column label']]`, row name ID and Math:

```python
print(df_student_index.loc[:,['ID','Math']])
```

```python
# output
        ID  Math
a  1100101    66
b  1100102    75
c  1100103    74
d  1100104    88
e  1100105    94
```

### 3.get partial data

If we only need to get a single grade of a single student and, we can also use `.loc[['index label'], ['column label']]` to get the data of a single row and a single column:

```python
print(df_student_index.loc[['a'],['Math']])
```

```python
# output
   Math
a    66
```

When we don't use square brackets to wrap the row part and the column part, such as `.loc['index label', 'column label']`, only a single value will be displayed, like this result:

```python
print(df_student_index.loc['a','Math'])
```

```python
# output
66
```

When we want to take data of a single row and multiple consecutive columns, use **colon** to separate **column name** in the row part, such as `.loc[['index label'], 'column label':'column label']`:

```python
print(df_student_index.loc[['a'],'Chinese':'English'])
```

```python
# output
   Chinese  Math  English
a       60    66       70
```

When we want to take the data of a single row and multiple discontinuous columns, we use "comma" to separate "column names" in the column part. Remember to use double square brackets to wrap the column part, such as `.loc [['index label'], ['column label','column label']]`:

```python
print(df_student_index.loc[['a'],['Chinese','English']])
```

```python
# output
   Chinese  English
a       60       70
```

If we only need to get some grades of students and some students, we can also use `.col[['row label', 'row label'], ['column label', 'column label']]` to get discontinuous row data and discontinuous column data:
```python
print(df_student_index.loc[['a', 'c'],['Math', 'English']])
```

```python
# output
   Math  English
a    66       70
c    74       67
```

Or use `.col['row label':'row label', 'column label':'column label']` to extract continuous row data and continuous column data:

```python
print(df_student_index.loc['a':'c','ID':'Math'])
```

```python
# output
        ID Sex  Chinese  Math
a  1100101   f       60    66
b  1100102   m       70    75
c  1100103   f       77    74
```

In the `.loc` method, when using **colon** to obtain continuous data, it does not need to be enclosed in square brackets, and using **comma** to obtain multiple discontinuous data, it needs to be enclosed in square brackets.

## Use `.iloc` to select data

In addition to the `.loc` described above, there is also a `.iloc` method that retrieves row and column data based on **index**, and index starts from 0. This is also in line with the principle of **row first and then column, and separate row and column data with commas"**

### 1. get row data

When we want to get the data of the first row, we can use `.iloc[index]` or `.iloc[[index]]` :

```python
# method 1
# Get data as Series
print(df_student_index.iloc[0])     

# method 2
# Get data as DataFrame
print(df_student_index.iloc[[0]])   
```

```python
# output
# method1
ID         1100101
Sex              f
Chinese         60
Math            66
English         70
Name: a, dtype: object

# method 2
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
```

If you take multiple consecutive row data, you also use a **colon** to separate the **column part** such as `.iloc[index:index]`, and it conforms to the Python rule of left-closed and right-open:

```python
print(df_student_index.iloc[0:3])
```

```python
# output
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
b  1100102   m       70    75       88
c  1100103   f       77    74       67
```

If multiple non-consecutive row data are taken, use **comma** to separate **row parts** such as `.iloc[[index,index]]`:

```python
print(df_student_index.iloc[[0,3]])
```

```python
# output
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
d  1100104   f       69    88       89
```

It should be noted here that when a single layer of square brackets is used to wrap `.iloc[0,3]` , it means that the data in the 0th column and the 3rd line is taken:

```python
print(df_student_index.iloc[0,3])
```

```python
# output
66
```

### 2. get column data 

If you want to get the data of a single column, you can get `.iloc[:,index]` or `.iloc[:,[index]]` like this:

```python
# method 1
# Get data as Series
print(df_student_index.iloc[:,2])   

# method 2
# Get data as DataFrame
print(df_student_index.iloc[:,[2]])  
```

```python
# method 1
a    60
b    70
c    77
d    69
e    70
Name: Chinese, dtype: int64

# method 2
   Chinese
a       60
b       70
c       77
d       69
e       70
```

If you take continuous row data, use "colon" to separate "row parts" such as `.iloc[:,index:index]`, which also conforms to Python's left-closed and right-open rules:

```python
print(df_student_index.iloc[:,0:2])
```

```python
# output
        ID Sex
a  1100101   f
b  1100102   m
c  1100103   f
d  1100104   f
e  1100105   m
```

If you want to take discontinuous column data, use "comma" to separate "row part", such as `.iloc[:,[index,index]]`:

```python
print(df_student_index.iloc[:,[0,2]])
```

```python
# output
        ID  Chinese
a  1100101       60
b  1100102       70
c  1100103       77
d  1100104       69
e  1100105       70
```

### 3.get partial data

When we want to take a single row value and a single column value, we can use `.iloc[[index],[index]]`:

```python
print(df_student_index.iloc[[0],[3]])
```

```python
# output
   Math
a    66
```

Alternatively, `.iloc[index,index]` can be used, and the output is a single value:

```python
print(df_student_index.iloc[0,3])
```

```python
# output
66
```

In the `.loc` method, when using **colon** to obtain continuous data, it does not need to be enclosed in square brackets, and using **comma** to obtain multiple discontinuous data, it needs to be enclosed in square brackets.

## Conclusion
Through the methods of `.loc` and `.iloc`, we are more flexible whether we are fetching row data or column data, jumping out of the limitation of value in the general serial form, allowing us to use index label or index to select pandas Data, as long as we understand the principles of using these methods, we can use them freely!