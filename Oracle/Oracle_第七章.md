[toc]

# 模式和对象



## 模式对象

## 视图

- ```sql
  create 【or replace】 【force】 view [schema.]view_name [(alias[,alias]...)]
  as
  subquery
  [with check option [Constraint constraint]]
  [with read only]
  ```

  - or replace表示若创建的视图已经存在，自动重建该视图。
  - force表示不管基表是否存在都创建视图
  - alias表示为视图产生的列定义的别名
  - subquery表示一条完整的select语句
  - **<font color ='tomato'>with check option</font><font color ='seagreen'>表示在对视图的数据进行插入或修改操作时，检查数据是否满足视图中select语句定义的约束条件；</font>> 选项constraint用于指定约束的别名**
  - with read only表示该视图为只读视图，不能进行任何DML操作。

### 视图操作

- 在视图上可以进行修改数据的DML操作。视图时“虚表”，因此对视图的操作最终会转换为对基表的操作。
- 视图上DML操作有以下限制
  - 只能修改一个底层的基表
  - 如果修改违反了基表的约束条件，则无法更新视图。
  - 如果视图包含连接操作符、distinct关键字、集合操作符、聚合函数、group by，connect by，strat with子句则无法更新视图；
  - 如果视图包含伪劣rowid、rownum等或表达式则将无法更新视图；

### 键值保存表

- 如果多个基表都没有主键，则不会出现键值保存表
- 有外键的表叫做从表，从表会作为键值保存表
- 对于视图的DML操作，只能操作属于键值保存表的列；

## 序列

> - 序列是用于生成唯一、连续序号的数据库对象，可以为多个数据库用户依次生成不重复的连续整数，通常会使用序列自动生成表中的主键值；

### 创建序列

- ```sql
  create sequence sequence_name
  [start with n]
  [increament by m]
  [maxvalue max|nomaxvalue]
  [minvalue min|nominvalue]
  [cycle|nocycle]
  [cache cache_name|nocache]
  ```

  - start with 用于设置序列的初始值，默认是1；
  - increment by设置相邻两个元素之间的差值，即步长，默认为1
  - maxvalue用于设置序列的最大值，默认情况下，递增序列的最大值为1027，递减序列的最大值为-1；
  - minvalue用于设置序列的最小值，默认情况下，递增序列的最小值为1，递减序列的最小值为-1026；
  - cycle用于指定序列达到最大或最小值后，是否循环生成值，默认为不循环；
  - cache用于设置oracle服务器预先分配并保留在内存中的值的个数，默认Oracle服务器高速缓存中有20个值；

### 使用序列

- 序列的使用是指通过序列的伪列来访问序列的值；
- 序列有以下两个伪列：
  - nextval：返回序列的下一个值
  - currval：返回序列的当前值，并且只有在发出至少一个nextval之后才可以使用currval<font color ='tomato'>（理解为最近一次使用的nextval的值）</font>>
- 序列的值可以应用于查询的选择列表、insert语句的values子句、update语句的set子句、但不能应用在where子句或PL/sql的过程性语句中；

### 序列的维护

#### 修改序列

- 序列创建完成后，可以使用alter，sequence语句修改序列。
- 除了不能修改序列起始值外，可以对序列其他任何子句和参数进行修改。
- 如果要修改maxvalue参数值，需要保证修改后的最大值大于序列的当前值；
- 序列的修改只影响以后生成的序列号

#### 删除序列

- drop  sequence sequence_name;

## 同义词

- 同义词是数据库中模式对象的别名
- 同义词分为两类
  - 私有同义词
    - 属于创建它的用户，且只能在该用户模式内访问，此用户可以通过授权来控制其他用户使用其私有同义词；
  - 公有同义词
    - 有特殊的用户组public所拥有，一般由dba，system，sys创建，数据库中的每个用户都能够访问；
    - 公用同义词往往用来标示一些普通，常用的数据库对象；

### 创建同义词

- ```sql
  create [public] synonym synonym_name
  for schema_object;
  ```

  - public用来指定创建的同义词是否为公有同义词；
  - synonym_name为创建的同义词名称；
  - schema_object为同义词所针对的模式对象

### 同义词的维护

- 创建或替换现有的同义词
  - 通过create or replace语句创建或替换现有的同义词；
- 删除同义词
  - drop synonym

## 索引

> **<font color='tomato'>索引需要独立占据数据存储空间，它是独立于表之外的数据库对象</font>**

### 索引分类

- 根据索引值是否唯一，可以分为唯一性索引和非唯一性索引；
- 根据索引的组织结构，可以分为平衡树索引和位图索引
- 根据索引基于的列数不同，可以分为单列索引和复合索引

### 创建索引

- ```sql
  create [unique][bitmap] index index_name
  on table_name(column_name[ASC|DESC],...)|[expression])
  [parameter_list];
  ```

  

- unique表示建立唯一性索引；
- bitmap表示建立位图索引；
- table_name(column_name)表示索引基于的表名和列名；
- ASC/DESC用于指定索引值的排列顺序，默认为DESC；
- parameter_list用于指定索引的存放位置、存储空间分配和数据块参数设置；

### 修改索引

- 可以使用alter index语句可以对索引进行修改
- 索引的修改包括索引的合并、索引的重建、索引的重命名等；

#### 合并索引

- 随着对表不断地进行更新操作，在索引中将会产生越来越多的存储碎片，影响索引的使用效率，可以通过合并索引和重建索引两种方法清理存储碎片；
- alter index ...coalesce语句可以进行索引合并操作；
- 但只是简单地将平衡树叶子节点中的存储碎片进行合并，并不会改变索引的物理组织结构；

#### 重建索引

- 使用alter index...republic语句重建索引；

#### 索引的重命名

- alter index..rename to 语句为索引重命名;

#### 删除索引

- 考虑删除索引的几种情况：
  - 该索引不再使用
  - 通过一段时间监视，发现几乎没有查询或索引很少使用；
  - 索引中包含太多的存储碎片
  - 移动了表的数据而导致索引失效
- 删除语句：   drop index [index_name]



