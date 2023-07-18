# 数据湖

## Apache Hudi


[Apache Hudi（Hadoop Upserts Deletes and Incrementals）](https://hudi.apache.org/) 最初是 Uber 公司开发的大数据存储和处理框架，希望解决大规模数据湖管理和实时数据处理的挑战。

目标是提供**高效、可靠和实时的数据湖解决方案**，支持实时数据更新和查询，同时保持数据一致性和可靠性。

后来将 Hudi 作为开源项目捐赠给 Apache 软件基金会。


**Apache Hudi**是一个事务性的数据湖平台，将数据库和数据仓库功能引入数据湖中。

Hudi是对传统批量数据处理方式的重构，采用强大的增量处理框架，实现低延迟的分钟级分析。

[Apache Hudi overview](https://jeasyplus.com/images/data-lakes/hudi-lake-overview.png)

### record-level updates/delet

Hudi 支持增量数据写入，记录级别的更新和删除。

Hudi 不需要重新写入整个数据集，而是利用“写时复制”的概念来完成。

当更新记录时，Hudi 会创建一个包含更新数据的新版本记录，并将其附加到数据集中，原始版本的记录保持不变。

当删除记录时，Hudi 会创建一个包含特殊标记的新版本记录，表示该记录已被删除。

通过支持记录级别的更新和删除，Hudi 实现了高效的数据管理、版本控制和时间旅行功能，使用户能够在大规模数据湖或数据仓库环境中保持数据完整性和历史数据。

这对于在各种大数据使用场景中构建实时和可靠的数据处理系统非常重要。

### transactions

支持 ACID 事务。

+ ACID Transactions: 支持 ACID 事务。


+ Write Consistency: 采用写时复制（Copy-on-Write）的方式，确保写操作的一致性。在写入数据时，Hudi 将数据追加到数据集中，并在后台处理复制和合并操作，保证数据的一致性。


+ Record-Level Updates and Deletes: 支持记录级别的更新和删除。当需要更新或删除单个记录时，Hudi 会创建新的版本来反映这些变更，而不是修改原始数据，保持数据的不可变性。


+ Merge-on-Read (MOR): Hudi 的 Merge-on-Read 功能允许在读取数据时进行数据合并操作，将增量写入的数据合并到基础数据集中。这确保了数据湖中的数据集是最新且一致的。


+ Optimistic Concurrency Control: Hudi 使用乐观并发控制机制，允许多个事务并发操作，通过版本控制和冲突解决来保证数据的一致性。

### change streams

Apache Hudi 提供了 Change Streams 特性，捕获和处理数据集合中的变更操作。

Change Streams 可以让用户实时获取数据集合中的插入、更新和删除等变更事件，并在数据湖或数据仓库中进行实时处理。

+ 实时数据捕获：Change Streams 可以实时捕获数据集合中的变更操作，保证变更事件的实时性。


+ 插入、更新和删除：Change Streams 支持捕获数据集合中的插入、更新和删除操作，让用户了解数据的变化情况。


+ 事件订阅：用户可以订阅 Change Streams，接收数据集合中变更事件的通知。


+ 集成支持：Change Streams 可以与其他数据处理框架和平台集成，例如 Apache Kafka、Apache Spark 等。




