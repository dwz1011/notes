# Scala

- 值和变量

	val修饰的是值 
	
		语法：val 值名称:类型 = xxx
			
		scala> val m = 100
		m: Int = 100
		
		scala> m = 1000
		<console>:12: error: reassignment to val
		       m = 1000
		         ^

	var修饰的是变量
			
		语法：var 变量名称:类型 = xxx
				         
		scala> var money: Int = 100
		money: Int = 100
		
		scala> money = 1000
		money: Int = 1000
		
	数据类型一般不需要显示指定，因为Scala编译器会自动推测出类型
必要的时候可以指定

- 数据类型

		Byte/Char
		Short/Int/Long/Float/Double
		Boolean
		String
		
		
	example
	
		scala> val a:Float = 1.1
		<console>:11: error: type mismatch;
		 found   : Double(1.1)
		 required: Float
		       val a:Float = 1.1
		                     ^
		
		scala> val a:Float = 1.1f
		a: Float = 1.1
		
		Int转Double, String
		scala> val b = 10
		b: Int = 10
		
		scala> b.toDouble
		res1: Double = 10.0
		
		scala> b.toString
		res3: String = 10

- lazy 定义惰性变量，实现延迟加载(懒加载)

	使用lazy关键字修饰变量后，只有在使用该变量时，才会调用其实例化方法

		scala> lazy val c = 1
		c: Int = <lazy>
		
		scala> c
		res4: Int = 1
			
- 函数	

	def FunName(x:Int, y:String) : Int{
	}
	
		def 函数定义的开始
		FunName 函数名称
		() 定义入参，多个参数以,分割
		: Int  定义返回值类型
		{} 方法体，如果方法体内就一行代码，{}可以省略
		
	`Scala函数中是把方法体的最后一行作为返回值，不需要显示调用return`

- 区间(to,Range,until)

		底层都是用range实现的	

		to       []
			1 to 10 ==> 1.to(10)
		
			scala> 1 to 10
			res5: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

			scala> 1.to(10)
			res6: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
			
		Range    [)
			scala> Range(1, 10)
			res7: scala.collection.immutable.Range = Range(1, 2, 3, 4, 5, 6, 7, 8, 9)
		
		until    [)	
		
		
		
		

		






















		
		
		
		
		
		
		
		
		
		
		
		
		
		