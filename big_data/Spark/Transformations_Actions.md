# Transformations and Actions

### Transformations


| 	    Transformation	     | 			   Meaning            |
|:-------------------------:|:-------------------------------:|
|	map(func)					| 返回一个新的分布式数据集，该数据集通过func函数传递源的每个元素|
|	flatMap(func)				| 与map类似，但每个输入项都可以映射到0个或多个输出项(因此函数应该返回一个序列而不是单个项)|
|	filter(func)				| 对调用filter的RDD数据集中的每个元素都使用func，返回一个包含使func为true的元素构成的RDD|
|	mapPartitions(func)		| 与map类似，但是在RDD的每个分区(块)上单独运行，所以在一个类型为T的RDD运行时，函数必须是类型迭代器
|	mapPartitionsWithIndex(func)| 与mappartitions类似，但是也提供了一个表示分区索引的函数
|	sample(withReplacement, fraction, seed)| 使用给定的随机数生成器种子采样数据的一小部分，不管是否替换
|	union(otherDataset)		| 返回一个新的数据集，包含源数据集和给定数据集的元素的集合
| 	intersection(otherDataset)| 返回一个包含源数据集中的元素和参数的交叉口新的RDD
|	distinct([numTasks]))	| 返回一个包含源数据集的不同元素的新数据集。
|	groupByKey([numTasks])	| 返回(K,Seq[V])，也就是hadoop中reduce函数接受的key-valuelist|
|	reduceByKey(func, [numTasks])| 就是用一个给定的reducefunc再作用在groupByKey产生的(K,Seq[V]),比如求和，求平均数
|	aggregateByKey(zeroValue)(seqOp, combOp, [numTasks])||
|	sortByKey([ascending], [numTasks])||
|	join(otherDataset, [numTasks])||
|	cogroup(otherDataset, [numTasks])||
|	cartesian(otherDataset)||
|	pipe(command, [envVars])||
|	coalesce(numPartitions)||
|	repartition(numPartitions)||
|	repartitionAndSortWithinPartitions(partitioner)||


### Actions

| 	    	Action	 		    | 			   Meaning            |
|:------------------------:|:-------------------------------:|
|		reduce(func)			|通过函数func聚集数据集中的所有元素。Func函数接受2个参数，返回一个值。这个函数必须是关联性的，确保可以被正确的并发执行|
|		collect()				||
|		count()				|返回数据集的元素个数|
|		first()				|返回数据集的第一个元素(类似于take(1))|
|		take(n)				|返回一个数组，由数据集的前n个元素组成|
|takeSample(withReplacement, num, [seed])||
|takeOrdered(n, [ordering])||
|	saveAsTextFile(path)	|将数据集的元素，以textfile的形式，保存到本地文件系统，hdfs或者任何其它hadoop支持的文件系统。Spark将会调用每个元素的toString方法，并将它转换为文件中的一行文本|
|saveAsSequenceFile(path)(Java and Scala)||
|saveAsObjectFile(path)(Java and Scala)||
|		countByKey()			|返回的是key对应的个数的一个map，作用于一个RDD|
|		foreach(func)			|在数据集的每一个元素上，运行函数func|


























