# MySQL Explain

## 关键指标

### type： 查询所使用的索引类型
包括ALL、index、range、ref、eq_ref、const等。以是按由快到慢排序进行说明：

+ system：系统表，少量数据，通常不需要磁盘IO
+ const：常数索引，只会在查询时使用常数值进行匹配。
+ eq_ref：唯一索引扫描，只扫描索引树中的一个匹配行。
+ ref：非唯一索引扫描， 扫描索引树的一部分查找匹配行。
+ range：范围扫描， 扫描索引树中的一个范围查找匹配行。
+ index：全索引扫描， 遍历索引树查找匹配行。
+ ALL：全表扫描， 遍历全表查找匹配行。

### posibable_key：

可能会用到的但实际又可能未用的索引。


### key：

实际使用的索引。

### extra：

描述MySQL在执行查询时所做的一些附加操作。
+ Using where：在存储引擎检索行后，再进行条件过滤（使用 WHERE 子句）。
+ Using index：使用了覆盖索引（也称为索引覆盖）优化，只需要扫描索引，无需回表。
+ Using index condition：在使用索引进行查找时，无法使用覆盖索引优化，需要回到表；
+ Using where; Using index：查询的列被索引覆盖，且where筛选条件是索引列之一。
+ Using join buffer：使用了连接缓存。
+ Using temporary：创建了临时表来存储查询结果。
+ Using filesort：：使用文件排序而不是索引排序。
+ Using index for group-by：在分组操作中使用了索引。
+ Using filesort for order by：在排序操作中使用了文件排序。
+ Using index for group-by; Using index for order by：在分组和排序操作中都使用了索引。

## 判断SQL是否使用了索引

### Key字段
不为空，说明使用了索引


## 其他字段

**rows**: 预估的扫描行数，表示执行查询所需扫描的行数。该值越大，查询的性能可能越差。

**filtered**: 表示通过索引条件过滤后的行数百分比。该值越小，说明索引条件过滤效果越好。

**cost**: 代价估算，表示查询执行的代价。通常与查询复杂度和索引使用有关，代价越低表示查询执行的效率越高。

**key_len**: 使用的索引长度，表示查询中使用的索引的长度。较短的索引长度通常更有利于性能。

**partitions**: 分区信息，如果查询涉及到分区表的话。

**ref**: 在等值连接中使用的列或常量。
