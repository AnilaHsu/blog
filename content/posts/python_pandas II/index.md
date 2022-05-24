---
title: "Pandas - 好用的資料選擇方法 "
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

## 前言

本篇會針對 Pandas 中 DataFrame 的取值方法進行介紹，主要分為三個部分：

- 基本的資料選擇
- DataFrame.loc 的方法
- DataFrame.iloc 的方法

在下文介紹的 .loc 及 .iloc 方法中，資料選擇的規則是「**先列再行，並以逗號間隔列與行的資料」** ex [列, 行]，因此在取資料的時候要特別注意。那話不多說就實際來看看這些方法要如何操作~~!

## 一、基本的資料選擇

在 DataFrame 的資料結構中，我們可以透過直接透過 index 和 column 來找到某一筆符合條件的資料。那我們以 df_student_index 為例來實際操作看看如何篩選和取出資料吧。

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

### 1. 取出特定 column (行) 的資料

針對 column 的取出，我們可以在資料後面指定該「行」 的名字，使用 「.行名」，比如取出欄位為 English 的資料：

```python
print(df_student_index.English) # 取得資料為 Series 
```

```python
# 輸出
a    70
b    88
c    67
d    89
e    97
Name: English, dtype: int64
```

我們也可以使用 Python 的「串列形式」來取出單行的資料，並在串列中指定行的名稱。

比如取出欄位為 Chinese 的資料：

```python
# 方法一
print(df_student_index['Chinese'])   # 取得資料為 Series 
# 方法二
print(df_student_index[['Chinese']]) # 取得資料為 DataFrame
```

```python
# 輸出
# 方法一
a    60
b    70
c    77
d    69
e    70
Name: Chinese, dtype: int64
# 方法二
   Chinese
a       60
b       70
c       77
d       69
e       70
```

其中可以發現，當資料使用單層中括號時包住，會呈現一個 series 的資料型別；若使用雙層中括號包住，則會取的一個 DataFrame 的資料結構。

當我們要取出多行的資料時，同樣也是以「串列形式」來指定行名，比如取出欄位為 Chinese 和 English 的資料：

```python
print(df_student_index[['Chinese', 'English']]) # 取得資料為 DataFrame
```

```python
# 輸出
   Chinese  English
a       60       70
b       70       88
c       77       67
d       69       89
e       70       97
```

再多行的情況下，記得要使用雙層中括號包覆指定的串列名稱ㄛ！

### 2. 取出特定 row (列) 的資料

如果我們要取出的是列資料，則可以使用 slice 的語法來取得資料，比如：

```python
print(df_student_index[0:3]) # 取得資料為 DataFrame
```

```python
# 輸出
        ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
2  1100103   f    74       77       67
```

這邊的取值符合 Python 左閉右開的習慣 (包含左邊界，不包含右邊界)，如同 Python 的 list、array 取值的規則。

### 3. 取出某一個 column (行) 的部分 row (列) 值

若我們要取某一個 column 的連續 row 值，可以串列形式來指定「行名」及「列的 index 範圍」:

```python
print(df_student_index['ID'][0:3])  # 取得資料為 Series 
```

```python
a    1100101
b    1100102
c    1100103
Name: ID, dtype: int64
```

在取連續資料的時候，使用『冒號』分隔，並用『單層中括號』包住列的範圍，取值規則同樣符合 Python 左閉右開的習慣。

或是我們要取某個 column 中，多個不連續 row 值，同樣使用串列形式來指定「行名」及「列的 index 」：

```python
print(df_student_index['ID'][[0,3]])  # 取得資料為 Series 
```

```python
a    1100101
d    1100104
Name: ID, dtype: int64
```

不連續的資料使用『逗號』分隔，再使用『雙層中括號』包住列的範圍。

當我們要取某個 column 中，單個 row 的值可以這樣取：

```python
print(df_student_index['ID'][[0]])  # 取得資料為 Series 
```

```python
a    1100101
Name: ID, dtype: int64
```

取某一個 column 中，單個 row 的值，並使用單層中括號包住列的 index，則會取得單個值的結果：

```python
print(df_student_index['ID'][0]) 
```

```python
1100101
```

在某一個特定 column 中取 row 資料，當使用『冒號』取得連續資料，外層只需單層中括號包覆，而使用『逗號』取得多個不連續資料，外層使用雙層中括號包覆。

## 二、使用 .loc 進行資料選擇

除了上述的方法，我們也可以使用 .loc 來取的列、行的資料，或是部分的列、部分的行資料。

### 1.  取出 row (列) 的資料

loc 是基於「label」 (標籤) 來取資料的方法，因此我們可以使用 `.loc['index label']` 來取出該 列資料。比如我們要取出 index label 為 a 的資料：

```python
# 方法一
print(df_student_index.loc['a'])   # 取得資料為 Series
# 方法二
print(df_student_index.loc[['a']]) # 取得資料為 DataFrame
```

```python
# 輸出
# 方法一
ID         1100101
Sex              f
Chinese         60
Math            66
English         70
Name: a, dtype: object

# 方法二
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
```

若要取的多個連續 row 的資料，則可以使用 ****`.loc['index label':'index label']`，以『冒號』間隔 index label，比如取出 index label 為 a 至 c 的資料：

```python
print(df_student_index.loc['a':'c'])  # 取得資料為 DataFrame
```

```python
# 輸出
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
b  1100102   m       70    75       88
c  1100103   f       77    74       67
```

這邊要特別注意的是 `.loc['index label':'index label']` 的取值不同於 Python 左閉右開的習慣，而是左閉右閉的形式，也就是說包含左邊界及右邊界。

若要取的多個不連續 row 的資料，可以使用 `.loc[['index label', 'index label',]]`，以『逗號』間隔 index label，並使用中括號包覆。比如取出 index label 為 a 和 c 的資料：

```python
print(df_student_index.loc[['a', 'c']])  # 取得資料為 DataFrame
```

```python
# 輸出
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
c  1100103   f       77    74       67
```

### 2.  取出 column (行) 的資料

既然可以取出 row 的資料，當然也可以取出 column 的資料。在我們取 column 的資料時，可以使用 `.loc[:, ['column label']]` ，同時符合「先列再行，並以逗號間隔列與行的資料」的原則。

比如取出包含所有列，行名為 ID 的資料：

```python
print(df_student_index.loc[:,['ID']])
```

```python
# 輸出
        ID
a  1100101
b  1100102
c  1100103
d  1100104
e  1100105
```

或是取出包含所有列`.loc[:, ['column label','column label']]`，行名為 ID 和 Math 的資料：

```python
print(df_student_index.loc[:,['ID','Math']])
```

```python
# 輸出
        ID  Math
a  1100101    66
b  1100102    75
c  1100103    74
d  1100104    88
e  1100105    94
```

### 3.取出部分資料

假如我們只要取出單個學生和的單個成績，也可以使用 `.loc[['index label'], ['column label']]`來取出單個的列和單個行的資料：

```python
print(df_student_index.loc[['a'],['Math']])
```

```python
# 輸出
   Math
a    66
```

當我們沒有使用中括號包住列的部分和欄的部分，如`.loc['index label', 'column label']`，就只會顯示單個值，如同這樣的結果：

```python
print(df_student_index.loc['a','Math'])
```

```python
# 輸出
66
```

當我們要取的是單個 row ，多個連續 column 的資料，則在行的部分使用『冒號』間隔「行的名稱」如 `.loc[['index label'], 'column label':'column label']`：

```python
print(df_student_index.loc[['a'],'Chinese':'English'])
```

```python
# 輸出
   Chinese  Math  English
a       60    66       70
```

當我們要取的是單個 row 和多個不連續 column 的資料，則是在行的部分使用『逗號』間隔「行的名稱」，記得使用雙層中括號包覆行的部分，如`.loc[['index label'], ['column label','column label']]`：

```python
print(df_student_index.loc[['a'],['Chinese','English']])
```

```python
# 輸出
   Chinese  English
a       60       70
```

假如我們只要取出部分學生和的部分成績，也可以使用 `.col[['row label', 'row label'], ['column label', 'column label']]`來取出不連續的列資料和不連續的行資料：

```python
print(df_student_index.loc[['a', 'c'],['Math', 'English']])
```

```python
# 輸出
   Math  English
a    66       70
c    74       67
```

或是使用 `.col['row label':'row label', 'column label':'column label']` 來取出連續的列資料和連續的行資料：

```python
print(df_student_index.loc['a':'c','ID':'Math'])
```

```python
# 輸出
        ID Sex  Chinese  Math
a  1100101   f       60    66
b  1100102   m       70    75
c  1100103   f       77    74
```

在 .loc 的方法中，當使用『冒號』取得連續資料，不需中括號包覆，而使用『逗號』取得多個不連續資料，需使用中括號包覆。

## 三、使用 .iloc 選擇資料

除了上述所介紹的 .loc ，還有一種 .iloc 的方法是基於「index」來取列與行的資料，index 從 0 開始。這邊同樣也符合「先列再行，並以逗號間隔列與行的資料」的原則。

### 1.  取出 row (列) 的資料

當我們要取第一列的資料，就可以使用 `.iloc[index]` 或 `.iloc[[index]]` ：

```python
# 方法一
print(df_student_index.iloc[0])     # 取得資料為 Series

# 方法二
print(df_student_index.iloc[[0]])   # 取得資料為 DataFrame
```

```python
# 輸出
# 方法一
ID         1100101
Sex              f
Chinese         60
Math            66
English         70
Name: a, dtype: object

# 方法二
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
```

若是取多個連續的列資料，同樣是使用『冒號』間隔「列的部分」如`.iloc[index:index]` ，且符合Python 左閉右開的規則：

```python
print(df_student_index.iloc[0:3])
```

```python
# 輸出
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
b  1100102   m       70    75       88
c  1100103   f       77    74       67
```

若是取多個不連續的列資料，則使用『逗號』的方式間隔「列的部分」如 `.iloc[[index,index]]`：

```python
print(df_student_index.iloc[[0,3]])
```

```python
# 輸出
        ID Sex  Chinese  Math  English
a  1100101   f       60    66       70
d  1100104   f       69    88       89
```

這邊要注意的是，當使用單層中括號包覆 .iloc[0,3] ，代表的是取的是第 0 列，第 3 行的資料：

```python
print(df_student_index.iloc[0,3])
```

```python
# 輸出
66
```

### 2.  取出 column (行) 的資料

若是取單行的資料可以這樣取`.iloc[:,index]` 或 `.iloc[:,[index]]`：

```python
# 方法一
print(df_student_index.iloc[:,2])    # 取得資料為 Series

# 方法二
print(df_student_index.iloc[:,[2]])   # 取得資料為 DataFrame
```

```python
# 方法一
a    60
b    70
c    77
d    69
e    70
Name: Chinese, dtype: int64

# 方法二
   Chinese
a       60
b       70
c       77
d       69
e       70
```

若是取連續的行資料則使用『冒號』的方式間隔「行的部分」如`.iloc[:,index:index]`，這邊同樣符合 Python 左閉右開的規則：

```python
print(df_student_index.iloc[:,0:2])
```

```python
# 輸出
        ID Sex
a  1100101   f
b  1100102   m
c  1100103   f
d  1100104   f
e  1100105   m
```

若是取不連續的欄資料則使用『逗號』的方式間隔「行的部分」，如`.iloc[:,[index,index]]`：

```python
print(df_student_index.iloc[:,[0,2]])
```

```python
# 輸出
        ID  Chinese
a  1100101       60
b  1100102       70
c  1100103       77
d  1100104       69
e  1100105       70
```

### 3.取出部分資料

當我們要取單個列值及單個行值，則可以使用 `.iloc[[index],[index]]`：

```python
print(df_student_index.iloc[[0],[3]])
```

```python
# 輸出
   Math
a    66
```

也可以選擇用`.iloc[index,index]` ，所輸出的結果就是單個值：

```python
print(df_student_index.iloc[0,3])
```

```python
# 輸出
66
```

在 .loc 的方法中，當使用『冒號』取得連續資料，不需中括號包覆，而使用『逗號』取得多個不連續資料，需使用中括號包覆。

## 結論

透過 .loc 和 .iloc 的方法，使得我們不論在取列的資料或是行的資料都更加有彈性，跳出一般串列形式取值的限制，讓我們可以使用 index label 或是 index 來選擇 pandas 的資料，只要我們理解這些方法的使用原則，就可以運用自如嘞！