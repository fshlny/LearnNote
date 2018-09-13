# Mysql问题记录

### Mysql创建function失败记录

在`Mysql8.0`版本下创建`Function`，提示如下错误:

![](../image/mysql_create_function_fail.png)
```
1418 - This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe 
    log_bin_trust_function_creators variable)
```

网友给出的解释如下：

这是我们开启了bin-log, 我们就必须指定我们的函数是否是
* 1 `DETERMINISTIC` 不确定的
* 2 `NO SQL` 没有`SQl`语句，当然也不会修改数据
* 3 `READS SQL DATA` 只是读取数据，当然也不会修改数据
* 4 `MODIFIES SQL DATA` 要修改数据
* 5 `CONTAINS SQL` 包含了`SQL`语句

其中在`function`里面，只有 `DETERMINISTIC`, `NO SQL` 和 `READS SQL DATA` 被支持。如果我们开启了 `bin-log`, 我们就必须为我们的`function`指定一个参数。

解决方案如下:

```
mysql> show variables like 'log_bin_trust_function_creators';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin_trust_function_creators | OFF   |
+---------------------------------+-------+
1 row in set

mysql> set global log_bin_trust_function_creators=1;
Query OK, 0 rows affected

mysql> show variables like 'log_bin_trust_function_creators';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin_trust_function_creators | ON    |
+---------------------------------+-------+
1 row in set
```
这样就可以创建`function`了