[toc]



# 数据查询

> - 掌握基本查询的使用
> - 掌握限定查询与排序的使用
> - 掌握单行函数、分组函数的使用
> - 掌握多表查询的使用
> - 掌握子查询的使用
> - 理解和掌握集合查询的使用
> - 理解和掌握TopN查询
> - 了解层次化查询

## 基本查询

### 在查询中选定特定的列

### 使用算术表达式

### 使用列的别名

- 列名与列名之间使用as 或 空格，别名可以加双引号，也可以不用。如果使用了双引号，别名的显示结果为引号中的原类型。

### 连接运算符

> 连接运算符由两个竖线''||''表示，可以把一个或多个列或字符串连接在一起，其中字符串需要加单引号。（**列之间的连接**）

### DISTINCT运算符

> 可以去除重复项（一列或者多列的重复，具体看查询写了几个列名）
>
> 当数据量很大时，避免使用distinct

## 限定查询与排序

### where表达式

> where后面可以支持关系运算符、逻辑运算符、BETWEEN...AND范围查询操作符、IN（NOT IN）列表范围查询操作符、NULL值判断操作符、LIKE模糊查询操作符

### NULL

- 它既不是0，也不是空格。它的值是没有定义的、未知的、不确定的。
- NULL值和任意值进行算术运算时，结果仍然为NULL；
- NULL值和任意值进行比较运算时，结果都为UNKNOWN;

### LIKE模糊查询

- %：可以匹配任意类型和长度（0个或多个）的字符。

- _:匹配单个任意字符。常用来限制表达式的字符长度。

- ESCAPE：使用ESCAPE操作符可以指定任意一个字符作为转义字符

  

### 排序

- order by
- ASC代表升序，为默认值
- DESC代表降序
- 多个列或多个表达式排序，首先按照1第一个列或者表达式进行排序，当地一个列或表达式存在相同数据时，然后以第二个列或表达式进行排序。

## 单行函数

> 单行函数同时只能对表中的一行数据进行操作，并且对每一行数据只产生一个输出结果

- 单行函数主要有以下5种：
  - 字符函数：对有字符组成的字符串进行操作
  - 数值函数：对数值进行计算
  - 日期函数：对日期和时间进行处理
  - 转换函数：将一种数据库类型转换成另外一种数据库类型
  - 其它函数
- 函数名[(参数1，1参数2，参数3)]
- 单行函数可以用在select、where、order by子句中
- 单行函数可以嵌套使用

### 字符函数

- upper（列|字符串） ： 将字符串中的内容全部转大写
- lower（列|字符串）： 将字符串中的内容全部转小写
- length（列|字符串）： 返回字符串的长度
- initcap（列|字符串）： 将字符串的开头首字母大写
- substr（列|字符串，开始点【，长度】）： 字符串的截取
- trim（列|字符串）： 去字符串左面空格、右面空格
- instr（列|字符串，要查找的字符串【列|字符串】，要查找的字符串[,开始位置]）：查找字符串的出现的位置

### 转换函数

- TO_CHAR（列|日期|数字，转换格式）： 将指定的数据按照指定的格式转换成字符串

  - 对数字类型数据的格式转换

    - 9 ： 一位数字

    - 0 ： 显示前导0

    - $ :   显示美元符

    - L： 显示本地货币符

    - .  :  显示小数点

    - ，：显示千位符

      > select TO_CHAR(12345,678,'999,999.999'),TO_CHAR(12345，678，'L999,999.999') from dual;

- TO_DATE（列|字符串，转换格式）： 将指定的数据按照指定的转换格式转换成日期型

- TO_NUMBER(列|字符串)： 将指定的数据转换成数字型

  



### 数值函数

- round（列|数字【，保留位数】）：对小数进行四舍五入，可以指定保留位数。
- trunc（列|数字【，截取位数】）： 保留指定位数的小数，若不指定，不保留小数
- mod（列|数字，数字）： 进行取模运算

### 日期函数

- Oracle 12c中，日期默认显示和输入格式是DD-MON-RR
- 可以通过sysdate来获取当前的系统时间
  - select sysdate from dual;
- 可以修改系统参数NLS_DATE_FORMAT来改变当前会话的显示格式
  - alter session set NLS_DATE_FORMAT='yyyy-mm-dd hh24:mi:ss';
  - 可以使用算术运算符对日期进行计算
    - 日期-数字（天数）=日期
    - 日期+数字（天数）=日期
    - 日期-日期 = 数字（天数）
- ADD_MONTHIS（日期，数字）：对指定的日期增加指定的月数，求出新的日期
- LAST_DATE（日期）： 计算指定日期当前月份的最后一天的日期
- NEXT_DAY(日期，星期)： 计算下一个星期的日期，星期可以为‘星期一’到‘星期天’或者数字‘1-7’，其中1代表星期天
- MONTHS_BETWEEN(日期1，日期2)： 计算两个日期间的月数

### 其它函数

- NVL(列，替换值)： 如果指定列的值为null，则返回替换值，否则返回列的值
- NVL2（列，替换值1，替换值2）： 如果指定列的值为null，则返回替换值2，否则替换值1
- NULLIF（表达式1，表达式2）：如果表达式1和表达式2相等则返回空值，不相等则返回表达式1的结果
- DECODE（列值，判断值1，显示结果1，判断值2，显示结果2,.....,默认值）： 相当于条件语句IF进行多值判断。

## 分组函数

- COUNT(*|[DISTINCT]列)： 计算全部记录数
- AVG(列)：计算平均值
- SUM(列)：计算总和
- MAX（列）： 求最大值
- MIN（列）： 求最小值

### COUNT函数

- count（*）和count（列名）的区别
  - count（列名）表示统计列部位NULL的记录行数
  - count（*） 表示统计列的全部记录行数

### AVG函数

- 不会处理NULL值

### SUM函数

- 不会处理NULL值

### GROUP BY

- GROUP BY子句需要写在WHERE子句之后，ORDER BY子句之前。
- **SELECT子句只允许出现在与GROUP BY 子句相同的分组字段和分组函数，其它的非分组字段不能出现；**
- 如果不使用GROUP BY子句，则SELECT子句中只允许出现分组函数，不能出现其他任何字段。
- select ... from ...where ... group by...order by...执行顺序
  1. 执行FROM
  2. 执行WHERE
  3. 执行GROUP子句
  4. 执行SELECT子句
  5. 执行ORDER BY 子句

### HAVING子句

- HAVING 放在 GROUP BY子句之后
- HAVING子句 对组进行筛选



## 多表查询

- 等值连接
- 内连接
- 外连接
- 自连接
- 不等连接

### 等值连接

### 自连接

- 是指在一张表之间的连接查询，主要用于显示上下级关系或层次关系。

### 内连接

- 自连接与等值连接功能相同，都用于返回满足连接条件的记录；

> - ... INNER JOIN  .... ON ...或者WHERE...
>
> - 语法
>
>   - ```sql
>     select table1.column1,...,table2.column1 from table1 [inner]join table2 
>     on   table1.column=table2.column;`
>     ```
>
>   

### 外连接

- 外连接是内连接的扩展，它不仅会返回满足连接条件的所有记录，而且会返回不满足条件的记录。
- 外连接分为3种
  - 左外连接
  - 右外连接
  - 全外连接

#### 左外连接

- 左外连接它不仅会返回满足连接条件的所有记录，而且会返回不满足连接条件的连接操作符左边表的其他行。

- 语法1

  - ```sql
    select table1.column，table2.column from table1 left [outer] join table2 on tabl1.column = table2.column;
    ```

> - 语法2
>
>   - ```sql
>     select table1.column,table2.column from table1,table2 where table1.column = table2.column(+);
>     ```
>
>     <font color ='red'>**不带(+)的相当于左表**</font>
>
>     <font color ='red'>**（+）只能用在where后面，不能用在from后面**</font>
>
>     <font color ='red'>**where后面每一个该表都要加（+）**</font>

- 当使用了左外连接时，左表会全部显示，不管满足不满足条件，而右表只会显示满足条件的数据

#### 右外连接

#### 全外连接

- ...FULL[outer] JOIN...

### 不等链接

## 子查询

- 在where子句中使用的子查询
- 在having子句中使用的子查询
- 在from子句中使用的子查询
- 在select子句中使用的子查询

### 在where中使用子查询

#### 多行单列子查询

- IN|NOT IN 
- \>|<|= ANY :          与子查询返回结果中某一个值大或小或相等。
- \>|< ALL ： 比子查询返回结果中所有值都大或小 
- EXISTS  :    子查询至少返回一行条件为true
- NOT EXISTS： 子查询不返回任何一行时条件为true

#### 多行多列子查询

- ```sql
  select empno,ename,job,deptno,sal from emp
  where(deptno,sal) in(
  select deptno,Max(sal) from emp
  where job!='MANAGER' and job not like 'PRE%' group by deptno
  );
  ```

  

### 在having子句中使用子查询

### 在from子句中使用子查询

- 当子查询返回一个结果集时，它就相当于一个普通的表。

- 由于子查询会被作为临时表，因此需要必须为盖子查询指定别名。

- 例：

  ```sql
  select d.deptno,d.dname,e.amout,e.avgsal
  from dept d,(
  select deptno,count(*) amount,avg(sal) avgsal
  from emp
  group by deptno) e
  where d.deptno  = e.deptno;
  ```

  





## 集合查询

- 集合查询是指是使用集合运算符将多个查询的结果集进行合并。
- UNION、UNION ALL表示并运算
- INTERSECT 表示交运算
- MINUS 表示差运算（补集）
- CROSS 表示笛卡尔积运算
- 使用集合查询使的注意点：
  - 参加集合操作的各查询的结果集必须具有相同的列数与数据类型。
  - 如果要对最终的集合查询结果集排序只能在最后一个查询通过 ORDER BY 排序。

### 并集操作

- 并集操作 UNION 、UNION　ALL
  - UNION（不包括两个结果集重复部分）
  - UNION ALL操作符返回两个查询结果集的并集以及两个结果集重复的部分

### 交集操作

- INTERSECT返回两个结果集的交集。
- MINUS 操作符返回两个结果集的补集

## TopN查询

- 在进行数据查询的时候，如果被查询的表的数据量非常大，通常采用多次分批提取的方式。

- TopN查询即是指筛选显示一个查询结果集的前N行记录。

- 在Oracle数据库中TopN查询通过ROWNUM伪列实现。

- 例：

  - ```sql
    select * from emp where rownum = 1;
    ```

  - ```sql
    select * from emp where rownum = 2;
    ```

    - rownum=2时会出错！

      - 原因：rownum是查询结果自动添加的伪列，并且总是从1开始。

      - 改错：

        - ```sql
          select * from emp where  rownum<3;
          ```

  - <font color='seagreen'>rownum的伪列判断 优先于 order by 排序</font>

## Oracle12c新特性FETCH

- FETCH FIRST rows ROW ONLY表示取得前rows行记录；
- OFFSET startposition ROWS FETCH NEXT num ROW ONLY表示通过指定起始行的位置startposition以及查询行数num，来取得指定范围的记录。
- FETCH NEXT per PERCENT ROW ONLY表示1按照指定的百分比per%取得相关行数的记录；

## 层次化查询