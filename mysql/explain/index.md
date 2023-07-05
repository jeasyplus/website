# MySQL Explain

## 关键指标
**rows**: 预估的扫描行数，表示执行查询所需扫描的行数。该值越大，查询的性能可能越差。

**filtered**: 表示通过索引条件过滤后的行数百分比。该值越小，说明索引条件过滤效果越好。

**cost**: 代价估算，表示查询执行的代价。通常与查询复杂度和索引使用有关，代价越低表示查询执行的效率越高。

**type**: 访问类型，表示查询时使用的访问方式。

```
system: 表示仅有一行数据，常见于表只有一行的情况。
const: 使用唯一索引查找，根据常量条件进行查找。
eq_ref: 使用唯一索引查找，与其他表进行等值连接。
ref: 使用非唯一索引查找，与其他表进行等值连接。
range: 使用索引范围查找。
index: 使用索引进行全表扫描。
all: 全表扫描。
```

常见的类型有system、const、eq_ref、ref、range、index和all，其中更优的访问类型往往意味着更高的性能。

**possible_keys**: 可能使用的索引列表。

**key_len**: 使用的索引长度，表示查询中使用的索引的长度。较短的索引长度通常更有利于性能。

**Extra**: 额外的信息，例如使用临时表、排序方式、是否使用索引等。特殊的Extra信息可能指示潜在的性能问题。

以上指标可以帮助你了解查询的执行计划和性能瓶颈，通过分析这些指标并结合具体的查询场景，可以进行针对性的性能优化和索引调整。需要注意的是，这些指标只是参考值，具体的优化策略还需要结合实际情况进行综合分析和测试。

## 其他指标

**partitions**: 分区信息，如果查询涉及到分区表的话。


**key**: 实际使用的索引。

**ref**: 在等值连接中使用的列或常量。