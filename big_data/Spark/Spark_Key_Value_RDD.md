# Spark常用函数讲解之键值RDD转换


1. mapValus(fun)

	`对[K,V]型数据中的V值map操作`
	
2. flatMapValues(fun)

	`对[K,V]型数据中的V值flatmap操作`

3. comineByKey(createCombiner,mergeValue,mergeCombiners,partitioner,mapSideCombine)

	```
	createCombiner:在第一次遇到Key时创建组合器函数，将RDD数据集中的V类型值转换C类型值(V => C)
	mergeValue:合并值函数，再次遇到相同的Key时，将createCombiner道理的C类型值与这次传入的V类型值合并成一个C类型值（C,V）=>C
	mergeCombiners:合并组合器函数，将C类型值两两合并成一个C类型值
	partitioner:使用已有的或自定义的分区函数，默认是HashPartitioner
	mapSideCombine:是否在map端进行Combine操作,默认为true
	```

4. foldByKey



5. reduceByKey(func,numPartitions)

	`按Key进行分组，使用给定的func函数聚合value值, numPartitions设置分区数，提高作业并行度`


6. groupByKey(numPartitions)

	`按Key进行分组，返回[K,Iterable[V]]，numPartitions设置分区数，提高作业并行度`

7. sortByKey(accending，numPartitions)

	`返回以Key排序的(K,V)键值对组成的RDD，accending为true时表示升序，为false时表示降序，numPartitions设置分区数，提高作业并行度`

8. cogroup(otherDataSet，numPartitions)

	`对两个RDD(如:(K,V)和(K,W))相同Key的元素先分别做聚合，最后返回(K,Iterator<V>,Iterator<W>)形式的RDD,numPartitions设置分区数，提高作业并行度`

9. join(otherDataSet,numPartitions)

	`对两个RDD先进行cogroup操作形成新的RDD，再对每个Key下的元素进行笛卡尔积，numPartitions设置分区数，提高作业并行度`

10. LeftOutJoin(otherDataSet，numPartitions)

	`左外连接，包含左RDD的所有数据，如果右边没有与之匹配的用None表示,numPartitions设置分区数，提高作业并行度`

11. RightOutJoin(otherDataSet, numPartitions)

	`右外连接，包含右RDD的所有数据，如果左边没有与之匹配的用None表示,numPartitions设置分区数，提高作业并行度`

