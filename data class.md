你以为class就只有那么点内容吗？

### 数据类
在Java中，和服务端进行数据交互时免不了要写许多的Java Bean对象来对数据进行封装。虽然IDE会帮我们生成程式化的模板代码，但少不了的是setters和getters。而这一情况在Kotlin中发生了翻天覆地的变化：

	// Java
	public class User {
		private String name;
		private String age;

		...setters
		...getters
		...toString()...
	}

<line>

	// Kotlin
	data class User(var name: String = "John", var age: Int = 18)

相较于Java而言，Kotlin极大的简化了Java Bean的代码量，同时也提升了其阅读性；接下来我们将对`data class`的特点进行总结：

 - 使用`data`关键字；
 - 自动为Primary Constructor中的所有属性代理setters/getters；
 - 默认参数时`toString()`时的结果`User(name="John", age=18)`；
 - `equals()`/`hashCode()`对应；

使用`data class`时还需要有如下事项需要注意：

 - Primary Constructor中至少要有一个参数
 - 所有的构造函数中的参数必须使用`var`或`val`修饰符；
 - 数据类不能使用`abstract`，`open`，`sealed`，`inner`(*这些都是后面要介绍的类*)修饰符；

#### 优势
假如`data class`带来了好处只是省代码，那么并不算特别的优势；数据类在继承其它类时，还可以对父类的属性进行重写，此处只给出示例简要介绍，后续章节会详细对属性重写进行说明:

	// Kotlin
	open class Human {
		open var name: String = ""
	}

	data class Man(var age: Int) : Human() {
		 override var name: String = "Smith"
        		get() = "name: $field"
	}
除此之外，某些情况下我们需要对某些属性进行复制，但又需要保持原属性不变，`data class`会自动生成`copy()`方法从而免去手动创建一个新对象，以最开始的`User`类作为示例：

	// Kotlin
	fun copy(name: String = this.name, age: Int = this.age) = User(name, age)    

因此可以有如下操作
	// Kotlin
	val john = User()
	val olderJohn = john.copy(age = 20)

#### [解构声明](https://kotlinlang.org/docs/reference/multi-declarations.html)
由于编译器会自动为数据类生成组件函数（component functions），所以我们可以很方便地对数据类进行解构操作：

	// Kotlin
	val user = User()
	val (name, age) = user
	println("name is $name, age is $age")
Tips：示例代码中的`name和age`，相当于局部变量名，不需要和User类中的属性名进行对应。
数据类的内容并不多，可以尝试在自己项目中将一些比较简单的Java Bean修改为data class。