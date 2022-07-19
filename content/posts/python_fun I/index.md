---
title: "Python - Function 入門的重要概念"
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

##  一、函式的基本認識

函式是程式中的**子程式**或**副程式**。概念有點類似於數學上的函數，它負責處理程式碼中的某項功能。

<!--more-->

像是我們在學習 Python 時，最常接觸的 print() 其實就是一種函式。print() 是屬於 Python 中的內建函式，而我們也可以自己定義的函式來創建一個我們想要執行的一段程式或功能。

這樣一來就方便我們在需要這段程式的時候，執行這個函式來處理，不僅能夠減少重複的程式碼，也能使我們的程式看起來更結構化，若之後要修改程式碼也會比較方便。

## 二、自定義函式

那我們該怎麼自定義一個函式呢，我們可以先來看看函式的組成結構：

```python
def 函式名(参數1，参數2，...參數n):
    函式內容
    return 回傳值
```

在函式中我們使用 **def**  開頭，後面接續函式名稱，如果有參數可以在 **()** 中加入參數名稱，並以**冒號**來分隔，在冒號後換行並縮排即可定義函式的程式內容，最後以 **return** 來回傳程式執行的結果，我們稱結果為回傳值。

在一個函式中，參數相當於函式的 **input** (輸入)，而回傳值則為函式的 **output** (輸出)。

執行函式的行為則稱為「函式呼叫」，呼叫函式時，我們可以藉由撰寫函式名稱來執行函式，如果有參數則需要給予參數。參考範例：

```python
def calc_add(x,y):
	return x + y		

calc_add(4,9)
```

```python
輸出：
9
```

## 三、函式的變數範圍


在程式語言中，變數皆有一個有效範圍，根據變數所在的位置，會影響所存取的值，可分為：「全域變數」及「區域變數」。

在 Python 中，全域變數只要在同一份 Python 檔中皆可存取，而區域變數是只有在函式的範圍中才可以進行存取的值，函式以外的地方無法存取。

可以參考下方範例，在函式的範圍內會印出區域變數的 x 值，而在函式以外的地方則是印出全域變數的 x 值：

```python
# 全域變數
x = 100

def calc_num():
	# 區域變數
	x = 20
	return x

print(calc_num())    
print(x)  

```

```python
輸出：     
20           # 印出區域變數 x = 20
100          # 印出全域變數 x = 100
```

如果想要透過函式來取得全域變數的值，則可以在函式中使用「global」，參考範例：

```python
# 全域變數
x = 100

def calc_num():
	global x
	return x

print(calc_num())    
print(x)  
```

```python
輸出：     
100          # 印出全域變數 x = 100
100          # 印出全域變數 x = 100
```

如果想要透過函式來修改全域變數的值，同樣可以在函式中使用「global」並進行修改，會連同全域變數一併被修改，參考範例：

```python
# 全域變數
x = 100

def calc_num():
	global x
	# 區域變數
	x = 20
	return x

print(calc_num())    
print(x)  
```

```python
輸出：     
20          # 印出區域變數 x = 20
20          # 印出區域變數 x = 20
```

> 非必要的時候，避免在函式中修改全域變數的值，因為永遠不會知道程式的其他地方有沒有使用了這個全域變數來進行運算，而在函式中修改了它的值後，很容易導致程式的 Side Effect (副作用)或 Bug (錯誤)。

## 四、函式返回值

函式需要 return 值，如果在函式最後沒有加 return 只會執行完函式裡的內容並回傳為 None，
參考範例：

```python
def calc_num():
	x = 20

print(calc_num())    
```

```python
輸出：     
None         # 因為沒有 return 值
```

## 五、函式參數

函式的參數類型主要有三種，分別是默認參數、關鍵字參數、不定量參數。

一般來說，我們在呼叫函式，通常會傳值給函式參數當作參考，如下：

```python
def say_hello(name):
    print('Hello '+ name);
    
say_hello('Anila')
```

```python
輸出：
Hello Anila
```


### 1. 默認參數

若呼叫函式時沒有傳值時，我們可以透過默認參數提供給函式參考，也就是給函式預設值，在沒有傳值的清況下，函式在會根據預設值來執行。

```python
def say_hello(name = 'Andy'):
    print('Hello '+ name);
    
sayhello()
```

```python
輸出：
Hello Andy
```

### 2. 關鍵字參數

有時候，我們定義的函式希望某些參數強制使用關鍵字參數傳遞，透過關鍵字參數就可以將我們要傳進去的值指定參數名稱，這樣一來我們也可以不用按順序來輸入參數的值。

```python
def say_hello(name,age):
    print('Hello '+ name + ' you are '+ age + ' years olds.');

say_hello(age = '18', name='Anila')
```

```python
輸出：
Hello Anila you are 18 years olds.
```

### 3. 不定量參數 *args 及 **kwargs

有的時候，我們在設計函式時，不確定要傳入函式的參數數量，當輸入多個不確定數量的參數時，則可以在參數名稱前加上「 *」，便可以一次傳入多個參數，而結果會以 tuple 的形式輸出。

```python
def info(*temp):
		print(temp)

info('Toby','Andy','Anila')
```

```python
輸出：
('Toby','Andy','Anila')
```

我們也可以使用「 **」 來將參數資料轉成 Dictionary 的型態。

```python
def info(**temp):
		print(temp)

foo(name = 'Toby',age= 2)
```

```python
輸出：
{'name':'Toby', 'age':2}
```

### 4. 函式傳值的概念

Python 值的傳遞主要基於 pass by reference 的作法來實現，也就是說傳遞物件的參照，而這些被傳遞的參照物件有些資料型態是可變的，有些資料型態則是不可變的。

可變的資料型態如：list、array，不可變的資料型態(基本的資料型態) 如：int、string 等，

可變的資料型態與不可變的資料型態傳遞值給函式進行簡單的運算時，都不會改動到原本參考的值，但若可變的資料型態傳遞值給函數時使用本身的方法來改變本身的值，則會連同原本參考的值一起被改變。

不可變的資料型態 int 傳遞值給函式進行簡單的運算時不會改變原本參考的值，參考範例：

```python

x = 100
def calc_num(x):
		result = x*2
		return result

print(calculate_number(x))
print(x)
```

```python
輸出：
200        # 印出經函式運算過的變數 x = 200
100        # 印出原本的變數 x = 100
```

可變的資料型態 list 傳遞值給函式進行簡單的運算不會改變原本參考的值，參考範例：

```python
x = [1,2,3,4]
def calc_num(x):
		result = x*2
		return result

print(calculate_number(x))
print(x)
```

```python
輸出：
[1, 2, 3, 4, 1, 2, 3, 4]
[1, 2, 3, 4]
```

可變的資料型態 list 傳遞值給函式時使用本身的方法(直接改變本身)會改變原本參考的值：

```python
x = [1,2,3,4]
def calc_num(x):
    x.append(5)
    return x
    
print(calc_num(x))
print(x)
```

```python
輸出：
[1,2,3,4,5]
[1,2,3,4,5]

＊注意：append() 不會回傳值，而是直接改變 x 這個 list，
	錯誤寫法： x = x.append，x 會變成 None
	正確寫法：直接使用 x.append，來改變 x 本身
```

### 5. 補充

若沒有給於函式裡變數的參考值，函式會根據最近的階層來拿值：

```python
x = 100
def calc_num():
		result = x*2
		return result

print(calc_num())
print(x)
```

```python
輸出：
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
輸出：
[1,2,3,4,5]
[1,2,3,4,5]
```

＊注意：在函式使用時，盡量避免函式內與和函式外有重複的變數名稱而搞混

## 六、匿名函式

最後來介紹函式裡的匿名函式，所謂的匿名函式就是不需要定義函式的名稱，將函式的內容直接撰寫在要執行的地方。

在 Python 中，使用「lambda」來創建匿名函式，並在 lambda 後方接續參數名稱及執行的內容，即可給予參數的值執行來執行函式的內容，參考範例：

```python
#寫法一
sum = (lambda x,y:x+y)
print(sum((4,5)))

# 寫法二
print((lambda x,y:x+y)(4,5))
```

```python
輸出：
9
9
```

在這個範例中，「lambda x, y: 」相當於函式名稱後的 (x, y)，並以「:」 區隔函式內容，並return x+y，這便是逆函式的基本寫法。

在匿名函式中有幾點需要特別注意：

- lambda 只是一個表達式，並不是一個程式碼區塊，因此只能封裝有限的邏輯進去

- lambda 函式擁有自己的命名空間，不能訪問自己參數列表以外或全域命名空間裡的參數。

## 結論

函式是在程式撰寫中非常好用的功能，尤其在撰寫比較複雜的專案時非常適合使用，不僅擁有更大的彈性調整程式，也能更清楚明瞭的了解正個專案的架構，那這次就先介紹到這邊。希望這篇文章可以幫助正在學習程式的你。一起加油！