# MySQL 视图

创建视图，以后就不用再执行select语句了，直接看对应select语句生成的视图就好了

视图是内存级的

视图和基表是联动的，一方改变另一方也改变

#### 删除

```mysql
mysql> drop view XXX;
```

