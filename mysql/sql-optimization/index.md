# SQL优化

## 慢SQL原因

+ 1、索引失效

```roomsql
SELECT * FROM table_name FORCE INDEX (index_name) WHERE condition;
```
+ 2、多表join
尽量避免联表。
如果联表，使用数量少的做为驱动表
```roomsql
SELECT  *
FROM table1
STRAIGHT_JOIN tbble2 ON id = id
WHERE condition;
```

+ 3、查询字段太多
+ 4、表中数据量太大
+ 5、索引区分度不高
+ 6、数据库连接数不够
+ 7、表结构不合理
+ 8、数据库I/O或CPU使用率高
+ 9、数据库参数不合理
+ 10、事务比较长
+ 11、锁竞争导致的等待

## MySQL热点数据更新

+ 1、拆分库存，拆分成多个库存，一次扣减动作就分散到不同的库。
+ 2、合并请求，将多个库存扣减请求合并成一个，批量更新。
+ 3、update转成insert，异步统计剩余库存。
+ 使用热点数据库：如RDS、TXSQL

[SQL调优指南](https://www.alibabacloud.com/help/zh/polardb/latest/sql-tuning-guide)

[热点数据处理Inventory Hint](https://www.alibabacloud.com/help/zh/apsaradb-for-rds/latest/inventory-hint)


[腾讯数据库热点更新处理](https://cloud.tencent.com/document/product/236/63239)
