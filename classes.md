与Java一样，在Kotlin中，使用关键字`class`来声明一个类。

### 如何创建
在Kotlin中创建类文件与在Java中的操作一致：
	
	// Java
	public class Empty {
	}

<line>
	
	// Kotlin
	class Empty{}
	// Or 
	class Empty

较之Java，Kotlin的方式可谓简洁（有关访问修饰符的内容参考Function章节）；仅使用`class`关键字和类名即可。当不需要构造函数(class header)和内容时，大括号可以省略。

### 构造函数
Kotlin的构造函数相对于Java而言发生了巨大的变化，包括构造函数重载在内：

	// Kotlin
	class Example constructor(name: String) {
	}
#### Primary Constructor
Kotlin中使用`constructor`关键字来表示构造函数，参数列表与普通函数的参数列表相同；如上例中所展示的被称为**primary constructor**，其中`constructor`关键字可以省略（部分使用注解的情况下不可省略，而且需要明确的访问修饰符，详情可参考官网[Classes And Inheritance](https://kotlinlang.org/docs/reference/classes.html)章节），同样的，`constructor`中的参数也支持默认参数值。
#### init {}
我们常常需要在构造函数中进行一些初始化的操作，因此Kotlin提供`init{}`块供我们进行必要的初始化操作：

	// Kotlin
	class Example(name: String) {
		init {
			println(name)
		}
	}

或者需要使用构造函数中的参数对某些属性进行赋值：
	
	// Kotlin
	class Example(name: String) {
		val value: String
		init {
			value = "Name: $name"
		}	
	}

如果你愿意的话，也可以写成下面这样：
	
	// Kotlin
	class Example(name: String) {
		val value = "Name: $name" 
	}

甚至，在不需要对**属性**进行额外操作的情况下，我们直接就可以写成下面这样：

	// Kotlin
	class Example(val name: String)

我们可以认为init块是Primary Constructor函数体的一部分。但Primary Constructor也并非必须存在的，只有在如上述示例中声明的构造函数才能被称为Primary Constructor。

#### Secondary Constructor
在Java中，我们往往需要不止一个构造函数，也时常根据需求来对构造函数进行重载，因此在Kotlin中对应的除了Primary Constructor，自然会存在有Secondary Constructor，值得注意的是，在Kotlin中，除去Primary Constructor之外的所有的构造函数，统称为Secondary Constructor，而且所有的Secondary Constructor都必须使用`constructor`关键字进行声明；这也是Kotlin中非常重要的概念。

	// Kotlin
	class Example(name: String) {

		constructor(size: Int) : this("Defalut Name") 
		constructor(name: String, size: Int) : this(name){
			// Do something ...
		}
	}
**注意**：所有的Secondary Constructor都必须要代理Primary Constructor；代理当前所在类的Primary Constructor时使用`this`关键字即可。这里的Secondary Constructor和Java中的重载构造函数非常的类似，可以在函数体内编写代码，没有代码时也可以省略大括号。同样需要说明的是，当Secondary Constructor与init同时存在时，init的执行会先于Secondary Constructor，即使在没有Primary Constructor的情况下仍然如此：
	
	// Kotlin
	class Example {
		init {
			println("Init block")
		}

		constructor(name: String) {
			println("Secondary Constructor")
		}
	}
上例中会优先输出**Init block**，而后输出**Secondary Constructor**，尽管编译器会提示你把Secondary Constructor转换为Primary Constructor。

### 补充
与Java相同的是，在不声明任何构造函数时，编译器会自动生成一个无参的构造函数，Kotlin也是如此，其访问可见性与当前类的访问修饰符保持一致。但在Java中，一旦显示的声明了有参构造函数后，我们仍然可以手动添加一个无参的构造函数；尽管在Kotlin中也可以以类似的方式处理，即Primary Constructor使用无参构造函数，Secondary Constructor相继代理无参主构造函数，但这并不是Kotlin所建议和支持的方式；而更好的实现方式，希望大家能够从官网或文档中寻找。

### 题外话
与函数一章类似，在结合Jvm注解的情况下，构造函数的操作也可以很另类；函数章节所涉及的只是最常用的情况，还有高级部分特性也会在日常开发中经常遇到，但这是后话了。