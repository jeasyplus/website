# JVM结构

## 类加载器子系统（Class Loader Subsystem）：

负责加载Java类文件（.class文件）到内存中，并将其转换成运行时的数据结构。JVM使用三个类加载器：启动类加载器（Bootstrap Class Loader）、扩展类加载器（Extension Class Loader）和应用程序类加载器（Application Class Loader）来加载类。

## 运行时数据区（Runtime Data Area）：

JVM在内存中划分了不同的数据区域用于存储不同类型的数据和执行操作。主要包括：

## 方法区（Method Area）：

用于存储类的结构信息、常量池、静态变量、即时编译器生成的代码等。

## 堆（Heap）：

用于存储对象实例和数组，是Java程序运行时动态分配内存的地方。

## 栈（Stack）：

用于存储方法的局部变量、操作数栈、方法调用和返回信息等。每个线程在运行时都有一个独立的栈。

## 本地方法栈（Native Method Stack）：

与Java栈类似，但用于执行本地（Native）方法。

## 程序计数器（Program Counter）：

用于记录当前线程执行的位置，也就是下一条指令的地址。

## 执行引擎（Execution Engine）：

负责执行编译后的Java字节码。它有不同的实现方式，包括解释器和即时编译器（Just-In-Time Compiler，JIT）。

解释器逐条解释字节码并执行，而即时编译器会将字节码转换为本地机器代码，以提高执行效率。

## 本地接口（Native Interface）：

允许Java代码调用本地（操作系统或其他语言编写的）代码。

它为Java提供了与底层系统交互的能力。

## 垃圾收集器（Garbage Collector）：

负责在运行时自动回收不再使用的内存空间，避免内存泄漏和程序崩溃。