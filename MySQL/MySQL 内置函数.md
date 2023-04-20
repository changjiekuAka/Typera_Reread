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



