## 小结

在过去的几篇章节中，与日常编程最紧密相关的内容基本都已经介绍完成，本章节是对前几篇章节中或有遗漏部分进行查漏补缺，也是对各章节进行补充和完善：

### 分号`;`与冒号`:`
每一篇对相关内容的讲解，实质上也是对语法的讲解；然而语法部分除语言本身强制的规范要求外，其余语法格式也仅为建议内容；
例如Kotlin在编写表达式时将分号`;`进行了省略，但如果一行有多句表达式语句，每句表达式之间的分号`;`则是万万不可省略的(但在每行的最后一句表达式之后仍可省略)：

	var a = "Hello";var b = 10	// Not suggested but this is allowed
	// or like below.
	var a = "Hello"
	var b = 10
	

在表示变量类型和函数的返回类型时，我们使用分号`:`进行表示，并在编码时会在`:`后加入空格便于识别，但写与不写，在个人习惯；同时为了区分二级构造函数（继承）与普通函数，在构造函数的分号前，通常也建议加入空格：

	// normal function
	fun foo(): Int {
		...
	}

	class Foo(name: String) {
		// secondary consturctor function
		constructor(name: String, age: Int) : this(name)
	}
	

### 条件与取反
在条件语句中，我们提到过使用`is`来判断类型，使用`in`来判断元素是否在集合中，对于相反的条件，在Java里需要使用`()`运算后再使用`!`运算符，而Kotlin则相对简化了许多：

	// Java
	void foo(CharSequence chars) {
		if (!(chars instanceof String)) {
			...
		}
	}

<Line>

	// Kotlin
	fun foo(chars: CharSequence) {
		if (chars !is String) {
			...
		} 
	}

在先前讲到的`when`表达式中，我们需要向`when`传递参数后进行判断，实际上，我们也可以省略参数，但是在每项分支条件下明确声明参数，只不过有些画蛇添足：

	when {
        args is Number -> println("Number")
        args is String -> println("Strings")
        args is List<*> -> println("List")
        args is Map<*, *> -> println("Map")
        args == 1 -> println("$args")
        else -> println("I don't know what it is.")
    }

但在对于集合或数组操作时，我们可以快速判断元素是否属于该集合：

	val items = setOf("apple", "banana", "kiwi")
    when {
        "orange" !in items -> println("juicy")
        "apple" !in items -> println("apple is fine too")
    }

### 循环
介绍`for`循环时，我们有讲过从x数到y，在某段范围内进行遍历：

	for (i in 0..100) {
		// Range from 0 to 100; like [0, 100], include 100.
	}

<line>

	for (i in 0 until 100) {
		// Range from 0 to 100; like [0, 100), NOT include 100.
	}

其实类似的操作被称之为范围操作：
	
	for (i in 1..10 step 2) {
		// Prints only 1, 3, 5, 7, 9.
	}
这里的`step`表示步长值，即每次遍历时要跨跃的元素（包含起始值）；例如这里步长值为2，第一次遍历时访问首个元素，第二次访问时跳过一个元素（包含了起始值）；即两个元素为一次遍历。值得注意的是，**步长值必须大于0**。

除去常见的升序操作，我们也需要进行降序操作：
	
	for (i in 10 downTo 1) {
		// Print from 10 to 1; include 1.
	}
注意，上述例子中`..`和`until`都是用于升序操作，这两个操作符前后两个参数中，`..`操作符后者必须**大于等于**前者，`until`操作符中后者参数必须**大于**前者，否则循环条件不满足；而`downTo`操作符用于降序，前后参数中后者必须**小于等于**前者，否则条件不满足。



### 字符串模板
日常编码中，我们时常会进行日志打印，正则表达式以及SQL操作等等一系列的场景，需要对字符串进行拼接操作，但之前所有的章节中，却并未见到类似Java的字符串拼接，基本上在Kotlin之中，利用字符串模板(String Templates)可以大大的方便对应的操作：

	var a = 1
	// simple name in template:
	val s1 = "a is $a" 

	a = 2
	// arbitrary expression in template:
	val s2 = "${s1.replace("is", "was")}, but now is $a"

SQL语句

	"""
		CREATE TABLE IF NOT EXISTS users 
		(id INTEGER PRIMARY KEY AUTOINCREMENT,
		 name TEXT UNIQUE,
		 ...
		)
	"""

输出日志
	
	var user = parseJson(json)
	Log.d(TAG, "$user")

