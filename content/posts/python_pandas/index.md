---
title: "Pandas 基礎 - Python 資料分析"
description: ""
date: 2021-08-18
draft: false
categories: 
- Python
tags:
- Pyrhon
- Pandas
cover:
    image: "cover.jpeg"
    relative: true
---


## 前言

在 Python 套件生態系中，Numpy、Pandas、Matplotlib、Scipy 以及 scikit-learn 是常用來進行資料分析、資料科學和機器學習應用的重要套件和模組。

實際上，Pandas 不算是資料科學的工具，而是資料科學分析工具前階段的工具。 本篇文章主要會針對 Pandas 中，重要的資料結構 Series 和 DataFrame 的基礎使用進行介紹。

## 一、Pandas 的認識

Pandas 是將資料預處理及分析資料非常好用的函式庫。它結合了Numpy 的特性，並擁有 Excel 和 SQL 的資料操作能力，使我們藉由 DataFrame 的形式，對各式各樣的資料進行各種有彈性的加工。

Pandas 的主要的特色如下：

- 提供兩種主要的資料結構：Series、DataFrame。Series 主要用於建立索引的一維陣列，用來處理時間序列相關的資料；而 DataFrame 則是有列索引和欄標籤的二維資料集，類似於 Excel 的資料表或是 SQL 的關聯式資料庫。
- 透過 Pandas 的資料結構及結構化物件的方法，將資料進行更多元的處理。像是將資料進行空值的填補、刪除或取代。
- 在異質資料的讀取、轉換和處理上更容易。像是從資料當中找出符合某個條件的欄或列，或是對資料進行分隔、結合等操作等。
- 更多樣的輸入來源及整合性的輸出方式。像是可以從 CSV 或資料庫讀取資料轉成 DataFrame ，也可以將處理完的資料轉成CSV 或資料庫。

透過 Pandas 的套件，使我們可以跨越 Excel 的限制，也使資料分析的工作更方便輕鬆，而且能夠更快速地發現資料中的資訊與其中的意義。

### 安裝

```python
pip3 install pandas
```

### 匯入

```python
import pandas as pd
```

## 二、Series (序列) 建立

Series 是類似於一維陣列的物件。我可以使用 Python 的 list 資料型別或是 dictionary 資料型別來建立 Series：

### 1. 使用 list 來建立 Series

```python
list_1 = [10, 20, 30, 40, 50, 60]
data = pd.Series(list_1)
print(data)
```

```python
# 輸出
0    10
1    20
2    30
3    40
4    50
5    60
dtype: int64
```

左邊的部分為 index (索引值)，右邊的部分為 value (資料值)，除了指定資料的 value 以外，我們也可以透過 index 參數來指定資料的索引值：

```python
list_1 = [10, 20, 30, 40, 50, 60]
data = pd.Series(list_1,index=['a','b','c','e','d','f'])
print(data)
```

```python
# 輸出
a    10
b    20
c    30
e    40
d    50
f    60
dtype: int64
```

### 2. 使用 dictionary 來建立 Series

```python
dict1 = {'a':10, 'b':20, 'c':30, 'd':40, 'e':50}
data = pd.Series(dict1)
print(data)
```

```python
# 輸出
a    10
b    20
c    30
d    40
e    50
dtype: int64
```

對於資料的 index 和 value，我們可以使用 index 屬性和 values 屬性各自取出：

```python
dict1 = {'a':10, 'b':20, 'c':30, 'd':40, 'e':50}
data = pd.Series(dict1)
print(data.index)
print(data.values)
```

```python
# 輸出
# print(data.index)
Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
# print(data.values)
[10 20 30 40 50]
```

值得注意的是，Series 的自訂索引值不適用於 dict 的資料型態，因為 dict 本身有 Key 來指定 Series 的 index 值，當自訂 index 的值會變成 NaN (無資料)。

```python
dict1 = {'a':10, 'b':20, 'c':30, 'd':40, 'e':50}
data = pd.Series(dict1,index=['f', 'g', 'h', 'i', 'j'])
print(data)
```

```python
# 輸出
f   NaN
g   NaN
h   NaN
i   NaN
j   NaN
dtype: float64
```

但我們可以透過改變 index 的排列來改變資料的順序：

```python
dict1 = {'a':10, 'b':20, 'c':30, 'd':40, 'e':50}
data = pd.Series(dict1,index=['b', 'a', 'd', 'e', 'c'])
print(data)
```

```python
# 輸出
b    20
a    10
d    40
e    50
c    30
dtype: int64
```

## 三、DataFrame (資料框) 建立

DataFrame 是一個具有 index (索引值) 和 column (欄位) 的二維資料物件，是 Pandas 最重要的資料結構。基本上我們在使用 Pandas 進行資料的操作和分析時，大多都是使用 DataFrame 的資料結構。

DataFrame 對於各個 column 可以設定不同的 dtype (資料型別)，如 int、str、bool 等。我們除了可以使用 dictionary、ndarray 來創建 DataFrame，還可以透過讀取外部資料(如：CSV、SQL、JOSN、HTML)的方式來創建。

### 1. 使用 dictionary 來創建 DataFrame

使用 dictionary 來創建 DataFrame 只要將 dict 型態的資料轉成 DataFrame 就可以：

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105],'Sex':['f', 'm', 'f', 'f', 'm'],'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df = pd.DataFrame(dict_student)
print(df)
```

```python
# 輸出
        ID Sex  Chinese  Math
0  1100101   f       60    66
1  1100102   m       70    75
2  1100103   f       77    74
3  1100104   f       69    88
4  1100105   m       70    94
```

### 2. 使用 ndarray 來創建 DataFrame

我們也可以使用單純的 array 來創建 DataFrame，並透過 columns 的參數來設定欄位名稱：

```python
student =(np.array([[1100101, 'f', 60, 66], [1100102, 'm', 70, 75], [1100103, 'f', 77, 74], [1100104, 'f', 69, 88],[1100105, 'm', 70, 94]]))
df_student = pd.DataFrame(student,columns=['ID', 'Sex', 'Chinese', 'Math'])
print(df_student)
```

```python
# 輸出
        ID Sex Chinese Math
0  1100101   f      60   66
1  1100102   m      70   75
2  1100103   f      77   74
3  1100104   f      69   88
4  1100105   m      70   94
```

我們也可以使用單純的 array 來創建 DataFrame，透過是 columns 的參數來設定欄位名稱：

```python
arr_student =([1100101, 'f', 60, 66],[1100102, 'm', 70, 75], [1100103, 'f', 77, 74], [1100104, 'f', 69, 88], [1100105, 'm', 70, 94])
df_student = pd.DataFrame(student,columns=['ID', 'Sex', 'Chinese', 'Math'])
print(df_student)
```

```python
# 輸出
        ID Sex  Chinese  Math
0  1100101   f       60    66
1  1100102   m       70    75
2  1100103   f       77    74
3  1100104   f       69    88
4  1100105   m       70    94
```

### 3.  讀取外部資料來創建 DataFrame

前面有說到我們可以讀取外部資料，如 CSV、SQL、JOSN、HTML 來轉換成 DataFrame 的資料結構。比如我們想載入 CSV 檔可以這以這樣撰寫：

```python
df = pd.read_csv('檔名')
```

```python
df = pd.read_html('檔名')
```

### 4. 自訂資料的 index (索引值)

而DataFrame 與 Series 相同，也可以透過 index 參數來自訂資料的索引值：

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df_student_index = pd.DataFrame(dict_student,index=['a', 'b', 'c', 'd', 'e'])
print(df_student_index)
```

```python
# 輸出
        ID Sex  Chinese  Math
a  1100101   f       60    66
b  1100102   m       70    75
c  1100103   f       77    74
d  1100104   f       69    88
e  1100105   m       70    94
```

要特別注意的是，在自訂 DataFrame 的索引值，data 適用於為未轉換成 DataFrame 的資料，比如上方的例子 data = dict_student 或是上方的例子 data = student ：

```python
student =([1100101, 'f', 60, 66], [1100102, 'm', 70, 75], [ 1100103, 'f', 77, 74], [1100104, 'f', 69, 88], [1100105, 'm', 70, 94])
df_student = pd.DataFrame(student,columns=['ID','Sex','Chinese','Math'],index=['a', 'b', 'c', 'd', 'e'])
print(df_student)
```

```python
# 輸出
        ID Sex  Chinese  Math
a  1100101   f       60    66
b  1100102   m       70    75
c  1100103   f       77    74
d  1100104   f       69    88
e  1100105   m       70    94
```

若將資料轉換成 DataFrame 再使用自訂義的 index 會變成 NaN。主要是因為資料再轉換成 DataFrame 的過程中已經被賦予 index 的值，如下：

```python
# 轉換成 df
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

當自訂 index 的值時，原本的資料會變成 NaN (無資料)，這與 Series 的 dict 要訂定索引值的狀況相似。

```python
# 將轉換成 df 的資料新增自訂義的 index
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

我們實際來驗證一下被轉換成 DataFrame 的資料再轉換回字典的樣子：

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df = pd.DataFrame(dict_student)
print(df.to_dict())
```

```python
{'ID': {0: 1100101, 1: 1100102, 2: 1100103, 3: 1100104, 4: 1100105}, 'Sex': {0: 'f', 1: 'm', 2: 'f', 3: 'f', 4: 'm'}, 'Chinese': {0: 60, 1: 70, 2: 77, 3: 69, 4: 70}, 'Math': {0: 66, 1: 75, 2: 74, 3: 88, 4: 94}} 
```

我們可以看見已經轉換成 DataFrame 的資料，的確已經被賦予 index 值了。

### 5. 更改 column 順序、新增新的 column

在進行資料分析的過程中，我們有可能會需要更改 column 的順序或是新增 column ，這些需求也可以透過 column 參數來幫助我們。

如果是要更改 column 順序，可以這樣撰寫：

```python
dict_student = {'ID':[1100101, 1100102, 1100103, 1100104, 1100105], 'Sex':['f', 'm', 'f', 'f', 'm'], 'Chinese':[60, 70, 77, 69, 70], 'Math':[66, 75, 74, 88, 94]}
df = pd.DataFrame(dict_student)
print(df)

# 方法一
df_columns1 = pd.DataFrame(df,columns=['ID', 'Sex', 'Math', 'Chinese'])
# 方法二
df_columns2 = pd.DataFrame(dict_student,columns=['ID', 'Sex', 'Math', 'Chinese'])
print(df_columns2)

```

```python
# 輸出
# 原本的資料順序
        ID Sex  Chinese  Math
0  1100101   f       60    66
1  1100102   m       70    75
2  1100103   f       77    74
3  1100104   f       69    88
4  1100105   m       70    94
# 更改後的資料順序 
        ID Sex  Math  Chinese
0  1100101   f    66       60
1  1100102   m    75       70
2  1100103   f    74       77
3  1100104   f    88       69
4  1100105   m    94       70
```

若是要新增新的 column，則可以這樣撰寫：

```python
# 方法一
df_columns_add1 = pd.DataFrame(df,columns=['ID', 'Sex', 'Math', 'Chinese', 'English']
# 方法二
df_columns_add2 = pd.DataFrame(dict_student,columns=['ID', 'Sex', 'Math', 'Chinese', 'English'])
print(df_columns_add2)
```

```python
# 輸出 
        ID Sex  Math  Chinese  English
0  1100101   f    66       60      NaN
1  1100102   m    75       70      NaN
2  1100103   f    74       77      NaN
3  1100104   f    88       69      NaN
4  1100105   m    94       70      NaN
```

可以看到成功新增了一個 English 的 column，接著透過 list 給 English 值：

```python
list_English = [70,88,67,89,97]
df_columns_add2['English'] = list_English
print(df_columns_add2)
```

```python
# 輸出 
        ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
2  1100103   f    74       77       67
3  1100104   f    88       69       89
4  1100105   m    94       70       97
```

## 四、DataFrame 資料資訊

當我們創建完 DataFrame 之後，可以使用 DataFrame 資料結構中的方法或屬性來查看資料的資訊，比較常見的方法如：.shape、.head()、.tail()、.describe()、.index、.columns 。那以下方的 df_student 為例，我們來實際操作看看這些方法。

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

### 1. 查看資料的 row (列數), column (欄數)

當我們要查看 df_student 有幾個 row 和幾個 column 的時候，可以使用 .shape，結果即為 (row, column)：

```python
# .shape
print(df_student.shape)
```

```python
# 輸出
(5, 5)
```

### 2. 查看前面 n 筆資料的值

當我們要查看 df_student 前面幾筆資料的值時，可以使用 .head()。預設為顯示 5 筆，我們也可以在 .head() 裡面代入數值來指定要顯示的筆數，比如查看前 2 筆資料：

```python
# .head()
print(df_student.head(2))
```

```python
# 輸出
        ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
```

### 3. 查看最後 n 筆資料的值

當我們要查看 df_student 最後幾筆資料的值時，可以使用 .tail()。預設同樣為顯示 5 筆，我們可以在 .tail() 裡面代入數值來指定要顯示的筆數，比如查看後 2 筆資料：

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

*要注意的是 .head() 和 .tail() 最多皆只能顯示 5 筆資料

### 4. 查看資料的描述性統計

當我們要查看 df_student 描述性統計的資料時，可以使用 .describe()，透過這個方法我們便可以更方便的了解整個資料的概要：

```python
# .describe()
print(df_student.describe())
```

```python
# 輸出
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

### 5. 查看資料的 index (索引值)

當要查看 df_student 的 index 時，我們可以使用 .index 的屬性：

```python
# .index
print(df_student.index)
```

```python
# 輸出
RangeIndex(start=0, stop=5, step=1)
```

### 6. 查看資料的所有 column (欄位) 名稱

當要查看 df_student 所有的欄位名稱時，我們則可以使用 .columns 的屬性：

```python
# .columns
print(df_student.columns)
```

```python
# 輸出
Index(['ID', 'Sex', 'Math', 'Chinese', 'English'], dtype='object')
```

### 7. 查看資料的內容

若要查看 df_student 的資料內容時，我們可以使用 .info 的屬性：

```python
# .info
print(df_student.info)
```

```python
# 輸出
<bound method DataFrame.info of         ID Sex  Math  Chinese  English
0  1100101   f    66       60       70
1  1100102   m    75       70       88
2  1100103   f    74       77       67
3  1100104   f    88       69       89
4  1100105   m    94       70       97>
```

### 8. 將 row (列) 與 column (行) 進行轉換

DataFrame 除了這些查看資料資訊的方法， 也提供如同矩陣轉置的操作，可以使 .T 的方法將行與列進行交換：

```python
# .T
print(df_student.T)
```

```python
# 輸出
               0        1        2        3        4
ID       1100101  1100102  1100103  1100104  1100105
Sex            f        m        f        f        m
Math          66       75       74       88       94
Chinese       60       70       77       69       70
English       70       88       67       89       97
```

當在我們做資料前處理的時候，經常會透過這些方法來幫助我們更了解整個資料的內容與概況，接著再來執行資料的處理與加工，像是：資料的取出、資料的刪除、合併或排序等等，甚至是空值與異常值的處理等更進階的操作。

那由於本篇文章的篇幅有限，下一篇我們再接著學習 Pandas 的資料前處理及 DataFrame 的操作方法吧 !  

讀到這邊，不知道你們對於 Pandas 有沒有更多的認識呢 ?~