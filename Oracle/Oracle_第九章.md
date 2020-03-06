[toc]



# PL/SQL高级应用

## 存储过程的创建与管理

- 存储过程创建的基本语法

  - ```plsql
    create [or replace] procedure 过程名称[(参数名称[参数模式]数据类型[default!=vavlue],...)]
    as|is
        声明部分;
    begin
      程序部分;
    exception 
    异常处理;
    end;
    ```

    - 参数模式表示存储过程的数据接收操作，包括in、out、in out 三种 默认为in；
    - ![image-20200305145203669](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_1.png)

  

### IN参数模式

- ![image-20200305150204657](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_2.png)

- ![image-20200305150229477](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_3.png)

### OUT参数模式

- ![image-20200305150319636](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_4.png)

- ![image-20200305150345897](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_5)

### IN OUT 模式

- ![image-20200305150731342](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_6.png)

- ![image-20200305150755152](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_7.png)

### 修改存储过程

- 对于存储过程的修改，可以先删除该存储过程，然后重新创建，这样需要为新创建的的存储过程重新进行权限分配，因此通常采用create or replace procedure方式重新创建并覆盖原有的存储过程，**<font color = 'tomato'>这样会保留存储过程原有的权限分配；</font>**

### 存储过程重新编译

- 语法
  - alter procedure   **<font color = 'red'>procedure_name </font>**   compile;

## 函数的创建与管理

![image-20200305151634761](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_8.png)

### 创建无参函数

- ![image-20200305151918999](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_9.png)

### 创建有参函数

![image-20200305152102160](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_10.png)

## 触发器

### DML触发器

- ![image-20200306092549265](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_11.png)

- before|after:指触发器的出发时间是前触发还是后触发
- insert|delete|update【of 列名称】：表示触发的事件；
- for each row ： 表示定义行级触发器，若省略则表示定义语句级触发器;
- follws: 用于指定配置多个触发器时所执行的先后次序；
- disable: 表示触发器的启用状态，默认启用，若使用此选项定义为禁用状态；
- when触发条件：在行级触发器中，用来控制触发器是否执行的一个控制条件；

#### DML触发器的分类

- ![image-20200306093222221](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_12.png)

##### 语句级触发器

- ![n级](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_13.png)

##### 行级触发器

- ![image-20200306101319545](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_14.png)

- ![image-20200306101352752](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_15.png)

- ![image-20200306101721816](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_16.png)

- 实现数据列的自动增长
  - ![image-20200306103125866](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_17.png)

- ![image-20200306103149802](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_18)

- 触发器谓词
  - ![image-20200306103301785](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_19.png)

- 执行操作日志
  - ![image-20200306103351548](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_21.png)
  - ![image-20200306103413955](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_22.png)

- 触发器的执行顺序
  - 当用户执行更新操作时，触发器的执行顺序为
    - ​	语句级前触发器->行级前触发器->更新操作->行级后触发器->语句级后触发器；

### 复合触发器

- ![image-20200306103930491](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_23.png)

- 语法
  - ![image-20200306103951773](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_24.png)

- ![image-20200306104020773](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_25.png)

- ![image-20200306104049115](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_26.png)

## 替代触发器

- ![image-20200306104302297](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_替代触发器)

- ![image-20200306104340846](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_27.png)

- ![image-20200306104410862](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_28.png)

- ![image-20200306110644932](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_29.png)

- ![image-20200306110910803](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_30.png)

- ![image-20200306110928632](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_31.png)

## 系统触发器

- ![image-20200306111053424](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_1.png)

- ![image-20200306111150197](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_2.png)

- 系统触发器创建语法
  - ![image-20200306111540616](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_3.png)

- 实现对数据库DDL操作的日志记录
  - ![image-20200306111836212](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_4.png)
  - ![image-20200306111919914](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_5.png)

- ![image-20200306112408041](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_6.png)

- ![image-20200306112446339](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_7.png)

- ![image-20200306112508918](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_8.png)

- ![image-20200306112535077](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_9.png)

- ![image-20200306112553670](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_10.png)

## 触发器的管理

- ![image-20200306112639176](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_11.png)

- ![image-20200306112710191](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_12.png)

- ![image-20200306112729117](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_13.png)

- ![image-20200306112755961](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_14.png)

- ![image-20200306112812241](C:\Users\lyh\Desktop\青软笔记\Oracle\青软笔记图床\第九章_系统触发器_15.png)