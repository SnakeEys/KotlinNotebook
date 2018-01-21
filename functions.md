今天开始给大家讲Kotlin的入门，从最基础的部分开始，每个部分都会以Java为出发点，方便大家理解，目的是为了能够在最快的时间内熟悉和使用Kotlin。Java和Kotlin可以进行100%的互操作，混用代码没有任何问题。所以各位可以放心使用。


首先讲的内容是函数（方法）；在Java中声明函数和在Kotlin中声明函数的区别，我把区别做了如下表对比：  

	|---------------| --------------| --------------- |
	|				|  Java  		|  Kotlin  		  |
	|---------------| --------------| --------------- |
	|			    |    public		| public(default) |  
	| 	Modifier	|   private  	|    private      |
	|	  可见性     |   protected   |    protected    |
	|		     	|    default	|    internal	  |
	|			 	|				|      open		  |
	|---------------|---------------|---------------- |
	|				|  hello		|   fun hello	  |
	|	函数名称     |      ...      |     fun ...     |
	|---------------|---------------|-----------------|
	|				| (String str)  | (str: String)   |
	|   参数列表     | (int i)       | (i: Int)        |
	|	(params)	| (Object... orr| (vararg o: Any) |
	|				|    .....      |     .....       |
	|---------------|---------------|-----------------|
    |				| void          |     Unit        |
	|  返回值        | String        |     String      |
	|				| int           |      Int        |
	|				|    Object     |       Any       |
	|				|    ......     |      ....       |
	|---------------|---------------|-----------------|
     
	
每个不同点都会单独讲解；Java方面大家都熟，所以直接跳过。

#### 访问修饰符：
Kotlin默认（缺省）都是`public`的，`private`和`protected`与Java中保持一至，区别在于Java中的缺省状态下是同包可见，Kotlin目前没尚未支持，`internal`修饰符在同一个module内相当于`public`，也就是说`internal`的可见范围是以module为界限。而关于`internal`，当编写的jar包中有使用`internal`时，其可见性仍然相当于`public`，后续有文章会发出来供大家学习参考。
open修饰符大家可以暂时忽略，到后面讲到OOP时会进行讲解。


#### 函数名（方法名）
Kotlin的方法命名与Java相同；只不过在方法名前面多了一个`fun`*（function）*表示方法。

#### 参数列表
与Java区别相反的是，Kotlin的参数类型是放在形参名的后方。方法名称和参数列表共同构成方法的签名，这一点在字节码中的表现与Java一致。
值得注意的是，在Kotlin中，**参数列表中所传入的参数都是不可变的**，相当于在Java中使用了`final`修饰符。也就是说在方法作用域内，传入的参数是不可以被重新赋值的；而Java是可以进行重新赋值，比如在listview adatper的getView()中，我们需要对convertView进行赋值，这一点在Kotlin中是不被允许的，对此的解决方案也非常简单，此处不作透露，希望大家自己探索。


#### 返回类型
也与Java差异最明显的部分：Kotlin的函数返回值是写在方法最后面（参数列表后方），如果没有返回值，可以不声明，也可以使用`Unit`表示该函数没有任何的返回；并且Kotlin对于返回值进行了简化：  

	// Kotlin
	fun hello() {
	    println("Hello world")
	}

	fun hello(args: String): Unit {
	    println("""Input args is "$args".""")
	}

	fun helloWithResult(): String {
	    return "Hello world."
	}

	private fun helloWithParam(vararg args: String) {
	    println(args)
	}

	internal fun helloWithResult2() = "Hello world."

当函数体只有一句表达式语句时，可以直接在函数后面使用`=`表示该函数的返回值，且不需要声明返回的类型（参考上例最后一个函数）。完整的函数带返回值可参看上例中第三个函数。

###更多
- 关于函数的重写  
在Kotlin中，所有的函数缺省状态都是被`final`修饰的，后续讲解继承时，会专门讲到`open`修饰符以及函数的重写，此处就先暂只提及。
- 关于重载  
熟悉python的朋友应该知道在python中是可以给参数给默认值的，Kotlin汲取了这一优势；也就是说，在Java中对方法进行重载时需要声明同样的方法，但参数列表不同；而Kotlin则只需要对参数列表中的参数给出相应类型的缺省值即可达到重载函数的目的；调用时也与Python类似，对形参名进行赋值即可，这样可以避免大量的重复编写代码。  

		// Kotlin
		fun overloadFun(name: String = "John", age: Int = 18) {
			...
		}

		fun main(args: Array<String>) {
			overloadFun()
			overloadFun("Tiger Smith", 33)
			overloadFun(age = 17)
      		overloadFun(name = "Jack")
		}


- 关于静态函数  
Kotlin没有静态函数一说，后续讲解`companion`（伴生）时会有相关介绍；尽管在编译为字节码后我们可以看到static的修饰符，但是Kotlin是没有static修饰符的。
- 补充  
Kotlin的函数是直接支持包级声明的（package-level），即不依赖任何的class。比如main函数就是直接写在包一级，而不需要写在某个类内部。Kotlin赋予了函数非常多的骚操作，以上内容只是在实际应用中最常见的情况了。还有非常多的深入内容等待大家去学习。  

		// Kotlin
		package cn.kotliner
		import ....
		

		fun main(args: Array<String>) {
			....
		} 