# Spark常用函数讲解之Action操作

1. reduce(func)

	`通过函数func先聚集各分区的数据集，再聚集分区之间的数据，func接收两个参数，返回一个新值，新值再做为参数继续传递给函数func，直到最后一个元素`

2. collect()

	`以数据的形式返回数据集中的所有元素给Driver程序，为防止Driver程序内存溢出，一般要控制返回的数据集大小`

3. count()

	`返回数据集元素个数`

4. first()

	`返回数据集的第一个元素`

5. take(n)

	`以数组的形式返回数据集上的前n个元素`

6. top(n)

	`按默认或者指定的排序规则返回前n个元素，默认按降序输出`

7. takeOrdered(n,[ordering])

	`按自然顺序或者指定的排序规则返回前n个元素`

8. countByKey()

	`作用于K-V类型的RDD上，统计每个key的个数，返回(K,K的个数)`

9. collectAsMap()

	`作用于K-V类型的RDD上，作用与collect不同的是collectAsMap函数不包含重复的key，对于重复的key。后面的元素覆盖前面的元素`

10. lookup(k)

	`作用于K-V类型的RDD上，返回指定K的所有V值`

11. aggregate



12. fold



13. saveAsFile(path:String)

	`将最终的结果数据保存到指定的HDFS目录中`

14. saveAsSequenceFile(path:String)

	`将最终的结果数据以sequence的格式保存到指定的HDFS目录中`











