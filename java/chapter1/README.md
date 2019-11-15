
java初中级开发工程师：扎实的java和计算机科学基础。  
java高级工程师或技术专家：全面考察java IO/NIO、并发、虚拟机等，要求对底层源代码层面的掌握，并对分布式、安全、性能等领域能力有进一步的要求。

java面试人员可能存在的问题：
- 知其然不知其所以然
- 知识碎片化，不成系统

5大模块：
- java基础：java语言基本特性和机制
- java进阶：并发编程、Java虚拟机等
- java应用开发扩展：数据库编程、主流开源框架、分布式开发等
- java安全基础：常见应用安全问题和处理方法
- java性能基础：掌握相关工具、方法论与基础实践


## 谈谈你对java平台的理解？"java是解释执行"，这句话对吗？

Java本身是一种面向对象的语言，最显著的特性有两个方面，一是所谓的"书写一次，到处运行"（Write once，run anywhere），能够非常容易地获得跨平台能力；另外
就是垃圾收集（GC，Garbage），Java通过垃圾收集器（Garbage Collector）回收分配内存，大部分情况下，程序员不需要操心内存的分配和回收。

JRE（Java Runtime Environment）或者JDK（Java Development Kit）。JRE也就是Java运行时环境，包含了JVM和Java类库，以及一些模块等。而JDK可以看作是JRE
的一个超集，提供了更多工具，比如编译器、各种诊断工具等。

"Java是解释执行"这句话，不太准确。我们开发的Java的源代码，首先通过Javac编译成为字节码（bytecode），然后，在运行时，通过Java虚拟机（JVM）内嵌的解释器将字节码
转换成为最终的机器码。但是常见的JVM，比如我们大多数情况使用的Oracle JDK提供的HotSpot JVM，都提供了JIT（Just-In-Time）编译器，也就是通常所说的动态编译器，
JIT能够在运行时将热点代码编译成机器码，这种情况下部分热点代码就属于编译执行，而不是解释执行。

## 请对比Exception和Error，另外，运行时异常和一般异常有什么区别？

Exception和Error都是继承了Throwable类，在Java中只有Throwable类型的实例才可以被抛出（throw）或者捕获（catch），它是异常机制的基本组成类型。

Exception和Error体现了Java平台设计者对不同异常情况的分类。Exception是程序正常运行中，可以预料的意外情况，可能并且应该被捕获，进行相应处理。

Error是指在正常情况下，不太可能出现的情况，绝大部分的Error都会导致程序（比如JVM自身）处于非正常的、不可恢复状态。既然是非正常情况，所以不便于也不需要捕获，常见的
比如OutOfMemoryError等，都是Error的子类。

Exception又分为可检查异常和不可检查异常，可检查异常在源代码里必须显式地进行捕获处理，这是编译期检查的一部分。不检查异常就是所谓的运行时异常，类似NullPointException、
ArrayIndexOutOfBoundsException之类，通常可以通过编码避免的逻辑错误，具体根据需要来判断是否需要捕获，并不会在编译期强制要求。

## 谈谈final、finally、finalize有什么不同？

final可以用来修饰类、方法、变量，分别有不同的意义，final修饰的class代表不可以继承扩展，final的变量是不可以修改的，而final的方法也是不可以重写的。

finally则是Java保证重点代码一定要执行的一种机制，我们可以使用try-finally或者try-catch-finally来进行类似关闭JDBC连接、保证unlock锁等动作。

finalize是基础类Object的一个方法，它的设计目的是保证对象在被垃圾收集前完成特定资源的回收。finalize机制现在已经不推荐使用，并且在JDK9开始被标记为deprecated。

## 强引用、软引用、弱引用、幻象引用有什么区别？

