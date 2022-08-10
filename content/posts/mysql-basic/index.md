---
title: "MySQL - Basic Concepts & Command Operations"
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

## Foreword
SQL is short for Structured Query Language, pronounced S.Q.L or sequel. Chinese is called Structured Query Language.

<!--more-->

SQL syntax is mainly divided into **DDL(Data Definition Language)**, **DQL(Data Query language)**
, **DML(Data Manipulation Language)**, **DCL(Data Control Language)**.

And MySQL is an open source database software, we can operate the MySQL database through the SQL syntax in MySQL,
Let's get to know the basic syntax of MySQL.


## DDL(Data Definition Language)

DDL is used to define database objects such as database, data table, view table, index, pre-stored program, trigger program, and function.

### 1. Use CREATE to create a database/table

**Create a database**

```sql
create database if not exists school
collate utf8mb4_unicode_ci;
```

**Create a table**

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

> id is set to be automatically accumulated, even if some rows are deleted, the id will continue to be automatically accumulated

### 2. Use ALTER to modify data table fields

**Add a new field to the table**

```sql
alter table students add society int;
```

**Delete field from table**

```sql
alter table students drop society;
```

### 3. Delete database/table with DROP

**Delete database**
```sql
drop database if exists school;
```

**Delete table**

```sql
drop table if not exists students;
```

### 4. Use SHOW to query the database/table

**Query database**

```sql
show databases;
```

**Query table**

```sql
show tables;
```

**Query all fields in the table**

```sql
show columns from students;
```


## DQL(Data Query Language)
DQL is used to query data without modifying the data itself,usually using SElECT to query data.

**Query data for all fields in the table**

```sql
select * from students;
```

**Query data for specific field in the table** 
- query data in the math field

    ```sql
    select math from students;
    ```


**Query data under specific conditions**

- Query data with scores greater than or equal to 60 in the english field

    ```sql
    select english from students where english >= 60;
    ```

- Query data for all fields whose name field is frank:

    ```sql
    select * from students where name = 'frank';
    ```

- Query the names of students whose chinese field is greater than 60 and math is 60:

    ```sql
    select name from students where chinese > 60 and math = 60;
    ```



## DML(Data Manipulation Language)

DML is used to process the data in the table.

### 1. INSERT INTO

**Insert into is used when adding a new row**

- Insert a piece of data into the table.

    ```sql
    insert into students values('Ace',59,60,93,5);
    ```

-  Insert a piece of data into the specified field of the table.

    ```sql
    insert into students(name,chinese,english,math) values(
    'frank',70,50,80);

    ```
> If you do not give the automatically accumulated id value, you need to use the specified method.

- Insert multiple data into the specified field of the table.

    ```sql
    insert into students(name,chinese,english,math) values('andy',75,91,60),('mimi',60,75,92);
    ```

### 2. UPDATE

**Update the data in the table**

```sql
update students set id=4 where name='Ace'
```

### 3. DELETE

**Delete data of a single row**

```sql
delete from students where name = 'naomi';
```

**Delete multiple rows of data**

```sql
delete from students where name in ('naomi','josh');
```


### 4. Differences between Insert Into and Update
Insert Into when the data does not exist, and Update when it already exists.

**First, we add a column for gender and set it to varchar(6)**

```sql
alter table students add gender varchar(6);
```

**Then use update to update the data in the gender field, example:**

```sql
update students set gender = 'm' where name = 'frank';
```
>Because the original row already has data, you cannot use insert into to add the value of gender, you should use **update** to add the value of gender

**When updating multiple gender values at a time, example:**

```sql
update students set gender = case name when 'frank' then 'm' when 'andy' then 'm' when 'mimi' then 'f' when 'Ace' then 'm' else gender end;
```
>use `gender = case name when ... then ...`

>If you do the update without changing the gender values of naomi and josh, remember to add else gender, otherwise the values of naomi and josn will become null.

>End with end;


**Insert into is suitable for situations where data does not exist, such as adding a new row, example:**

```sql
insert into students(name,chinese,english,math) values('naomi',79,84,93,'f'),('josh',8,64,73,'m');
```

**There is one thing worth noting in the use of update, when we add a field with age as int, hobby as varchar(100) and all of them are not null:**

```sql
alter table students add age int not null;
```

```sql
alter table students add hobby varchar(100) not null;
```

**Update usage example:**

```sql
update students set age = case name when 'frank' then 21 else age end;
```

>If the column is set to not null and the value of the update part must be added, else xxx must be added, otherwise an error will be reported.


### Auxiliary command

### 1. ORDER BY - sort the data to be queried

- **ASC:** the default form small to large

    ```sql
    select * from students order by math;
    ```


- **DESC:** sort from big to small

    ```sql
    elect * from students order by math desc;
    ```


### 2. GROUP BY - grouping the data to be queried

```sql
select gender,count(*) from students group by gender;
```


### Aggregate Function

**Common functions such as count, sum, max, min, age**

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


## DCL（Data Control Language）

### 1. GRAN - authorized User

`[WITH GRANT OPTION]` If an optional **with GRANT OPTION** clause is included at the end of the GRANT command, it not only grants the specified user the permissions defined in the SQL statement, but also allows the user to further grant the same permissions to other database users.

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

### 2. REVOKE - cancel user privileges

`[GRANT OPTION FOR] `
The grant option of a term removes the ability of a given user to grant given permissions to other users. If you include a "grant option" clause in your revocation statement, the primary permission will not be revoked. This clause only revokes the conferred capacity.

`[CASCADE]` The CASCADE option revokes the specified permission from any user that the specified user grants that permission.

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

### 3. COMMIT - update operation to database

**Query AUTOCOMMIT status**

```sql
select @@autocommit;
```


**The AUTOCOMMIT variable is set to true by default. This can be changed in the following way:**

- set to false

    ```sql
    set autocommit=false; ／ set autocommit=0;
    ```

- set to true

    ```sql
    set autocommit=true;  /   set autocommit=1
    ```

**If AUTOCOMMIT is set to false and not committed, only the current connection will see the update**

```sql
# currently connected
insert into testTable values (1);
set autocommit=false;
insert into testTable values (2), (3);
select * from testTable;
```

```sql
# other connections
select * from testTable;
```

**But if committed, the current connection and other connections can see the update**

```sql
# currently connected
commit;
select * from testTable;
```

```sql
# other connections
select * from testTable;
```

### 4. ROLLBACK - cancels the operation on the database

**Before commit, you can use rollback to change**
```sql
insert into testTable values (1);
set autocommit=false;
insert into testTable values (2), (3);
select * from testTable;
```


```sql
rollback;
select * from testTable;
```


```sql
insert into testTable values (2), (3);
select * from testTable;
commit;
```



**Cannot use rollback to change after commit**
```sql
rollback;
select * from testTable;
```
>If AUTOCOMMIT is set to true , then COMMIT and ROLLBACK are useless

### 5. DENY - explicitly deny database access

**The DENY command explicitly prevents the user from receiving certain permissions**

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
