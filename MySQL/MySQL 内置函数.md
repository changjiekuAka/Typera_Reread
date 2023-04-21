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
