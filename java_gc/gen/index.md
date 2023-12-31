# 分代模型

对象的生命周期不同，通过分代采用不同的回收策略，以提高性能。

## 分代
+ 新生代 - （标记 - 复制）
  + Eden
  + Survivor(2)
+ 老年代 - （标记 - 清除/整理）

### 晋级
三者满足其一就会晋级到老年代。

+ 15次GC依然存活。
```text
-XX:MaxTenuringThreshold
```

+ 动态对象年龄判断：
  + 如果某一个值能筛选出Survivor区中50%的对象，这个值就是晋级的年龄。

+ 大对象直接进入老年代。
```text
-XX:PretenureSizeThreshold
```

## 分代GC
Minor GC（YoungGC），主要用于对新生代垃圾回收。

Full GC，除了会对新生代垃圾回收以外，还会对老年代或者永久代进行回收。

相比较于Minor GC，Full GC的收集频率更低，耗时更长。

## 空间担保

+ YoungGC 每次执行前都会检查一下**老年代连续可用的内存**是否大于**新生代对象占用的内存**。
  + 如果大于说明，本次YoungGC是安全的。 
  + +如果小于，会检测JVM相关参数（HandlePromotionFailure）的值。 
    + 如果为true，会用以往晋级时所需内存的平均值做为参考。
      + 如果平均值小于老年代连续可用的内存，尝试进行YoungGC。
      + 如果平均值大于老年代连续可用的内存，进行FullGC。
    + 如果相关JVM参数为false，进行FullGC。

JDK7之后取消了HandlePromotionFailure参数，进行使用平均值作判断。

担保结果可能成功，也可能失败。

所以，在YoungGC的复制阶段执行后，会发生以下三种情况：
+ 存活对象进入Survivor区。
+ 进入年老代区 -  存活对象大于Survivor小于老年代。
+ 触发FullGC - 存活对象大于Survivor+老年代。

## GC触发条件

+ YoungGC：eden区内存占满时
+ FullGC：
  + 老年代空间不足
  + 空间分配担保失败
  + 永久代空间不足
  + System.gc()

## STW

GC过程中应用程序被暂停执行。

不管选用哪种GC，STW都不能避免。

STW的原因：
+ 避免不断产生垃圾。
+ 防止对象标记出错
  + 多标：垃圾对象被标为存活对象
  + 应该存活的对象未被正确标记，被回收。

## GC Root

"root"对象通常是指虚拟机**栈中的引用**、**静态变量**等。

+ Class，系统类加载器创建的对象。
+ Thread，活跃的线程。
+ Stack Local，方法的局部变更和参数
+ JNI Local，JNI方法的局部变量和参数
+ JNI Global，全局JNI引用
+ Monitor Used，被同步锁持有的对象
+ JVM内部对象

## 三色标记

一种垃圾标记算法，帮助减少STW时间。

三色：白色（未标记）、灰色（对象已标记，引用对象未标记完）、黑色（已标记且所引用的对象也全部标记完成）

### 阶段：

#### 初始标记（STW）：
只标记直接从"根"对象可达的对象，这些对象被标记为灰色。
#### 并发标记：
用户线程运行的同时，垃圾收集器在后台进行标记工作。
从灰色对象开始遍历，并将已经遍历的对象标记为黑色。
同时会使用**写屏障（Write Barrier）**保证并发标记的正确性。
#### 重新标记(STW)：
针对并发标记阶段未被标记和被修改的对象。
从灰色对象开始重新标记，将引用的对象标记为灰色，已遍历的标记为黑色。

写屏障（Write Barrier）： 记录对象的标记状态，且只针对未标记的对象。


### 多标与漏标的解决方案

多标：产生浮动垃圾，问题不大。
漏标：对象被回收。漏标的两个前提：
+ 对象在标记为**黑色**后**引用**了**白色**对象。
+ 对象被标记为**灰色**后**删除**了对白色对象的**引用**。

解决方案：
条件1（增量更新）：把黑色对象记录下来，之后以这些对象为根重新标记。
条件2（原始快照）：把白色对象记录下面，之后以这些对象为根重新标记。





















 


