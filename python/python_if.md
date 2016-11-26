#python之if语句

###通用格式
	
	if <test1>:
		<do something>
	elif:
		<do something>
	else:
		<do something>
		
- if 语句的判断条件可以用>（大于）、<(小于)、==（等于）、>=（大于等于）、<=（小于等于）来表示其关系


**一些例子**

例1：

```
# -*- coding: UTF-8 -*-
flag = False
name = 'luren'
if name == 'python':        
    flag = True          
    print 'welcome boss'    
else:
    print name 

输出结果为：
>>> luren
```

例2：

```
# -*- coding: UTF-8 -*-
num = 5     
if num == 3:            
    print 'boss'        
elif num == 2:
    print 'user'
elif num == 1:
    print 'worker'
elif num < 0:           
    print 'error'
else:
    print 'roadman'
    
输出结果为：
>>> roadman
```

例3：

```
# -*- coding: UTF-8 -*-
num = 9
if num >= 0 and num <= 10:    
    print 'hello'
else:
	print 'undefine'

输出结果为：
>>> hello
```

###if-else多种写法
例：

```
a, b = 1, 2
1.常规
if a>b:
    c = a
else:
    c = b
print c

2.表达式
c = a if a>b else b
print c

3. 二维列表
c = [b,a][a>b]
print c
```
