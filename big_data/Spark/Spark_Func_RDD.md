# Spark函数详解系列之RDD基本转换


1. map(func)

	`数据集中的每个元素经过用户自定义的函数转换形成一个新的RDD，新的RDD叫MappedRDD`
	
2. flatMap(func)

	`与map类似，但每个元素输入项都可以被映射到0个或多个的输出项，最终将结果”扁平化“后输出`
	
3. mapPartitions(func)
	
	`类似与map，map作用于每个分区的每个元素，但mapPartitions作用于每个分区工`
	
4. mapPartitionsWithIndex(func)

	`与mapPartitions类似，不同的时函数多了个分区索引的参数`
	
5. sample(withReplacement,fraction,seed)

	`以指定的随机种子随机抽样出数量为fraction的数据，withReplacement表示是抽出的数据是否放回，true为有放回的抽样，false为无放回的抽样`
	
6. union(ortherDataset)

	`将两个RDD中的数据集进行合并，最终返回两个RDD的并集，若RDD中存在相同的元素也不会去重`
	
7. intersection(otherDataset)

	`返回两个RDD的交集`

8. distinct([numTasks])

	`对RDD中的元素进行去重`
	
9. cartesian(otherDataset) 

	`对两个RDD中的所有元素进行笛卡尔积操作`	

10. coalesce(numPartitions，shuffle)

	`对RDD的分区进行重新分区，shuffle默认值为false,当shuffle=false时，不能增加分区数
目,但不会报错，只是分区个数还是原来的`

11. repartition(numPartition)

	`是函数coalesce(numPartition,true)的实现，效果和例9.1的coalesce(numPartition,true)的一样`
	
12. glom()

	`将RDD的每个分区中的类型为T的元素转换换数组Array[T]`
	
13. randomSplit(weight:Array[Double],seed)

	`根据weight权重值将一个RDD划分成多个RDD,权重越高划分得到的元素较多的几率就越大`
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	