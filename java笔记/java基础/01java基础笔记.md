###Java语言基础
* A:什么是跨平台性
	*不依赖于操作系统，也不依赖硬件环境。一个操作系统下开发的应用，放到另一个操作系统下依然可以运行。相对而言如果某种计算机语言不用修改代码即可做到高度跨平台，那么此语言就越抽象，硬件控制力就越低，只适合开发高度抽象的模型系统。
* B:Java语言跨平台原理
	* 只要在需要运行java应用程序的操作系统上，先安装一个Java虚拟机(JVM Java Virtual Machine)即可。由JVM来负责Java程序在该系统中的运行。
	* Java语言是跨平台的，但是JVM却不是跨平台的。
* C:Java语言跨平台图解
	* write once ,run anywhere!(一处编译,到处运行)

###Java语言基本概念
* JRE是什么
	* 包括Java虚拟机(JVM Java Virtual Machine)和Java程序所需的核心类库等，如果想要运行一个开发好的Java程序，计算机中只需要安装JRE即可。
	* JRE:JVM+类库。
* JDK是什么
	* JDK是提供给Java开发人员使用的，其中包含了java的开发工具，也包括了JRE。所以安装了JDK，就不用在单独安装JRE了。
	* 其中的开发工具：编译工具(javac.exe)  打包工具(jar.exe)等
 	* JDK:JRE+JAVA的开发工具。

###Java中的基本数据类型
* A: Java语言是强类型语言，对于每一种数据都定义了明确的具体数据类型，在内存中分配了不同大小的内存空间
* B: Java中数据类型的分类
	* 基本数据类型
	* 引用数据类型 
		* 面向对象部分讲解 
* C: 基本数据类型分类(4类8种) 
	* 整数型
		* byte 占一个字节  -128到127
		* short 占两个字  -2^15~2^15-1
		* int 占四个字节 -2^31~2^31-1
		* long 占八个字节 -2^63~2^63-1
	* 浮点型
		* float 占四个字节 -3.403E38~3.403E38  单精度
		* double 占八个字节-1.798E308~1.798E308 双精度
	* 字符型
		* char 占两个字节 0~65535
	* 布尔型
		* boolean   
* D：数据类型转换
	* 隐式数据类型装换
		*Java中的默认转换规则 取值范围小的数据类型与取值范围大的数据类型进行运算,会先将小的数据类型提升为大的,再运算 
		* a:int + int
		* b:byte + int  先将byte类型提升为int，然后进行相加运算
	* 强制数据类型转换
		
		 	int a = 10;
		 	byte b = 20; 
		 	b = a + b;   	// b = (byte)(a + b);

###基本数据类型的包装类
* 虽然Java语言是一种面向对象的语言，但是八种基本数据类型却并不支持面向对象编程，基本数据类型不能带有属性，没有方法可以调用，这种非面向对象的做法有时候会带来许多的不便之处。因此Java为每一种基本数据类型都包装了对应的引用数据类型。
	*	byte 	=====》		Byte
	*	int		=====》		Integer
	*	short	=====》		Short
	*	long	=====》		Long
	*	float	=====》		Float
	*	double	=====》		Double
	*	char	=====》		Character
	*	boolean	=====》		Boolean
* 装箱和拆箱

		Integer integer = 5;	//装箱		valueof(int)方法
		int i = integer;		//拆箱		xxxValue()方法
		

	