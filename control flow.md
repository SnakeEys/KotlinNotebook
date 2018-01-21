两个判断，三个循环（1）。


### 两个判断
程序中总是存在各式各样的逻辑，在Kotlin中，`if...else`和Java中扮演着同样的角色。只不过在某些情况下还是存在着些许不同，例如在Java中我们使用非常多的三目运算符`?:`：

	// Java
	int max = a;
	max = a < b ? b : a;
	
    
在Kotlin中反而不能使用，取而代之的是`if`表达式：

	// Kotlin
	val max = if (a > b) a else b   
较之于Java而言，Kotlin的`if`表达式在赋值语句中甚至可以直接编写代码块，这一点相对于三目运算符而言，尤其是在打印日志的时候极其有用，默认每个分支代码块的最后一句表达式即为该代码块的返回值：

	// Kotlin
	val max = if (a > b) {
			println("Choose $a")
			a
		} else {
			println("Choose $b")
			b
		}	
从整体而言，Kotlin和Java中的`if`并没有很明显的区别，使用习惯几乎没有发生任何变化；常用的三目运算符过度到`if`表达式也不会有不习惯。  

至于`switch`关键字，在Kotlin中则无法使用，取而代之的是更为强大的`when`表达式。

#### When
在Java中，`switch`的使用频率虽不及`if`，但在某些情况下会大大的简化代码逻辑，例如Android开发中，Activity之间的交互码`RESULT_OK`的判断，HTTP响应码等情况；`switch`虽然能将代码块进行简化，但其功能较为单一，只能处理Java基础类型和枚举，尽管在1.7之后也可以支持字符串，但在`case`中的条件仍然为`switch()`中的参数类型所限制：

	// Java
	switch(CODE) {
		case RESULT_OK:
		...
		break;
		
		....
		default:
		break;
	}
	
而情况换言到Kotlin中之后，则完全是另一番景象，首先，将`when`当成`switch`来用不会有任何问题，比如上面的例子：

	// Kotlin
	when (CODE) {
		1 -> println("Code is $CODE")
		2 -> println("2")
		...
		else -> {....}
	}
	
其次，`when`表达式也可以多个条件下执行同一段代码块：

	// Kotlin
	when (CODE) {
		0, 1, 2 -> {...}
		else -> ...
	}
甚至还支持使用迭代语法作(`in`, `!in`)为分支条件：

	// Kotlin
	when (CODE) {
		in 0..15 -> {...execute this code block...}
		!in 22..33 -> {...}
		else -> {...}
	}

要是你愿意，还可以将任意的函数作为分支条件：
	
	// Kotlin
	when (CODE) {
		parseInt(s) -> println("s encodes CODE $CODE")
		else -> {...}
	}
某些情况下我们需要判断传入参数的类型，通常我们在Java使用 `instance of `操作符在`if`条件中判断，而在Kotlin中，除了像Java中使用`if`的方式处理，`when`也能够胜任：

	// Kotlin
	when (exception) {
		is ArithmeticException -> {}
		is IOException -> {}
		is NullPointerException -> {}
		...
		else -> {}	
	}
可是`when`的强大还不止于此，它还可以支持混合数据类型的判断，比如传入的参数完全未知甚至可能为空时，`when`也能应付自如：

	// Kotlin
	when (args) {
		null -> println("Null")
		is String -> println("String type")
		in 0..10 -> println("$args is in range 0 to 10")
		...
		else -> println("$args can not be caught.")
	}
	
要知道，Java中的`switch`是无法处理空参数的，即当传入的参数为空时会抛出`NullPointerException`，而在上面的例子中`when`是可以处理的，极大地提升了程序的稳定性以及代码的可读性。  
总结下来，`when`的强大已令`switch`相形见绌，既可以当成普通的`switch`，又可以灵活作为作为一部分`if`的替代；而在使用中也没有特殊要求，为了更快更方便的理解，分清以下情况即可：

- 条件分支为常量值时，无需使用任何操作符；
- 条件分支为类型判断时，使用`is`/`!is`操作符（类似于`instance of `）；
- 条件分支为范围类型时，使用`in`或者`!in`操作符；
- 条件分支代码块内只有一句时，大括号可以省略；
- 当使用`when`表达式作为函数的返回值时，`else`分支**不可以省略**，其它情况可以不写但*建议不省略*；
- 条件分支的执行顺序与`switch`同样是自上而下，即最上方的分支最优先判断。
