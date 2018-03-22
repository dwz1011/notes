# Scala(三)

### 数组(Array and ArrayBuffer)

**定长数组(Array)**

	一：
	scala> var a:Array[String] = new Array[String](3)
			或 var a = new Array[String](3)
	a: Array[String] = Array(null, null, null)
	二：
		scala> var b = Array("a", "b", "c")
	b: Array[String] = Array(a, b, c)
	
	添加：
	scala> val c = "d" +: b
	c: Array[String] = Array(d, a, b, c)
	
**变长数组：数组缓冲(ArrayBuffer)**

	1.导入ArrayBuffer的包  
	import scala.collection.mutable.ArrayBuffer 
	
	2.定义变长数组ArrayBuffer  
	var arr = new ArrayBuffer[Int]();
	
	3.添加
	 +=  是在尾部添加元素
	 ++= 是往数组缓冲后面追加集合
		① 添加一个元素
			arr += 1
		② 添加多个元素
			arr += (2, 3, 4)
		③ 添加集合
			arr ++= Array(6,7,8,9)
		④ 指定位置添加元素
			arr.insert(3, 33) 
		⑤ 指定位置，添加多个元素，指定第一个值为index，后面所有的值为要添加的值  
			arr.insert(3,3,4,5,6)
		
	4. 删除	
		① 使用trimEnd(n)移除尾部n个元素 
			arr.trimEnd(3)
		② 使用trimStart(n)移除开头n个元素
			arr.trimStart(3)
		③ 移除某个位置的元素 
			arr.remove(3)
		④ 移除从下标为n开始（包括n）的count个元素
			arr.remove(3, 2)
		
ArrayBuffer 转 Array -------> ArrayBuffer.toArray
		
Array 转 ArrayBuffer -------> Array.toBuffer
	
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
	