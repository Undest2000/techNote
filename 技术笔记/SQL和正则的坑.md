## SQL和正则的坑

SQL：容易出现}没有写全 

更新表的同时如果要进行子查询要把子查询的表命名为别的表

正则: 像点.这种特殊符号要加\\\来反义，要不然出来结果

MYSQL是没有全外连接的写法的，也不支持SQL92

代替写法是用union all/union

把left join和right join给连接起来
