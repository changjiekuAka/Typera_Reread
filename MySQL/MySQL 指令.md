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





​		

