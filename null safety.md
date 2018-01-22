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
	var str1: String = "ABB"
	str1 = null // OK
	~~~