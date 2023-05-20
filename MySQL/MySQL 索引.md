# MySQL 索引

### 介绍

​	索引是一种数据结构，可以为存储引擎提高数据的访问速度、

![image-20230517155010253](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230517155010253.png)

操作系统与磁盘交互的基本单位是4k'b

##### 没有索引

​	数据是存储在磁盘上的，如果没有索引，查询数据就需要将数据加载到内存中来，依次进行检索，读取磁盘次数较多。

> 注意：Mysql与磁盘进行交互的单位是16kb，在MySQL这里叫做page
>
> <img src="C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230430170930448.png" alt="image-20230430170930448" style="zoom: 200%;" />
>
> page的结构：
>
> ![image-20230430181111027](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230430181111027.png)

### 好处

​	索引不需要扫描全表，可以通过索引表找到该数据的物理地址访问数据。

### 副作用

- 创建索引会占用存储空间。如果你在表上创建了太多的索引，那么表的大小将会变得很大。
- 当你执行INSERT、UPDATE和DELETE操作时，MySQL需要同时更新索引，这需要更多的时间和资源。

### 内核结构

![image-20230501101518317](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230501101518317.png)

> 注意：叶子节点是连接在一起的(方便范围查找)，目录表不是！

有主键的表有b+树

### 聚簇vs非聚簇 

![image-20230507182853245](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230507182853245.png)

聚簇索引相当于属性和数据是耦合的

MyISAM引擎创建普通键索引，就是重新创一个新的表出来，保存数据记录的地址

Innodb引擎创建普通键索引，去普通索引找会找到主键，然后再用主键去主键表里去寻找；普通索引里没有数据，这种查询叫回表查询

```mysql
mysql> alter table XXX add index(name);
```

```mysql
mysql> alter table XXX add unique(name);
```

普通索引和唯一索引是没有差别的
