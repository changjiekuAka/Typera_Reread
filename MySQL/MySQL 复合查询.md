# MySQL 复合查询

### 子查询

```mysql
mysql> select name,job from XXX where ral > (select avg(ral) from XXX);
```

查询工资大于平均工资的人的姓名和职位

子查询查询先查子`select`的

### 多表查询

```mysql
mysql> select name,depno from XXX,XXX where XXX.depno = XXX.depno;
```

最终我们查的始终只有一个表，第一个表中的数据依次与第二个表中的数据进行拼接。

### 自链接

```mysql
mysql> select * from XXX as t1,XXX as t2; 
```

自链接成功要求必须两个表**重命名**

### 多行查询

```msyql
mysql> select * from XXX where job in (select distinct job from XXX where depno > 10); 
```

有时候自查询查询的结果可能有多行数据：

`in` ：只要job信息在子查询出来的信息里面就可以列出来

`all`:  意思是把子查询的结果看成一个集合

`any`：查询数据的任意一个

### 多列查询

```mysql
mysql> select * from XXX where (job,depno) = (select jon,depno from XXX);
```

同时比较`job`和`depno`两个列信息

### UNION

```mysql
mysql> select * from XXX where sal > 2000 union select * from XXX where job = '监工';
```

`union`：选出职位是监工且工资大于2000的人，满足任意一边的条件就会有一条数据，去重

`union all` ：满足任意一边的条件就会有一条数据，不会去重