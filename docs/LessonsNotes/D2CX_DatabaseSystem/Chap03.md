---

title: Chap 3 | SQL 

hide:
  #  - navigation # 显示右
  #  - toc #显示左
  #  - footer
  #  - feedback  
comments: true  #默认不开启评论

---
<h1 id="欢迎">Chap 3 | SQL </h1>
!!! note "章节启示录"
    本章节主要涉及SQL相关的语句编写。其中的内容和Chap02的关系代数表达式很像，可以说基本一致了，难度上也是很大，但有一些细节需要注意，而且需要多加练习才可以熟练掌握。

## 1.Data Definition Language(DDL) 

### 1.1 SQL 中的域类型
!!! abstract "域类型"
    * $\large char(n)$:固定长度的字符串，具有用户指定的长度。
    * $\large varchar(n)$:可变长度字符串，用户指定的最大长度为 n。
    * $\large int$:整数（整数的有限子集，与计算机相关）。
    * $\large smalint$:小整数（整数域类型的与计算机相关的子集）。
    * $\large numeric(p,d)$:定点数，用户指定的精度为 p 位，小数点右侧为 d 位。
    * $\large real,double precison$：浮点数和双精度浮点数，具有与机器相关的精度。
    * $\large float(n)$:浮点数，用户指定的精度至少为 n 位。
    * $\large Null values$:在所有域类型中都允许。将属性声明为非 null 将禁止该属性的 null 值。
    * $\large date$:日期，包含（4 位数字）年、月和日期。
    >e.g:date ‘2007-2-27’  
    * $\large Time$:一天中的时间，以小时、分钟和秒为单位。
    >e.g:time ‘11:18:16’, time ‘11:18:16.28’ 
    * $\large timestamp$:日期加上一天中的时间。
    >e.g:timestamp ‘2011-3-17 11:18:16.28’ 
### 1.2 Create Table
Create table command:  
$$
CREATE\;\; TABLE \;\; r(A_1,D_1,A_2,D_2,……,A_n,D_n,(integrity constraint_1),……,(integrity constraint_k))
$$

* $r$ 是关系的名称  
* 每个 $A_i$ 都是关系 $r$ 架构中的一个属性名称  
* $D_i$ 是属性 $A_i$ 域中值的数据类型

>e.g:  
声明 branch_name 作为分支的主键，并确保资产的值为非负数。  
**Method1:**  
CREATE TABLE *branch*(*branch_name* char(20) not null,*branch_city* char(30),assets integer,primary key(*branch_name*),check(assets>=0));  
**Method2**:  
CREATE TABLE branch2(*branch_name* char(20) primary key,*branch_city* char(30),assets integer,check(assets >= 0)); 

### 1.3 Drop and Alter Table 
Drop Table command:  
$$
DROP\; TABLE\;\; r
$$
>e.g:
DROP TABLE branch2  

Alter Table command:
$$
ALTER\;\; TABLE\;\; r\;\; ADD\;\; A\;\; D; 
$$
$$
ALTER\;\; TABLE\;\; r\;\; ADD\;\; (A_1,D_1,……,A_n,D_n); 
$$

* A 是要添加到关系 r 中的属性的名称
* D 是 A 的域。

