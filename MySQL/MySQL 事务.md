# MySQL 事务

`MySQL`事务本质不是数据库软件天然就有的，他是为了简化程序员的工作模型，**多个sql语句合在一起** 当然还得满足四个属性：原子性 隔离性 持久性 --> 一致性

`Innodb` 引擎才支持事务

------

`mysqld`里有很多事务，`需要先描述再组织`

```mysql
mysql> begin;
```

开始一个事务，后续输入的`sql`语句都在事务中 

------

- 如果在事务提交前，`mysql`奔溃了(ctrl + /)，那么事务会进行回滚到开始的位置
- 如果提交了，再崩溃是没有影响的
- 事务可以手动`rollback`回滚到`savepoint`
- 如果提交了，就不存在回滚了

------

`autocommit`并不影响我们手动`begin`和`end`中间的sql语句，影响的是我们平常使用的单sql语句，单sql语句其实就相当于是一个事务，如果 `set autocommit = 0`，没有`commit`，`mysql`奔溃，同样也会回滚

`sql`语句执行完后，会根据`autocommit`判断事务是否自动提交

### 回滚

```mysql
mysql> save point p1; #设置保存点
```

```mysql
mysql> rollback; #回滚到事务的开始部分
```

```mysql
mysql> rollback to 保存点;
```



------

###  设置隔离等级

```mysql
set session transaction isolation level READ UNCOMMITED; 
```

设置会话隔离等级

```mysql
set global transaction isolation level READ UNCOMMITED;
```

设置全局等级

@@session.tx_isolation  ==  @@tx_isolation

### READ UNCOMMITTED

这个隔离等级表示一个事务在还没有提交前，其他事务可以看到我操作的数据 ，`脏读`

### READ COMMITTED

这个隔离等级代表，一个事务只有commit后才能被其他的事务看见，`不可重复读`

### REAPEATABLE-READ

可重复读隔离等级，一个事务的删除和修改是不会被其他事务看见的，增加的话，MySQL不会，其他数据库可能会。

> 注意： 增加的数据被其他事务看见，这种称为幻读现象

### serializable

串行化的隔离等级，一个事务进行修改操作会阻塞，只有等另一个事务commit后，才会执行阻塞事务后续的sql语句

### 隐藏列字段

![image-20230513205654098](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230513205654098.png)

> 注意：删除flag字段，一个事务再进行的过程中，是不能看到其他事务进行操作的变化的，所以对于删除操作，使用flag表示已经删除，实际得等与删除数据相关的事务结束，清理线程回去清理

### undolog原理

![image-20230514112056717](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230514112056717.png)

因为事务的开始是有先有后的，先到和后到的事务看到的东西应该是不一样的

### 隔离级别决定是快照读还是当前读

### RR和RC

- `RR`级别下对某条记录快照读会形成快照和`read view`，此后再调用快照读时都用的是同一个`read view`，所以读到的是历史上的数据，与其几乎同时开启的事务提交的更改，它也看不见
- `RC`级别下每次形成快照读，是能看到与其几乎同时开启事务提交的更改的
