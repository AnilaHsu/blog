---
title: "Pandas - Data manipulation of Series & DataFrame"
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

Among the Python libraries, Numpy, Pandas, Matplotlib, Scipy, and scikit-learn are important libraries and modules commonly used for data analysis, data science, and machine learning.
  <!--more-->
In fact, Pandas is not a tool for data science, but a tool for the pre-analytical stage of data science. This article will mainly introduce the basic use of important data structures Series and DataFrame in Pandas.

## Knowledge of Pandas

Pandas is a very useful library for data preprocessing and analysis. It combines the characteristics of Numpy and has data manipulation capabilities similar to Excel and SQL, allowing us to perform various flexible processing of various data in the form of DataFrame.

The main features of Pandas are as follows:

- Provides two main data structures: Series, DataFrame. Series is mainly used to create a one-dimensional array of indexes to process data related to time series; while DataFrame is a two-dimensional data set with column indexes and column labels, similar to Excel's data table or SQL's relational database .

- Through the data structure of Pandas and the method of structuring objects, the data is processed in a more diverse manner. Such as filling, deleting or replacing data with null values.

- Easier to read, convert and process heterogeneous data. For example, to find out a column or row that meets a certain condition from the data, or to separate and combine the data, etc.

- More diverse input sources and integrated output methods. For example, you can read data from CSV or database into DataFrame, and you can also convert processed data into CSV or database.

Through Pandas, we can overcome the limitations of Excel, and also make the work of data analysis more convenient and easier, and can more quickly discover the information in the data and its meaning.

### Install

```python
pip3 install pandas
```

### Import

```python
import pandas as pd
```

## Create Series

Series are objects similar to one-dimensional arrays. We can use Python's list data type or dictionary data type to create a Series:

### 1. Use list to create Series

```python
list_1 = [10, 20, 30, 40, 50, 60]
data = pd.Series(list_1)
print(data)
```

```python
# output
0    10
1    20
2    30
3    40
4    50
5    60
dtype: int64
```

The left part is index and the right part is value . In addition to specifying the value of the data, we can also specify the index value of the data through the index parameter:

```python
list_1 = [10, 20, 30, 40, 50, 60]
data = pd.Series(list_1,index=['a','b','c','e','d','f'])
print(data)
```

```python
# output
a    10
b    20
c    30
e    40
d    50
f    60
dtype: int64
```

### 2. Use dictionary to create Series
```python
dict1 = {'a':10, 'b':20, 'c':30, 'd':40, 'e':50}
data = pd.Series(dict1)
print(data)
```

```python
# output
a    10
b    20
c    30
d    40
e    50
dtype: int64
```

For the index and value of the data, we can use the index attribute and the values attribute to retrieve them respectively:

```python
dict1 = {'a':10, 'b':20, 'c':30, 'd':40, 'e':50}
data = pd.Series(dict1)
print(data.index)
print(data.values)
```

```python
# output
# print(data.index)
Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
# print(data.values)
[10 20 30 40 50]
```

It is worth noting that the custom index value of Series does not apply to the data type of dict, because the dict itself has a Key to specify the index value of the Series, and the value of the custom index will become NaN (no data).


```python
dict1 = {'a':10, 'b':20, 'c':30, 'd':40, 'e':50}
data = pd.Series(dict1,index=['f', 'g', 'h', 'i', 'j'])
print(data)
```

```python
# output
f   NaN
g   NaN
h   NaN
i   NaN
j   NaN
dtype: float64
```

But we can change the order of the data by changing the order of the index:

```python
dict1 = {'a':10, 'b':20, 'c':30, 'd':40, 'e':50}
data = pd.Series(dict1,index=['b', 'a', 'd', 'e', 'c'])
print(data)
```

```python
# output
b    20
a    10
d    40
e    50
c    30
dtype: int64
```

## Create DataFrame

DataFrame is a two-dimensional data object with index and column, which is the most important data structure in Pandas. Basically, when we use Pandas for data manipulation and analysis, we mostly use the data structure of DataFrame. 

DataFrame can set different dtypes for each column, such as int, str, bool, etc. In addition to using dictionary and ndarray to create DataFrame, we can also create it by reading external data (such as: CSV, SQL, JOSN, HTML).

### 1. Use dictionary to create DataFrame

To create a DataFrame with a dictionary, just convert the data in the dict type into a DataFrame:

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105],'Sex':['f', 'm', 'f', 'f', 'm'],'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df = pd.DataFrame(dict_student)
print(df)
```

```python
# output
        ID Sex  Chinese  Math
0  1100101   f       60    66
1  1100102   m       70    75
2  1100103   f       77    74
3  1100104   f       69    88
4  1100105   m       70    94
```

### 2. Use ndarray to create DataFrame

We can also use a simple array to create a DataFrame, and set the column names through the columns parameter:

```python
student =(np.array([[1100101, 'f', 60, 66], [1100102, 'm', 70, 75], [1100103, 'f', 77, 74], [1100104, 'f', 69, 88],[1100105, 'm', 70, 94]]))
df_student = pd.DataFrame(student,columns=['ID', 'Sex', 'Chinese', 'Math'])
print(df_student)
```

```python
# output
        ID Sex Chinese Math
0  1100101   f      60   66
1  1100102   m      70   75
2  1100103   f      77   74
3  1100104   f      69   88
4  1100105   m      70   94
```


### 3. Read external data to create DataFrame

As mentioned earlier, we can read external data, such as CSV, SQL, JOSN, HTML, to convert it into a DataFrame data structure. For example, if we want to load a CSV file, we can write it like this:

```python
df = pd.read_csv('檔名')
```

```python
df = pd.read_html('檔名')
```

### 4. Index of custom data

The DataFrame is the same as the Series, and the index value of the data can also be customized through the index parameter:

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df_student_index = pd.DataFrame(dict_student,index=['a', 'b', 'c', 'd', 'e'])
print(df_student_index)
```

```python
# output
        ID Sex  Chinese  Math
a  1100101   f       60    66
b  1100102   m       70    75
c  1100103   f       77    74
d  1100104   f       69    88
e  1100105   m       70    94
```

It should be noted that in the index value of the custom DataFrame, data is applicable to data that has not been converted into a DataFrame, such as the above example **data = dict_student** or the above example **data = student** :

```python
student =([1100101, 'f', 60, 66], [1100102, 'm', 70, 75], [ 1100103, 'f', 77, 74], [1100104, 'f', 69, 88], [1100105, 'm', 70, 94])
df_student = pd.DataFrame(student,columns=['ID','Sex','Chinese','Math'],index=['a', 'b', 'c', 'd', 'e'])
print(df_student)
```

```python
# output
        ID Sex  Chinese  Math
a  1100101   f       60    66
b  1100102   m       70    75
c  1100103   f       77    74
d  1100104   f       69    88
e  1100105   m       70    94
```

In the process of converting to DataFrame, the value of index has been assigned, as follows:

```python
# convert to df
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df = pd.DataFrame(dict_student)
print(df)
```

```python
        ID Sex  Chinese  Math
0  1100101   f       60    66
1  1100102   m       70    75
2  1100103   f       77    74
3  1100104   f       69    88
4  1100105   m       70    94 
```

When the value of index is customized, the original data will become NaN (no data), which is similar to the situation where the index value of the Series dict needs to be determined.

```python
# Add a custom index to the data converted to df
df_student_index = pd.DataFrame(df,index=['a', 'b', 'c', 'd', 'e'])
print(df_student_index)
```

```python
   ID  Sex  Chinese  Math
a NaN  NaN      NaN   NaN
b NaN  NaN      NaN   NaN
c NaN  NaN      NaN   NaN
d NaN  NaN      NaN   NaN
e NaN  NaN      NaN   NaN
```

Let's actually verify what it looks like when the data is converted to a DataFrame and then converted back to a dictionary:

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df = pd.DataFrame(dict_student)
print(df.to_dict())
```

```python
{'ID': {0: 1100101, 1: 1100102, 2: 1100103, 3: 1100104, 4: 1100105}, 'Sex': {0: 'f', 1: 'm', 2: 'f', 3: 'f', 4: 'm'}, 'Chinese': {0: 60, 1: 70, 2: 77, 3: 69, 4: 70}, 'Math': {0: 66, 1: 75, 2: 74, 3: 88, 4: 94}} 
```

We can see that the data that has been converted into a DataFrame has indeed been assigned an index value.

### 5. Change column order, add new column

In the process of data analysis, we may need to change the order of columns or add new columns. These requirements can also be helped us through the column parameter.

If you want to change the column order, you can write it like this:

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df = pd.DataFrame(dict_student)
print(df)

# method 1
df_columns1 = pd.DataFrame(df,columns=['ID', 'Sex', 'Math', 'Chinese'])
# method 2
df_columns2 = pd.DataFrame(dict_student,columns=['ID', 'Sex', 'Math', 'Chinese'])
print(df_columns2)

```

```python
# output
# original data order
        ID Sex  Chinese  Math
0  1100101   f       60    66
1  1100102   m       70    75
2  1100103   f       77    74
3  1100104   f       69    88
4  1100105   m       70    94
# Changed data order
        ID Sex  Math  Chinese
0  1100101   f    66       60
1  1100102   m    75       70
2  1100103   f    74       77
3  1100104   f    88       69
4  1100105   m    94       70
```

If you want to add a new column, you can write it like this:

```python
# method 1
df_columns_add1 = pd.DataFrame(df,columns=['ID', 'Sex', 'Math', 'Chinese', 'English']
# method 2
df_columns_add2 = pd.DataFrame(dict_student,columns=['ID', 'Sex', 'Math', 'Chinese', 'English'])
print(df_columns_add2)
```

```python
# output 
        ID Sex  Math  Chinese  English
0  1100101   f    66       60      NaN
1  1100102   m    75       70      NaN
2  1100103   f    74       77      NaN
3  1100104   f    88       69      NaN
4  1100105   m    94       70      NaN
```

You can see that an English column has been successfully added, and then the English value is given through the list:

```python
list_English = [70,88,67,89,97]
df_columns_add2['English'] = list_English
print(df_columns_add2)
```

```python
# output 
        ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
2  1100103   f    74       77       67
3  1100104   f    88       69       89
4  1100105   m    94       70       97
```

## DataFrame data information

After we have created the DataFrame, we can use the methods or properties in the DataFrame data structure to view the information of the data. The more common methods are: .shape, .head(), .tail(), .describe(), .index, .columns . Let's take df_student below as an example, let's take a look at these methods in practice.

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94], 'English':[70, 88, 67, 89, 97]}
df_student = pd.DataFrame(dict_student)
print(df_score)
```

```python
# print(df_student) 
        ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
2  1100103   f    74       77       67
3  1100104   f    88       69       89
4  1100105   m    94       70       97
```

### 1. View data row, column

When we want to see how many rows and columns df_student has, we can use .shape, and the result is (row, column):

```python
# .shape
print(df_student.shape)
```

```python
# output
(5, 5)
```

### 2. View the value of the previous n data

When we want to view the values of the previous records of df_student, we can use .head(). The default is to display 5 records. We can also specify the number of records to be displayed by substituting a value in .head(), such as viewing the first 2 records:

```python
# .head()
print(df_student.head(2))
```

```python
# output
        ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
```

### 3. View the value of the last n records

When we want to see the value of the last few records of df_student, we can use .tail(). The default is also to display 5 records. We can specify the number of records to be displayed by substituting a value in .tail(), for example, to view the last 2 records:

```python
# .tail()
print(df_student.tail(2))
```

```python
# 輸出
        ID Sex  Math  Chinese  English
3  1100104   f    88       69       89
4  1100105   m    94       70       97
```

*It should be noted that both .head() and .tail() can only display up to 5 pieces of data

### 4. View descriptive statistics for a profile

 When we want to view the descriptive statistics of df_student, we can use .describe(), through which we can more easily understand the summary of the entire data:

```python
# .describe()
print(df_student.describe())
```

```python
# output 
                 ID       Math    Chinese    English
count  5.000000e+00   5.000000   5.000000   5.000000
mean   1.100103e+06  79.400000  69.200000  82.200000
std    1.581139e+00  11.349009   6.058052  13.026895
min    1.100101e+06  66.000000  60.000000  67.000000
25%    1.100102e+06  74.000000  69.000000  70.000000
50%    1.100103e+06  75.000000  70.000000  88.000000
75%    1.100104e+06  88.000000  70.000000  89.000000
max    1.100105e+06  94.000000  77.000000  97.000000
```

### 5. View the index of the profile

When we want to see the index of df_student, we can use the .index property:

```python
# .index
print(df_student.index)
```

```python
# 輸出
RangeIndex(start=0, stop=5, step=1)
```

### 6. View all column names of the data

When we want to see all the column names in df_student, we can use the .columns property:

```python
# .columns
print(df_student.columns)
```

```python
# output
Index(['ID', 'Sex', 'Math', 'Chinese', 'English'], dtype='object')
```

### 7. View the content of the profile

To view the data content of df_student, we can use the properties of .info:

```python
# .info
print(df_student.info)
```

```python
# output
<bound method DataFrame.info of ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
2  1100103   f    74       77       67
3  1100104   f    88       69       89
4  1100105   m    94       70       97>
```

### 8. Convert row to column 

In addition to these methods for viewing data information, DataFrame also provides operations like matrix transpose, allowing the .T methods to swap rows and columns:

```python
# .T
print(df_student.T)
```

```python
# output
               0        1        2        3        4
ID       1100101  1100102  1100103  1100104  1100105
Sex            f        m        f        f        m
Math          66       75       74       88       94
Chinese       60       70       77       69       70
English       70       88       67       89       97
```


When we do data processing before, we often use these methods to help us better understand the content and overview of the entire data, and then perform data processing and processing, such as: data extraction, data deletion, merging or Sorting, etc., and even more advanced operations such as handling of nulls and outliers.

Then, due to the limited space of this article, in the next article, let's learn about Pandas' data preprocessing and how to operate DataFrame!

After reading this, do you know more about Pandas?~