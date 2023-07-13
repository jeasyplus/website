# MyBatis

## MyBatis工作原理
**启动阶段：**
+ 解析配置文件并加载到内存当中

**运行阶段：**
+ 读取内存中的配置信息，实现对应的功能

### 生成代理
+ 根据接口XXXMapper生成动态代理对象MapperProxy。
+ 读取XML或者方法注解生成MapperMethod。
+ 执行SQL，MapperMethod解析和记录代理方法的入参和出参，对SQL的占位符进行替换，并对SQL结果进行转换。
+ 通过BoundSql将方法的参数转成SQL的入参，使用SqlSession执行Sql。
+ Sqlsession封装了对Sql执行，处理逻辑由Executor执行。
+ Executor有多个实现类，查询之前，先check缓存是否存在，默认使用CachingExecutor类，作用是二级缓存。
+ 二级缓存是和命名空间绑定，如果执行多表操作会出现脏数据，如果是不同的事务，也可能引起脏读，需慎重。
+ 二级缓存没命中会进入到BaseExecutor中继续执行，这个过程中，会调用一级缓存。
+ 一级缓存使用的是PerpetualCache，是一个简单的HashMap。在更新操作事务提交或者回滚时清空。一级缓存是和SqlSession绑定的。
+ 一级缓存中没有的话，调用JDBC执行SQL。调用JDBC之前，需要建立数据库连接。
+ Mybatis使用数据源获取链接。
+ 获取到Connection之后，使用StatementHandler处理JDBC的Statement。
+ 通过BoundSql组装Sql。
+ 拿到执行结果ResultSet使用ResultMap将数据映射，核心的转换逻辑通过TypeHandler完成。

## MyBatis字段映射

+ 同名映射
+ 别名映射
+ ResultMap映射
+ 自定义TypeHandler映射

## MyBatis#和$的区别

#{}传递的参数，mybatis默认将其作为字符串，并作预处理。

${}传入的参数，mybatis不作特殊处理，直接进行占位替换。

## Mybatis插件

Mybatis插件涉及3个关键接口：

**Interceptor、Invocation**和**Plugin**。

+ Interceptor：拦截器接口，定义插件的基本功能，包括插件的初始化、插件的拦截方法以及插件的销毁方法。

+ Invocation：调用接口，执行SQL语句时的状态，包括SQL语句、参数、返回值等信息。

+ Plugin：插件接口，所有插件都会封装成Plugin对象，实现对SQL语句的拦截和修改。

### 插件处理流程

+ 首先，将所有实现了Interceptor接口的插件进行初始化。
+ 将所有插件和原始的Executor对象封装成InvocationChain对象。
+ SQL执行时，通过InvocationChain对象依次调用所有插件的intercept方法，实现对SQL语句的拦截和修改。
+ 将修改后的SQL语句交给Executor对象执行，并将执行结果返回给调用方。


## Mybatis缓存机制

Mybatis缓存分为：一级缓存和二级缓存。

### 一级缓存

同一个会话中，Mybatis将SQL结果缓存到内存中，再执行相同的SQL语句时，先查看缓存中是否存在，存在则直接返回缓存中的结果。一级缓存是默认开启，可通过Mybatis配置文件设置禁用或刷新缓存控制缓存的使用。

### 二级缓存

基于命名空间的缓存，跨会话，可在多个会话之间共享，二级缓存不适用于多表查询。开启二级缓存，需要在Mybatis的配置文件中配置相应的缓存实现，并在需要使用缓存的Mapper接口上添加@CacheNamespace。

## PageHelper分页原理

PageHelper从ThreadLocal中再获取分页参数信息，页码和页大小，然后执行分页算法，计算数据块的起始位置。
通过修改SQL语句，动态拼接上limit语句。

## MyBatis-Plus

LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<>();



