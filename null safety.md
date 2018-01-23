烦人的NPE，怎么也逃不掉。

### 空与不空
Kotlin意在消除代码产生NPE的危险；在Java中，如果不小心使用了空引用对象，那么很不幸，程序会立刻抛出`NullPointerException`；在Kotlin中，能够抛出`NPE`的情况如下：

- 手动调用，即`throw NullPointerException`；
- 使用断言操作符`!!`，下文介绍；
- 未在构造函数或者初始化函数中对数据进行初始化操作；
- 与Java进行互操作时
	- 调用(`null`)空引用；
	- 泛型参数引，例如Java中的`List<String>`可以将`null`作为元素添加，然而在Kotlin中的`MutableList<String>`是不允许添加`null`的，Java中的情况到Kotlin应该被认为是`MutableList<String?>`；
	- 其它情况。  
	
总而言之，都怪Java。

那么在Kotlin中系统是如何区分引用是否可以被`null`赋值，哪些又不能被`null`赋值？比如在Java中可以对String赋值为`null`，或者说任何对象数据类型的初始值都是`null`。

	~~~ Java
	String str = null;
	// which equals to 
	String str1;
	~~~

情况到了Kotlin之后就完全不同了：首先，我们在`var`章节中提到过，向变量重新赋值，必须使用`var`修饰符：
	
	~~~ Kotlin
	var str: String = "ABC"
	str = null // Error
	~~~
允许变量为空，需要在数据类型后使用`?`操作符：

	~~~ Kotlin
	var str1: String = "Hello"
	str1 = null // OK
	~~~
上述两份示例代码中，在访问`str`变量时可以确保不会有NPE的产生，比如：
	
	~~~ Kotlin
	val length = str.length // OK
	~~~
而在访问`str1`变量时，是不安全的操作，有可能产生NPE，因此编译无法通过：
	
	~~~ Kotlin
	val length1 = str1.length // ERROR
	~~~ 

尽管是不安全的，但我们仍然需要对其进行访问和操作，因此要对操作对象进行强制性的空校验：
	
	~~~ Kotlin
	val l = if (str1 != null) str1.length else -1
	~~~

编译器会自动检查是否已执行空校验，所以在非空的条件分支语句中可以安全地操作对象：
	
	~~~ Kotlin
	if (str1 = null && str1.length > 0) {
		println("String of length: ${str1.length}")
	} else {
		println("Empty string")
	}

#### 安全调用
不想写`if`？那么可以使用安全操作符`?.`：

	~~~ Kotlin
		str1?.length
	~~~
如果`str1`为空，则返回的数据类型也是`Int?`类型，否则会直接返回`str1`的长度。
#### 链式调用
有种情况下，我们会要链式调用长串的方法，比如：

	~~~ Kotlin
	val temp = a?.b?.c?.x
	~~~
看上去不好理解？把上面的代码完整写下来就很直观了：

	~~~ Kotlin
	
	class A(var b: B?)
	
	class B(var c: C?)

	class C(var x: String?)

	~~~
链式调用在遇到任何空属性时均会中断且直接返回`null`而不会抛出NPE。如果只需要对非空情况下处理而不需要处理空的情况时，可以使用`let`操作符：
	
	~~~ Kotlin
	val list: List<String?> = listOf("A", "B", null)
	for (item in list) {
		item?.let {
			println(it)
		}
	}
	~~~
最后要引起注意的是，当要给链接调用的某个属性赋值时，遇到任何一环为空，则整行函数或表达式均不执行，例如：

	~~~ Kotlin
	a?.b?.c?.x = "ABC"
	~~~

#### Elvis操作符
在前文中我们提到了强制空校验，或者直接返回`null`。但是如果接收方是不允许为可空类型时，除了使用强制`if`空校验给出默认值，还可以使用Elvis操作符`?:`:
	
	~~~ Kotlin
		val l: Int = str1?.length ?: -1
	~~~ 
在Control Flow章节中，我们提到过Java的三目运算符在Kotlin中变为了`if`表达式，然而Kotlin的Elvis操作符在形式上十分接近Java的三目运算符，但两者功能想去甚远，Kotlin中仅用于空校验；

同时由于Kotlin将`return`和`throw`作为表达式，因此在某些情况下我们也可以用以替代`if`表达式：

	~~~ Kotlin
		fun foo(node: Node): String? {
			val parent = node.getParent() ?: return null
			val name = node.getName() ?: throw IllegalArgumentException("Name expected")
		}

	~~~

#### 断言操作符
某些时候，写连串的`?`难免会让代码看上去不那么舒服；因此Kotlin提供了断言操作符`!!`；当然在遇到`null`时仍然会抛出NPE：

	~~~ Kotlin
		val l = str1!!.length	// NPE happens if str1 is null
	~~~

#### 安全转型
除了NPE，在Java中我们时常需要进行强制转型，而如果将`null`强制进行转型，则会引发`ClassCastException`，尽管可以在转换前进行`instance of`/`is`判断，但我们仍可以对其进行简化，尤其是在Kotlin中我们很可能会遇到这样的情况：

	~~~ Kotlin
		val aInt: Int? = a as? Int
	~~~

### 补充
除了之前章节中所说过的基础类型不能自动进行转型之外，可空类型和不可空类型也不能进行自动转换，比如上例所述。