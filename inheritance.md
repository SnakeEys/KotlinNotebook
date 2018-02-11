Object Oriented Programming。在前面的章节中我们对数据类型封装已有所涉及，本章节主要对Kotlin中的继承进行介绍。

### 类的继承
在Java中，表示继承关系时使用`extends`关键字：

	class Bar {}

	class Foo extends Bar {}

在一般情况下，Java未使用`final`修饰符时，且只要该类在可见范围内，通常都可以被继承；然而在Kotlin中情况则变得不同：

	open class Bar

	class Foo : Bar()

Kotlin在默认情况下，类是不允许被继承的，必须使用`open`关键字表示该类是开放的；在表示继承关系时，也不使用`extends`关键字，取而代之的是冒号“`:`”操作符，这在前面的章节中也有讲解，不再详述；另外，与Java差异较大的是：即使父类未声明任何的构造函数，子类也需要显式的调用父类的无参构造函数（一级构造函数）。


### 函数重写
在Java中子类对父类的函数进行重写时，会在方法前添加`@Override`注解，当然这也是可以省略的。

	class Bar {
		...	
		void printName() {

		}
		...
	}

	class Foo extends Bar {
		
		...
		@Override
		void printName() {
			super.printName();
		}
	}

同样的，在Kotlin中，如果某个函数允许重写，也必须使用`open`修饰符，子类在重写函数时`override`作为关键字也**不能省略**：

	open class Bar {
	    open fun sayHello() {
	
	    }
	}
	
	open class Foo : Bar() {
	    override fun sayHello() {
	        super.sayHello()
	    }
	}

### 属性重写
作为Kotlin的一项特性；Kotlin支持子类重写父类中的属性，当然，`open`和`override`关键字是必不可少的：

	open class Bar {
		open val x: Int = 10
	}

	class Foo : Bar() [
		override var x: Int = 100
	}

在上例中我们看到，子类将父类中使用`val`修饰的属性，重写时使用`var`替换，但反之则不亦然。

#### 补充
`open`在Kotlin中只表示开放继承，但这*并不代表其子类必须对父类所提供的开放方法或属性进行重写*，若要求子类必须重写或实现，则需要使用`abstract`关键字，其用法也与Java中基本一致，但相对于Java，Kotlin允许对属性使用`abstract`关键字修饰，同时在使用`abstract`关键字时，不能像`open`一样对属性赋值：

	abstract class Bar {
	    abstract val x: Int
	
	    ...
	}
	
	open class Foo : Bar() {
	    override var x: Int = 11
	
	    ...
	}