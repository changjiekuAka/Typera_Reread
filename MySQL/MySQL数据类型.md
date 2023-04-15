# MySQL 数据类型

### 文本、二进制类型

- `text` 大文本，不支持全文索引，不支持默认值

- `varchar` : 变长字符串，最大长度26635

- `char(L)` ：固定长度字符串，L是可以存储的长度，单位是字符，最大长度可以是255

  `name char(2)`   字符既可以是两个字母也可以是两个中文汉字

### 枚举类型

- `enum ('男'，'女')`

   表示只能插入 男 或 女

- `set('登山','篮球','游泳')`   1 1 1

  可以选择括号中的任意一个或多个插入

  枚举的每一个数据也对应一个位图，比特位的位置对应数据

### 时间日期

- `date/datetime/timestamp`

​		日期类型（yyyy-mm-dd，yyyy-mm-dd hh:mm:ss）timestamp 时间戳

### 数值类型

<img src="C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230414222405281.png" alt="image-20230414222405281" style="zoom:67%;" />

