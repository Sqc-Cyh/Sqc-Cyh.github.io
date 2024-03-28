---

title: Chap 4 | Advanced SQL 

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---
<h1 id="欢迎">Chap 4 | Advanced SQL </h1>
!!! note "章节启示录"
    本章节主要涉及高级SQL相关的语句编写。理解上难度不大，但要熟练运用，还是有难度的！。

## 1.SQL Data Types and Schemas 
### 1.1 User-defined types 
```SQL
Create type person_name as varchar (20)   
Create table student   
            (sno char(10) primary key,   
            sname person_name,   
            ssex char(1),   
            birthday date)   
Drop type person_name   
```

### 1.2 Domains

``` SQL
Create domain Dollars as numeric(12, 2) not null; 
Create domain Pounds as numeric(12,2); 
Create table employee 
            (eno char(10) primary key, 
            ename varchar(15), 
            job varchar(10), 
            salary Dollars, 
            comm Pounds); 

```

!!! warning "对比"
    与 TYPE 不同的是，DOMAIN 是在现有基本类型的基础上添加了约束和限制的新类型，它是对现有类型进行包装的一种方式。DOMAIN 允许我们在不修改底层数据类型的情况下，根据数据类型的特定需求创建新类型。

### 1.3 Large-object types 
大型对象（例如，照片、视频、CAD 文件等）将以大型对象的方式存储：

* blob: 二进制大型对象 （Binary Large Object），对象是大量未解释的二进制数据（其解释留给数据库系统外部的应用程序）
* clob: 字符大对象 -- 对象是字符数据的大型集合，当查询返回大型对象时，将返回指针，而不是大型对象本身。

!!! info "BLOB in MySQL"
    * TinyBlob ： 0 ~ 255 bytes.
    * Blob： 0 ~ 64K bytes.
    * MediumBlob ： 0 ~ 16M bytes.
    * LargeBlob : 0 ~ 4G bytes.

## 2.Integrity Constraints 
完整性约束：确保当数据库的授权更改时，不会导致数据一致性的丢失。

### 2.1 Constraints on a single relation 

* Not null
* Primary key 
* Unique
* Check (P), where P is a predicate   

前三者之前已经介绍过，这里我们重点来看第四种约束方式。  
Check command:
```SQL
Create table branch2 
            (branch_name varchar(30) primary key, 
            branch_city varchar(30), 
            assets integer not null, 
            check (assets >= 100)) 

```
* SQL-92 中的 check 子句允许限制域.
>e.g:  
Create domain hourly-wage numeric(5, 2)   
Constraint value-test check(value > = 4.00) 

* 子句约束值测试是可选的;用于指示更新违反了哪个约束。
### 2.2 Assertions
Assertion是一个谓词，表示我们希望数据库始终满足的条件。它将对**多个关系**进行**复杂的**检查条件！  

Assertion command:  
```SQL
CREATE ASSERTION <assertion-name> 
        CHECK <predicate>; 
```
创建Assertion时，系统会对每个更新进行测试。（当谓词为 true 时，它是 OK，否则报告错误。）

>e.g:  
每个分行的所有贷款金额的总和必须小于该分行所有账户余额的总和。
```SQL
CREATE ASSERTION sum-constraint CHECK 
                (not exists (select * from branch B 
                where (select sum(amount) from loan 
                        where loan.branch-name = B.branch-name) 
                            > (select sum(balance)from account 
                        where account.branch-name = B.branch-name))) 

```

### 2.3 Triggers
Triggers是系统自动执行的语句，在数据库数据被修改时会自动触发，并执行预先设定好的程序。  
要设计Trigger的机制，我们必须设计以下两点：  

* 指定执行触发器的条件。
* 指定触发器执行时要执行的操作。

>e.g:  
假设银行不允许负账户余额，而是通过（以下操作）处理透支：  
1.将帐户余额设置为零  
2.创建透支金额的贷款，为该贷款提供与透支账户的帐号相同的贷款编号
那么执行触发器的条件就是：对账户关系的更新导致了余额值为负值。
```SQL
CREATE TRIGGER overdraft-trigger after update on account 
    referencing new row as nrow for each row 
    when nrow.balance < 0 
        begin atomic 
        insert into borrower 
            (select customer-name, account-number from depositor 
             where nrow.account-number = depositor.account-number) 
            insert into loan values 
            (nrow.account-number, nrow.branch-name, – nrow.balance) 
            update account set balance = 0 
            where account.account-number = nrow.account-number 
        end 

```

!!! abstract "Trigger的相关性质"
    * 触发事件可以是插入、删除或更新。  
    * 更新时的触发器可以限制为特定属性。
    >e.g:  
    Create trigger overdraft-trigger   
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;after update of balance on account

    * 可以引用更新前后的属性值。
    >Referencing old row as: for deletes and updates.   
    Referencing new row as: for inserts and updates.

    * 触发器不能用于直接实现外部操作。
  
* 总结一下提到的三个操作：Check,Assertion,Trigger。

| **Check** | **Assertion** |**Trigger**|
| ----------- | ----------- | ---------- | 
| 单表中做简单判断|可以做复杂判断操作|可以做复杂判断操作|
| 性能较好| 效率较低| **？（应该比Check性能要差一些）**|
|只能阻止|只能阻止|可以做额外操作|

## 3.Authorization
这里我们主要研究针对DATABASE的授权权限。 

!!! abstract "对数据库各部分的授权形式:"
    * Read authorization：允许读取，但不允许修改数据。
    * Insert authorization：允许插入新数据，但不允许修改现有数据。
    * Update authorization：允许修改，但不允许删除数据。
    * Delete authorization：允许删除数据。
  
!!! abstract "修改数据库架构的授权形式:"
    * Index authorization：允许创建和删除索引。
    * Resources authorization：允许创建新关系。
    * Alteration authorization：允许添加或修改关系中的属性。
    * Drop authorization：允许删除关系。

* 授予权限：从一个用户到另一个用户的授权传递可以用授权图来表示。  
  1.此图的节点是用户。  
  2.图形的根是数据库管理员。  
  3.$U_i \longrightarrow U_j$表示用户 $U_i$ 已向 $U_j$ 授予了权限。    

![](./img/52.png)

