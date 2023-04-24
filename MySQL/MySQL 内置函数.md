# MySQL 内置函数

### COUNT

```mysql
mysql> select count(distinct math) from exam;
```

统计去重的`math`成绩

```mysql
mysql> select count(english),sum(english) from XXX where english < 60;
```

统计英语成绩小于60的人数，以及他们的总英语成绩

### AVG

```mysql
mysql> select avg(english + math + chinese) 平均日期 from exam_reslut;
```

算三种课目总成绩的平均值

### max.min

```mysql
mysql> select max(math) from exam_reslut where math >= 60;
```

### GROUP BY

```mysql
mysql> select dep,job,avg(ral) 平均工资,min(ral) 最低工资 form XXX group by dep,job;
```

只有group by 的列信息和内置函数可以显示(追加也可以)

### HAVING

```mysql
mysql> select dep,avg(ral) 平均工资 from XXX where ral > 1000 group by dep having ral < 2000;
```

1. from XXX
2. where
3. group by
4. select、avg
5. having

> `having`通常是在整个分组聚合统计之后，在去筛选得
>
> `where`通常是在分组之前去表中筛选数据得

### 日期函数

![image-20230421211633361](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230421211633361.png)

```mysql
mysql> select * from XXX where data_add(datetime,interval 2 minute) > now();
```

选出两分钟之内插入的数据

### 字符串函数‘

![image-20230422231413111](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230422231413111.png)

#### charset

```mysql
mysql> select charset(列) from XXX;
```

返回字节数

#### concat

```mysql
mysql> select concat("nihoa",'world',123,456.78);
```

拼接字符串，整数和浮点数自动转换成字符串

#### length

```mysql
mysql> select length('你好');
```

返回字符串长度，单位是字节，不同的字符集编码，结果就不同

#### substring

```mysql
mysql> select substring('你好',2,1);		/******结果为 好 *******/
```

> ​	注意：最后一个参数表示的是字符的个数，不是字节数。

第二个参数为第几个字符开始

#### replace

```mysql
mysql> select replace(name,'X','大角牛') from XXX where ral > 2000;
```

把`name`中包含`X`的替换成 `大角牛`

#### instr

```mysql
mysql> select * from XXX where instr(name,'A') > 0;
```

查找名字中带有`A`的人，返回字符出现的位置

#### strcmp

```mysql
mysql> select * from XXX where strcmp(name,'qcv') = 0;
```

选出名字是`qcv`的人

- 等于 返回0
- 小于 返回-1
- 大于 返回 1

### 数学函数

![image-20230424102726411](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230424102726411.png)

#### conv

```mysql
mysql> select conv(10,10,16);
```

从十进制转为16进制

### 其他函数

#### password

```mysql
mysql> select password('1234');
```

可以对设置的密码进行加密

#### 子查询

```mysql
mysql> select name,job from XXX where ral > (select avg(ral) from XXX);
```

查询工资大于平均工资的人的姓名和职位

子查询查询先查子`select`的
