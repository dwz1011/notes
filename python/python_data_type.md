#python学习之数据类型
##字符串(string)
- 用引号括起的都是字符串,其中的引号可以是单引号, 也可以是双引号

1.使用方法修改字符串的大小写

	例：
	>>> name = "ada lovelace"
	>>> print name.title()
	Ada Lovelace
	>>> print(name.upper())
	ADA LOVELACE
2.合并(拼接)字符串

	例：
	>>> first_name = "ada"
	>>> last_name = "lovelace"
	>>> full_name = first_name + " " + last_name
	>>> print full_name
	ada lovelace
3.使用制表符或换行符来添加空白
	
	例:
	>>> print("Python")
	Python
	>>> print("\tPython")
		Python
	>>> print("\nPython\nhello")
	
	Python
	hello
	>>> print("\nPython\n\thello")

	Python
		hello
4.删除首尾空白

	例：
	>>> message = ' python '
	>>> message
	' python '
	>>> message.rstrip()
	' python'
	>>> message.lstrip()
	'python '
	>>> message.strip()
	'python'	
	
##数字
**1.整数(int)**

**2.浮点数(float)**

**3.长整型(long)**

**4.布尔型(bool)**

**5.复数型(complex)**

##列表(list)
- 列表由一系列按特定顺序排列的元素组成，用方括号([])来表示列表,并用逗号来分隔其中的元素

**访问列表元素，索引从0而不是1开始**

	例：
	>>> bicycles = ['trek', 'cannondale', 'redline', 'specialized']
	>>> print bicycles[0]
	trek
	>>> print bicycles[1]
	cannondale
	>>> print bicycles[-1]
	specialized
	
**修改、添加和删除元素**

	例：
	>>> motorcycles = ['honda', 'yamaha', 'suzuki']
	>>> print motorcycles
	['honda', 'yamaha', 'suzuki']
	修改	
		>>> motorcycles[0] = 'ducati'
		>>> print motorcycles
		['ducati', 'yamaha', 'suzuki']
	添加
		>>> motorcycles.append('ducati')
		>>> print motorcycles
		['honda', 'yamaha', 'suzuki', 'ducati']
		append只是在末尾添加元素
		
		在列表中插入元素用insert()
		>>> motorcycles = ['honda', 'yamaha', 'suzuki']
		>>> motorcycles.insert(0, 'ducati')
		>>> print motorcycles
		['ducati', 'honda', 'yamaha', 'suzuki']
	删除
		del方法----使用del语句将值从列表中删除后,你就无法再访问它了		
		>>> del motorcycles[0]
		>>> print motorcycles
		['yamaha', 'suzuki']

		pop()方法
		1.删除列表的最后一个元素
		>>> motorcycles.pop()
		'suzuki'
		>>> print motorcycles
		['honda', 'yamaha']
		2.指定元素删除
		>>> motorcycles.pop(0)
		'honda'
		>>> print motorcycles
		['yamaha', 'suzuki']

		remove()方法----删除第一个指定的值

**组织列表(排序)**

1.使用方法sort()对列表进行永久性排序
	
	例：
	>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
	>>> cars.sort()
	>>> print cars
	['audi', 'bmw', 'subaru', 'toyota']

	反向排序----sort()传递参数reverse=True
	>>> cars.sort(reverse=True)
	>>> print cars
	['toyota', 'subaru', 'bmw', 'audi']
	
2.使用函数sorted()对列表进行`临时`排序

- 调用函数sorted()后,列表元素的排列顺序并没有变

3.倒着打印列表

	例：
	>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
	>>> cars.reverse()
	>>> print cars
	['subaru', 'toyota', 'audi', 'bmw']
	
4.列表的长度

	>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
	>>> len(cars)
	4
	
**操作列表**

1.遍历整个列表

	例：
	>>> cars = ['bmw', 'audi', 'toyota', 'subaru']
	>>> for car in cars:
	...     print car
	...
	bmw
	audi
	toyota
	subaru
	
2.创建数值列表

 ① 使用函数range()

	例：
	>>> for i in range(1,5):
	...     print i
	...
	1
	2
	3
	4
 ② 使用range()创建数字列表

 	例： 
 	>>> num = list(range(1,5))
 	>>> print num
 	[1, 2, 3, 4]
 	
 3.对数字列表执行简单的统计
 	
 	例：
 	>>> digits = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
	>>> min(digits)
	0
	>>> max(digits)
	9
	>>> sum(digits)
	45
	
**使用列表的一部分**

1.切片

	例：
	>>> players = ['charles', 'martina', 'michael', 'florence', 'eli']
	>>> print players[1:3]
	['martina', 'michael']
	
2.遍历切片

	例：
	>>> players = ['charles', 'martina', 'michael', 'florence', 'eli']
	>>> for player in players[:3]:
	...     print player.title()
	...
	Charles
	Martina
	Michael
	
##元组(tupe)

- 不可变的列表被称为元组
- 元组看起来犹如列表,但使用圆括号而不是方括号来标识
- 元组的运算与列表一样

**修改元组元素**

元组中的元素值是不允许修改的，但我们可以对元组进行连接组合

	例：
	>>> t = (12, 13, 14, 15, 21, 22 )
	>>> t1 = t[0:3]
	>>> t2 = (19,)
	>>> t3 = t[4:]
	>>> t = t1 + t2 + t3
	>>> print t
	(12, 13, 14, 19, 21, 22)

**删除元组元素**

	例：
	>>> t = (12, 13, 14, 15, 21, 22 )
	>>> t1 = t[0:3] + t[4:]
	>>> t = t1
	>>> print t
	(12, 13, 14, 21, 22)
	
##字典(dict)

- 字典是另一种可变容器模型，且可存储任意类型对象
- 字典的每个键值(key=>value)对用冒号(:)分割，每个对之间用逗号(,)分割，整个字典包括在花括号({})中 

**访问字典**

	例：
	>>> dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
	>>> print dict['Name']
	Runoob
	
**修改字典**

	例：
	>>> dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
	>>> print dict
	{'Age': 7, 'Name': 'Runoob', 'Class': 'First'}
	>>> dict['Age'] = 11
	>>> print dict
	{'Age': 11, 'Name': 'Runoob', 'Class': 'First'}
	
**删除字典中的元素**

	例：
	>>> dict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
	>>> del dict['Name']
	>>> print dict
	{'Age': 7, 'Class': 'First'}
	
##集合(set)

- 无序不重复元素的序列
- 基本功能是进行成员关系测试和删除重复元素
- 可以使用大括号({})或者 set()函数创建集合

注意：创建一个空集合必须用 set() 而不是{}，因为{}是用来创建一个空字典
 
 	例：
 	>>> student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'}
	>>> print student
	set(['Mary', 'Rose', 'Jim', 'Jack', 'Tom'])


				