# RDD、DataFrame和DataSet的区别

### RDD ------------>弹性数据集

RDD是Spark建立之初的核心API。RDD是不可变分布式弹性数据集，在Spark集群中可跨节点分区，并提供分布式low-level API来操作RDD，包括transformation和action

使用RDD的一般场景：

- 需要使用low-level的transformation和action来控制数据集；
- 数据集非结构化；
- 使用函数式编程来操作你的数据，而不是用特定领域语言(DSL)表达；
- 不在乎schema，比如，当通过名字或者列处理(或访问)数据属性不在意列式存储格式；
- 放弃使用DataFrame和Dataset来优化结构化和半结构化数据集。

特性：

1. 是不变的数据结构存储
2. 支持跨集群的分布式数据结构
3. 可以根据数据记录的key对结构进行分区
4. 提供了粗粒度的操作，且这些操作都支持分区
5. 将数据存储在内存中，从而提供了低延迟性
		
### DataFrame

DataFrame是分布式数据和数据结构组成的组织集合，概念等同于关系型数据库里的表

DataFrame引入了schema和off-heap
	
- schema: RDD每一行的数据，结构都是一样的，这个结构就存储在schema中。Spark通过schame就能够读懂数据，因此在通信和IO时就只需要序列化和反序列化数据，而结构的部分就可以省略了
- off-heap: 意味着JVM堆以外的内存，这些内存直接受操作系统管理(而不是JVM)。Spark能够以二进制的形式序列化数据(不包括结构)到off-heap中，当要操作数据时，就直接操作off-heap内存。由于Spark理解schema，所以知道该如何操作

### DataSet
		
- Dataset是特定域对象中的强类型集合，它可以使用函数或者相关操作并行地进行转换等操作
- 每个Dataset都有一个称为DataFrame的非类型化的视图，这个视图是行的数据集	

优势：

- 静态类型和运行时类型安全
- High-level抽象以及结构化和半结构化数据集的自定义视图
- 简单易用的API
- 性能和优化
		
		
		
**什么时候使用DataFrame或者Dataset？**

+ 你想使用丰富的语义，high-level抽象，和特定领域语言API，那你可以使用DataFrame或者Dataset；
+ 你处理的半结构化数据集需要high-level表达，filter，map，aggregation，average，sum，SQL查询，列式访问和使用lambda函数，那你可以使用DataFrame或者Dataset；
+ 你想利用编译时高度的type-safety，Catalyst优化和Tungsten的code生成，那你可以使用DataFrame或者Dataset；
+ 你想统一和简化API使用跨Spark的Library，那你可以使用DataFrame或者Dataset；
+ 如果你是一个R使用者，那你可以使用DataFrame或者Dataset；
+ 如果你是一个Python使用者，那你可以使用DataFrame或者Dataset。		
		
		
		
		
### RDD、DataFrame与DataSet比较

**三者共性**

1. RDD、DataFrame、Dataset全都是spark平台下的分布式弹性数据集，为处理超大型数据提供便利
2. 三者都有惰性机制
3. 三者都会根据spark的内存情况自动缓存运算，这样即使数据量很大，也不用担心会内存溢出
4. 三者都有partition(分区)的概念
5. 三者有许多共同的函数

**DataFrame与DataSet共性**

1. DataFrame和Dataset均可使用模式匹配获取各个字段的值和类型
2. DataFrame与Dataset一般与spark ml同时使用
3. DataFrame与Dataset均支持sparksql的操作
4. DataFrame与Dataset支持一些特别方便的保存方式，比如保存成csv，可以带上表头
5. 在对DataFrame和Dataset进行操作许多操作都需要`spark.implicits._`这个包进行支持

**区别**

- RDD

	1. RDD不支持sparksql操作
	2. RDD对容错的支持，支持容错通常采用两种方式：数据复制或日志记录
		
			RDD天生是支持容错的。
			首先，它自身是一个不变的(immutable)数据集，
			其次，它能够记住构建它的操作图（Graph of Operation）
			因此当执行任务的Worker失败时，完全可以通过操作图获得之前执行的操作，进行重新计算。
			由于无需采用replication方式支持容错，很好地降低了跨网络的数据传输成本。
			

- DataFrame

	1. DataFrame每一行的类型固定为Row，只有通过解析才能获取各个字段的值

- Dataset

	1. Dataset中，每一行是什么类型是不一定的







