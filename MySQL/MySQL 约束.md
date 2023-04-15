# MySQL 约束

### 非空属性

```mysql
mysql> create table 'class'(
class_name varchar(20) not null,
class_room varchar(20) not null);
```

*设置非空属性 插入属性不能为`null`*

### 默认值

```mysql
mysql> create table 'person'(
age tinyint unsigned default 18,
name varchar(30) default '张三');
```

*设置默认值*

#### default自动效果

```mysql
mysql> create table 'person'(
age tinyint unsigned default 18,
name varchar(30) not null default '张三');
```

*`name`不显示传入`null` 就是 `default` 的值*

### zerofill属性

*添加`zerofill`属性，如果填入数据位数不足补0，实际存储的依旧是你填入的数*

### Primary key

```mysql
mysql> create table if not exits 'student'(
id int unsigned primary key comment '这是学号，不能重复',
name varchar(30) default '痞老板');
```

#### 建表后添加

```mysql
mysql> alter table student add primary key(id);
```

*此时如果表中已有数据重复，就不行了*

#### 复合主键

```mysql
mysql> create table if not exits 'student'(
id int unsigned comment '这是学号，不能重复',
name varchar(30) default '痞老板',
primary key(id,name));
```

*`id`和`name`不能同时出现重复*

### 自增长

```mysql
 mysql> create table t1(
    -> id int unsigned primary key auto_increment,
    -> name varchar(30) not null);
```

`id` 会因为`name`数据插入而自增长 **1**

```mysql
mysql> show create table t1\G
*************************** 1. row ***************************
       Table: t1
Create Table: CREATE TABLE `t1` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(30) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
```

这里的可以看到表我们制作了一个计数器 `AUTO_INCREMENT` 表示下一个数据的`id`

**也可以在制作表手动设置**

