---
title: "MySQL - 基本觀念＆指令操作"
description: ""
date: 2021-06-30
draft: false
categories: 
- MySQL Note
tags:
- MySQL
cover:
    image: "photo/cover.jpeg"
    relative: true
---

### 前言
SQL 為 Structured Query Language 縮寫，念 S.Q.L 或 sequel。 中文稱結構化查詢語言。
<!--more-->
SQL 語法主要分成 **DDL(Data Definition language)**、**DQL(Data Quary language)**
、**DML(Data Manipulation Language)**、**DCL(Data Control Language)**。

而 ＭySQL 是一個開放原始碼的資料庫軟體，我們可以透過 MySQL 中的 SQL 語法來操作 MySQL 的資料庫，
讓我們來認識 MySQL 的基礎語法吧。


## 一、DDL(Data Definition language)

中文表示為資料定義語言，用來定義資料庫、資料表、檢視表、索引、預存程序、觸發程序、函數等資料庫物件。

### 1. 使用 CREATE 建立資料庫/資料表

**建立資料庫**

```sql
create database if not exists school
collate utf8mb4_unicode_ci;
```

**建立資料表**

```sql
use school
```

```sql
create table if not exists stdents(
name varchar(10) not null,
chinese int,
english int,
math int,
id int auto_increment primary key
);
```

> id 設為自動累加，即使刪掉某些 row，id 仍會繼續自動累加

### 2. 使用 ALTER 修改資料表欄位

**資料表新增欄位**

```sql
alter table students add society int;
```

**資料表刪除欄位**

```sql
alter table students drop society;
```

### 3. 使用 DROP 刪除資料庫/資料表

**刪除資料庫**

```sql
drop database if exists school;
```

**刪除資料表**

```sql
drop table if not exists students;
```

### 4. 使用 SHOW 查看資料庫/資料表

**查看資料庫**

```sql
show databases;
```

**查看資料表**

```sql
show tables;
```

**查看資料表所有欄位**

```sql
show columns from students ;
```
![](12.png)

![](13.png)


## 二、DQL(Data Quary language)
中文表示為資料查詢語言，用來進行資料查詢，並不會對資料本身進行修改的語句。

### 1.使用 SElECT 查詢資料

**查看資料表所有欄位的值**

```sql
select * from students;
```

**查看資料表特定欄位的資料** 
- 查看 math 成績

    ```sql
    select math from students;
    ```


**查看特定條件的值**

- 查看分數大於等於 60 的 english 成績

    ```sql
    select english from students where english >= 60;
    ```

- 查看學生 frank 的所有成績：

    ```sql
    select * from students where name = 'frank';
    ```

- 查看 chinese 大於 60，且 math 為 60 的學生：

    ```sql
    select name from students where chinese > 60 and math = 60;
    ```

![](5.png)
![](12.png)



## 三、DML(Data Manipulation Language)

中文表示為資料操作語言，用來處理資料表裡的資料。

### 1. INSERT INTO 插入資料

**insert into 為新增新的 row 的時候使用**

- 插入一筆資料到資料表

    ```sql
    insert into students values('Ace',59,60,93,5);
    ```

-  插入一筆資料到資料表指定欄位

    ```sql
    insert into students(name,chinese,english,math) values(
    'frank',70,50,80);

    ```
    > 如果不給自動累加的 id 值的時候，需要用指定的方式


- 插入多筆資料到資料表指定欄位

    ```sql
    insert into students(name,chinese,english,math) values('andy',75,91,60),('mimi',60,75,92);
    ```

### 2. UPDATE 更新資料

**更新資料表資料**

```sql
update students set id=4 where name='Ace'
```

### 3. DELETE 刪除資料

**刪除單個 row 的值**

```sql
delete from students where name = 'naomi';
```

**刪除多個 row 的值**

```sql
delete from students where name in ('naomi','josh');
```


### 4. Insert Into 與 Update 使用差異
當資料不存在時就 Insert Into（新增)，已存在就 Update（更新）。

**首先，我們先新增一個 column 為 gender，並設定為 varchar(6)**

```sql
alter table students add gender varchar(6);
```



**接著使用 update更新 gender 欄位的資料，使用範例:**

```sql
update students set gender = 'm' where name = 'frank';
```
>因為原本的 row 已經有資料，不能使用 insert into 來新增 gender 的值，應該使用 **update** 來新增 gender 的值


![](6.png)



**若一次 update 多個 gender 的值的時候，使用範例：**

```sql
update students set gender = case name when 'frank' then 'm' when 'andy' then 'm' when 'mimi' then 'f' when 'Ace' then 'm' else gender end;
```
>使用 gender = case name when ... then ...
>若在不更動 naomi、josh 的 gender 值進行 update 記得要加 else gender，否則 naomi、josn 的值會變成null。
>結束需加上 end;

![](9.png)

**insert into 適用於資料不存在的情況，如新增新的 row 的情況，使用範例：** 

```sql
insert into students(name,chinese,english,math) values('naomi',79,84,93,'f'),('josh',8,64,73,'m');
```

![](7.png)



**在 update 的使用上有一點值得注意的地方，當我們新增 age 為 int、hobby 為 varchar(100) 並且皆為 not null 的欄位：**

```sql
alter table students add age int not null;
```

```sql
alter table students add hobby varchar(100) not null;
```

![](10.png)

**update 使用範例：**

```sql
update students set age = case name when 'frank' then 21 else age end;
```
>該欄設定 not null 的情況如果要 update  部分的值，必須加上else xxx 否則會報錯

![](12.png)


### 輔助指令

### 1. ORDER BY 資料排序

**將欲查詢的資料進行排序**

- 預設為小到：**ASC**

    ```sql
    select * from students order by math;
    ```


- 由大到小：**DESC**

    ```sql
    elect * from students order by math desc;
    ```


### 2. GROUP BY 資料分群

**將欲查詢的資料進行分組**

```sql
select gender,count(*) from students group by gender;
```

![](17.png)

![](12.png)


### Aggregate Function 函式

**常見函式如 count、sum、max、min、age**

- count

    ```sql
    select count(*) from students;
    ```

- sum

    ```sql
    select sum(math) from students;
    ```


- min

    ```sql
    select min(chinese) from students;
    ```


- max

    ```sql
    select max(english) from students;
    ```


- avg

    ```sql
    select avg(english) from students;
    ```

![](22.png)

## 四、DCL（Data Control Language）

### 1. GRAN 授權使用者

[WITH GRANT OPTION]：如果在 GRANT 命令末尾包含可選的「與 GRANT 選項」條款，不僅授予指定使用者 SQL 語句中定義的許可權，還允許使用者進一步授予其他資料庫使用者相同的許可權。

```
GRANT [privilege]
ON [object]
TO [user]
[WITH GRANT OPTION]
```

```sql
grant select
on school.students
to Andy
```

### 2. REVOKE 取消使用者權限

[GRANT OPTION FOR]：條款的授予選項刪除指定使用者授予其他使用者指定許可權的能力。如果您在撤銷聲明中包含「授予選項」條款，則主要許可權不會被撤銷。本條款僅撤銷授予能力。

[CASCADE]：CASCADE選項會撤銷指定使用者授予該許可權的任何使用者的指定許可權。

```sql
revoke [grant option for] [permission]
on [object]
from [user]
[cascade]
```

```sql
revoke select
on school.students
from Andy
```
![](12.png)

### 3. COMMIT 將操作更新到資料庫

**檢視 AUTOCOMMIT 狀態**

```sql
select @@autocommit;
```

![](23.png)

**AUTOCOMMIT 變數預設設定為 true 。這可以通過以下方式更改**

- 設定為 false：

    ```sql
    set autocommit=false; ／ set autocommit=0;
    ```

- 設定為 true：

    ```sql
    set autocommit=true;  /   set autocommit=1
    ```

**如果 AUTOCOMMIT 設定為 false 且未 commit，則只有當前連線可以看見更新**

```sql
# 目前連線
insert into testTable values (1);
set autocommit=false;
insert into testTable values (2), (3);
select * from testTable;
```

```sql
# 其他連線
select * from testTable;
```

![](24.png)

![](25.png)

**但若進行 commit 則目前連線和其他連線皆可以看見更新**

```sql
# 目前連線
commit;
select * from testTable;
```

```sql
# 其他連線
select * from testTable;
```

![](26.png)

![](27.png)

### 4. ROLLBACK 取消對資料庫的操作

**commit 前，使用 rollback 可以改變**
```sql
insert into testTable values (1);
set autocommit=false;
insert into testTable values (2), (3);
select * from testTable;
```

![](28.png)

```sql
rollback;
select * from testTable;
```

![](29.png)

```sql
insert into testTable values (2), (3);
select * from testTable;
commit;
```

![](12.png)


**commit 後使用 rollback 無法改變**
```sql
rollback;
select * from testTable;
```
>如果 AUTOCOMMIT 設定為 true ，那麼 COMMIT 和 ROLLBACK 就沒用了

### 5. DENY 明確拒絕資料庫訪問

**DENY 命令明確阻止使用者接收特定許可權**

```sql
deny [permission]
on [object]
to [user]
```

```sql
deny delete
on school.studends
to Blackmaple
```


## 結論
大家可以動手試試才能真正了解整個資料庫的使用噢。