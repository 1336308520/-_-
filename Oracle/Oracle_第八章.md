[toc]



# 第八章 PL/SQL

<img src="C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_1.png" alt="image-20200303151232140" style="zoom:150%;" />

## pl/sql简介

```plsql
declare
v_empno number;
v_ename varchar2(10);
begin
	DBMS_OUTPUT.put_line("请输入员工编号:");
	v_empno:=&input_empno;
	select ename into v_ename from emp where empno = v_empno;
	DBMS_OUTPUT.put_line('编号为：'||v_empno||'姓名为'||v_ename);
exception
  when no_data_found then
     DBMS_OUTPUT.put_line('此编号的员工不存在');
 end;
	
```

## 	PL/SQL匿名块

- pl/sql程序的基本单元是语句块，所有的pl/sql程序都是由语句块构成的，每个语句块可以完成特定的功能。

- pl/sql语句块分为两类：匿名块，命名块。

- ```plsql
  [declare]
   -声明部分：定义变量、数据类型、异常、局部子程序等。
   BEGIN
   -执行部分：实现块的功能
   [EXCEPTION]
   -异常处理部分： 处理程序执行过程中产生的异常
   END；
   /    “/”表示立即执行此匿名语句块。
  ```

  

- 如果要查看DBMS_OUTPUT.put_line（）方法输出的结果信息，需要使用“set serveroutput on”（或sql developer）的环境变量serveroutput的值设置为on；

- pl/sql是一种强类型的编程语言，所有变量都必须先声明再使用；

  - 变量的声明再DECLARE部分进行

    - ```plsql
      declare
        变量名称[constant]类型[not null][:=value];
        ...
        begin
      ```

      

  - constant不省略表示定义常量，此时必须在声明时为其赋默认值；

  - not null表示此变量不允许设置为null，称为非空变量；非空变量在声明时必须赋初始值；

## 变量与常量

### 常量

- 字符型文字： 以单引号引起来的字符串，在字符串中的字符区分大小写。
- 数字型文字
- 布尔型文字
- 日期型文字

### 基本数据类型

- 几种常用的数据类型
  - 数字类型
  - 字符类型
  - 日期类型
  - 布尔类型
  - %type与%rowtype类型
  - 记录类型

#### 数字类型

- 包括NUMBER.....

#### 字符类型

- CHAR、VARCHAR2、ROWID
  - ROWID
    - 表示表中每行记录的唯一物理地址，与ROWID伪列功能相同；

#### 日期类型

- DATE、TIMESTAMP

#### 布尔类型

- TRUE、FALSE、NULL

#### %TYPE与%ROWTYPE类型

- PL/SQL程序中的变量最常被用来存储数据库表中的数据，此时，变量需要具有与列类型相同的数据类型；

- PL/SQL提供了 %TYPE 类型用来表示表重某一列的类型；

- %ROWTYPE 类型用来表示表中一行记录的类型

- ```plsql
  declare
  v_name emp.ename%TYPE;
       v_name的类型时 emp表中ename的类型
  ```

  

- 所引用的数据库列的数据类型可以实时改变

- ```plsql
  declare
   emp_record emp%ROWTYPE
   begin
   select * into emp_record from emp where empno = &empno;
  ```

  

#### 记录类型

- PL/SQL提供了一种类似于c语言中的结构体类型，其中包含若干个成员分量，称为记录类型。

- 记录类型完全由用户自己来定义结构，属于用户自定义数据类型。

- ```plsql
  TYPE 类型名称 is record（
  成员名称 数据类型【【not null】【：= 默认值】表达式】
  ...
  );
  ```

  

```plsql
declare 
type emp_type is record(
   ename scott.emp.ename%TYPE,
   job scott.emp.job%TYPE	,
   hiredate scott.emp.hiredate%TYPE,
   sal scott.emp.sal%TYPE
);
v_emp emp_type; --定义记录类型变量
begin
 select ename,job,hiredate,sal into v_emp
 from scott.emp where empno = 7369;
 end;
```

## PL/SQL控制结构

- 选择结构
  - IF语句和CASE语句，用于进行条件判断；
- 循环结构
  - LOOP循环和FOR循环
- 跳转结构
  - 指利用GOTO语句实现程序流程的强制跳转；

### 选择结构

- 有3类语法格式，分别为if、if...else、if...elsif...else

- 语法

  - ```plsql
    if 判断条件1 then
              满足条件1时执行的语句；
     【elsif 判断条件2 then】
            满足条件2时执行的语句；
        else
             所有条件都不满足时执行的语句；
         end if；
    ```

    

#### CASE语句

- 有两种形式，一种是进行等值比较；另一种是进行多条件比较；

- 语法

  ```plsql
  CASE[变量]
      when [值|表达式] then
         执行语句块
       when[值|表达式]  then
         执行语句块
         ...
         [else
          所有when条件都不满足时执行语句块   
         ]
    end case;
  ```

  

### 循环结构

- 循环结构有两种

  - LOOP循环和FOR循环，和其他程序语言中的WHILE循环和FOR循环类似

  - LOOP循环又分为LOOP和WHILE...LOOP两种结构，类似于程序语言中的DO...WHILE和WHILE结构。

  - LOOP循环结构的语法

    - ```plsql
      LOOP
         循环执行的语句块
         EXIT WHEN循环结束条件；
         循环结束条件修改；
       END LOOP;
      ```

      

  - WHILE...LOOP循环结构的语法

    - ```plsql
      WHILE(循环结束条件) LOOP
          循环执行的语句块;
          循环结束条件修改;
      END LOOP;
      ```

  - FOR循环结构的语法

    - ```plsql
      FOR 循环变量 IN [REVERSE] 循环变量下界..循环变量上界 LOOP
         循环体;
       END LOOP;
      ```

      

    - 循环变量不需要显示定义，系统隐含将其声明为BINARY_INTEGER变量；

    - 循环变量默认从下界往上界递增计数，若reverse则表示循环变量从上界向下界递减计数；

    - 循环变量只能在循环体中使用

    - 在正常循环的操作中如果要结束循环或退出当前循环，可以使用EXIT与CONTINUE语句来完成；

    - EXIT语句会强制性地结束整个循环体，继续执行循环体之后地程序；

    - CONTINUE语句仅仅会结束当次循环，不会退出整个循环体，当次循环中continue语句之后的代码不再执行；

### 跳转结构

- 跳转结构指利用GOTO语句实现程序流程的强制跳转；
- GOTO语句为无条件转移语句，可以直接转移到指定标号内；
  - 注意事项
    - GOTO语句可以从内层块跳转到外层块，但不能从外层块跳转到内层块
    - GOTO语句不能从IF语句外部跳转到IF语句内部、不能从循环体外部跳转到循环体内部；
    - GOTO语句所编写的程序可读性差，所以在开发中建议尽量少使用；

## PL/SQL异常处理

- PL/SQL程序的错误可以分为两类：编译型错误和异常型错误；
  - 编译型错误：程序的语法出现错误所导致的异常，由PL/SQL编译器发出错误报告；
  - 运行时异常：因程序
- 对于编译型异常：主要是语法方面的错误，只能修改代码；

### 异常处理的语法

- ```plsql
  EXCEPTION
      WHEN 异常类型|用户自定义异常|异常代码|OTHERS THEN
         异常处理;
  END;
  ```

### 自定义异常

- 语法

  - ```plsql
    异常名称  EXCEPTION
     pragme exception_init(异常名称，异常代码);
    ```

  - EXCEPTION用于定义异常的名称

  - 用户自定义异常需要使用**<font color = 'tomato'>RAISE语句</font>**进行显示触发，然后再进行异常的处理

## 游标

- 游标的概念
- ![image-20200305100457172](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_2.png)

- 主要有两种类型的游标
  - 隐式游标：由系统自动进行操作，用于处理DML语句和返回单行数据的select查询；
  - 显示游标：由用户显式声明和操作的游标，用于处理返回多行数据的select查询。
  - 由于游标需要对结果集中的每一条数据分别进行操作，当数据量较大时，游标的使用会带来性能的降低，在使用前需要考虑；

### 隐式游标

- 在使用DML语句和单行查询语句时系统自动创建隐式游标;
- 隐式游标由系统自动声明、打开和关闭；
- 通过隐式游标的属性可以获得最近所执行的sql结果信息；

#### 隐式游标的属性

- ![image-20200305101349291](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_3.png)

### 显式游标

- ![image-20200305102321533](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_4png)

#### 定义游标

- ```plsql
  cursor 游标名称 is 查询语句;
  ```

- 游标必须定义在pl/sql语句块的声明部分；

- 定义游标时必须明确定义要使用的sql查询语句；

- 定义游标时并没有生成数据，只是将定义信息保存在数据字典中；

- 游标定义后，可以使用"游标名称%rowtype"定义记录类型的变量;

#### 打开游标

- ![image-20200305102818083](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_5.png)
- 只有在打开游标时，才能真正创建缓冲区，并从数据库中检索数据；
- 如果游标定义中的变量值发生变化，则只能在下次打开游标时才起作用；
- 游标一旦打开，就无法再次打开，除非先关闭；

#### 检索游标

- 游标打开后，可以使用FETCH操作将游标中的数据以记录为单位检索出来放入pl/sql变量中,然后进行过程化的处理；

- ```plsql
  fetch 游标名称 into 变量;
  ```

  - ![image-20200305103429690](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_6.png)

- ![image-20200305104328577](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_7.png)

-  参数化显示游标

  - 参数化游标是指定义游标时使用参数，当通过不同的参数值打开游标时，可以产生不同的结果集；

  - 语法

    - ```plsql
      cursor 游标名称(参数名称 类型【，参数名称 类型】) is 查询语句;
      ```

    - 定义参数化游标时，只能指定参数的类型，而不能指定参数的长度和精度；

    - 参数化游标的打开语法

      - ```plsql
        open游标名称（参数值[,参数值...]）
        ```

        - 打开参数化游标时，实参的个数和数据类型必须与游标定义时的形参的个数和数据类型相匹配。

### 修改游标数据

```
cursor 游标名称 is
查询语句 [for update[of 据列,...][nowait]];
```

- ![image-20200305114530491](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_8.png)

- 避免死锁的游标定义

  - ```plsql
    cursor cursor_emp is 
    select * from emp for update nowait;
    ```

  - 使用了nowait子句，这样在游标数据进行更新操作时，如果发现所操作的数据行已经锁定，将不会等待立即返回，从而避免出现死锁的情况。

### 游标变量

![image-20200305134406295](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_9.png)

#### 定义游标变量

- ![image-20200305135122462](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_10.png)

- ![image-20200305135231311](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_11.png)

- 游标变量示例：
  - ![image-20200305135438883](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第八章_12.png)