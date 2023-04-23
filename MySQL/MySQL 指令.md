# MySQL 指令

- `show processlist`   

  显示链接用户

- `show create table XXX \G`

  查创建语句

- `alter table XXX drop XXX`

  删除表中的列，只剩一个列不能删除

- `alter table XXX modify 列名 列属性`

  修改列名的属性 **覆盖**

- ``alter table XXX change 列名 新列名 varchar(80) comment  `这是一个新列名`  ``

  修改表中的列名 **覆盖**

- `alter table XXX rename XXX `

  修改列名

- `select find_in_set('a','a,b,c');`

  查a是否在集合里，返回下标

- `select * from XXX where find_in_set('a',集合);`

  返回所有a在集合里的 数据的信息

- `select * from XXX order by XXX ASC;`

  升排序

- `select * from XXX order by XXX DESC;`

  降排序
  
- `update table student set student_id = 666 where id = 104;`

  修改`id`为 104 的学生的`student_id` 为 666

- `delete from student where id = 104`

  删除表中`id`为104的数据

- `insert into student(id,name) values(2,'吉吉暴') on duplicate key update id = 2,name='超人墙';`

  插入失败变为跟新数据 repalce也可以
  
- `select id, name, 科目 + 科目 + 科目 total from XXX;`

  `total` 为三个科目的成绩  
  
- `select * from XXX where name like '%S%'`

  查看名字中包含`S`的有哪些人

### where

![image-20230417104922076](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230417104922076.png)

<=> 可以查NULL = 不可以 ； is NULL，is not NULL

`select * from XXX order by math desc,english asc,chinese asc;`

`math`相等就按`english`排，以此类推

![image-20230417232428066](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230417232428066.png)

> 注意：where里面不可以使用avg等内置函数

```mysql
mysql> select ral from XXX where avg(ral) < 20;
```

`where`是根据条件删选出数据，`avg`是根据数据得到结果，`where`的执行顺序更靠前，此时两者就矛盾了，`where`要`avg`的结果得到数据，`avg`要数据得到结果。



### TRUNCATE

```mysql
TRUNCATE [TABLE] table_name;
```

普通的删除delete表中的数据，AUTO_INCREMENT不会被清零；

truncate 会把它清零



***增、删、查、改，从磁盘load到内存***



```mysql
select count(*) from XXX;
```

统计XXX中的数据的数量，NULL不参与运算

