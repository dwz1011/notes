# NumPy
**以下IDE为ipython**

### 创建ndarray

- np.array(collection),collection为`序列型`对象(list)，嵌套序列(list of list)
- np.zeros, np.ones, np.empty 指定大小的全0或全1数组
	- 注：第一个参数是元组，用来指定大小
	- empty不总是返回全0，有时返回的是未初始的随机值
- np.arange()

例：

	1.一维数组
	In [1]: import numpy as np
	In [2]: data1 = [6, 7.5, 8, 0, 1]
	In [3]: arr1 = np.array(data1)
	In [4]: arr1
	Out[4]: array([ 6. ,  7.5,  8. ,  0. ,  1. ])
	2.二维数组
	In [5]: data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
	In [6]: arr2 = np.array(data2)
	In [7]: arr2
	Out[7]:
	array([[1, 2, 3, 4],
	       [5, 6, 7, 8]])
	       
	In [8]: np.zeros((3,4))
	Out[8]:
	array([[ 0.,  0.,  0.,  0.],
	       [ 0.,  0.,  0.,  0.],
	       [ 0.,  0.,  0.,  0.]])

	In [9]: np.ones((2,3))
	Out[9]:
	array([[ 1.,  1.,  1.],
	       [ 1.,  1.,  1.]])
	
	In [10]: np.empty((3,3))
	Out[10]:
	array([[  0.00000000e+000,   0.00000000e+000,   6.92125962e-310],
	       [  6.92125775e-310,   6.92125907e-310,   6.92125157e-310],
	       [  0.00000000e+000,   0.00000000e+000,   6.92125953e-310]])
	
	In [11]: np.empty((3,3), int)
	Out[11]:
	array([[0, 0, 0],
	       [0, 0, 0],
	       [0, 0, 0]])


### ndarray, N维数组对象

- 所有元素必须是相同类型
- ndim属性，维度个数
- shape属性，各维度大小
- reshape, 注:大小必须一致

例：

	In [14]: arr2.ndim
	Out[14]: 2
	In [15]: arr2.shape
	Out[15]: (2, 4)
	In [16]: arr2.reshape(4,2)
	Out[16]:
	array([[1, 2],
	       [3, 4],
	       [5, 6],
	       [7, 8]])

### ndarray数据类型

- dtype，类型名+位数
- 转换数组类型
	- astype

例：
	
	In [24]: arr2.dtype
	Out[24]: dtype('int64')
	In [25]: arr2.astype('string')
	Out[25]:
	array([['1', '2', '3', '4'],
	       ['5', '6', '7', '8']],
	      dtype='|S21')

### 矢量化

例：

	In [32]: arr = np.array([[1., 2., 3.], [4., 5., 6.]])
	In [33]: arr
	Out[33]:
	array([[ 1.,  2.,  3.],
	       [ 4.,  5.,  6.]])
	In [34]: arr * arr
	Out[34]:
	array([[  1.,   4.,   9.],
	       [ 16.,  25.,  36.]])
	In [35]: arr - arr
	Out[35]:
	array([[ 0.,  0.,  0.],
	       [ 0.,  0.,  0.]])
	In [36]: 1/arr
	Out[36]:
	array([[ 1.        ,  0.5       ,  0.33333333],
	       [ 0.25      ,  0.2       ,  0.16666667]])
	In [37]: arr ** 0.5
	Out[37]:
	array([[ 1.        ,  1.41421356,  1.73205081],
	       [ 2.        ,  2.23606798,  2.44948974]])

### 索引与切片

- 一维数组

例：

	In [38]: arr3 = np.arange(10)
	In [39]: arr3
	Out[39]: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
	In [40]: arr3[5]
	Out[40]: 5
	In [41]: arr3[5:8]
	Out[41]: array([5, 6, 7])
	In [42]: arr3[5:8] = 12
	In [43]: arr3
	Out[43]: array([ 0,  1,  2,  3,  4, 12, 12, 12,  8,  9])
	
- 多维数组----各索引位置上的元素不再是标量，而是一维数组

例：

	In [44]: arr4 = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
	In [45]: arr4
	Out[45]:
	array([[1, 2, 3],
	       [4, 5, 6],
	       [7, 8, 9]])
	In [46]: arr4[2]
	Out[46]: array([7, 8, 9])
	In [47]: arr4[0][2]			等价于:	   In [48]: arr4[0, 2]
	Out[47]: 3								 Out[48]: 3
	
- 条件索引
	- 布尔值多维数组
		- 注：多个条件组合要使用 & |，而不是and or
		
例：
	
	In [50]: year_arr
	Out[50]:
	array([[2000, 2001, 2000],
	       [2005, 2002, 2009],
	       [2001, 2003, 2010]])
	In [51]: year_arr >= 2005
	Out[51]:
	array([[False, False, False],
	       [ True, False,  True],
	       [False, False,  True]], dtype=bool)
	In [52]: data_arr = np.random.rand(3,3)
	In [53]: data_arr
	Out[53]:
	array([[ 0.41695636,  0.1500084 ,  0.1424562 ],
	       [ 0.90233294,  0.18459908,  0.77189913],
	       [ 0.04190812,  0.68878862,  0.91359385]])
	1.单个条件索引
	In [54]: data_arr[year_arr >= 2005]
	Out[54]: array([ 0.90233294,  0.77189913,  0.91359385])	2.多个条件索引
	In [56]: data_arr[(year_arr >= 2005) & (year_arr % 2 == 0)]
	Out[56]: array([ 0.91359385])

### 维数转换

- 转置 transpose()

例：

	In [58]: arr5 = np.random.rand(2,3)
	In [59]: arr5
	Out[59]:
	array([[ 0.96528126,  0.01189675,  0.43198019],
	       [ 0.74033865,  0.88163279,  0.01505989]])
	In [60]: arr5.transpose()
	Out[60]:
	array([[ 0.96528126,  0.74033865],
	       [ 0.01189675,  0.88163279],
	       [ 0.43198019,  0.01505989]])
	       
- 高维数组转置要指定维度编号

### 通用函数()

常用的通用函数

- ceil，向上最接近的整数
- floor，向下最接近的整数
- rint，四舍五入
- isnan，判断元素是否为NaN
- multiply，元素相乘
- divide，元素相除

np.where

- 矢量版本的三元表达式 x if condition else y
- np.where(condition, x,y)

常用统计方法

- np.mean, np.sum
- np.max, np.min
- np.std, np.var
- np.argmax, np.argmin
- np.cumsum, np.cumpod
- 注：多维的话指定统计的维度，否则默认是全部维度上做统计
- np.all, np.any	生成bool
- np.unique

### 操作文件读取

- 读取
	- np.loadtxt(参数)
	- 参数：
		- delimiter = '', 分隔符
		- dtype = '', 数据类型
		- usecols = (), 指定读取的列索引号
	

