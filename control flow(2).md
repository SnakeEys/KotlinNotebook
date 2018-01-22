循环/遍历/迭代

### 循环语句
作为逻辑控制的一部分，循环`loops`在程序中是必不可少的一环，在Java中的几种常见遍历方式：`while`,`do...while`,`for`,`for...each`以及出境率似乎并不高的`iterator`；在Kotlin中除了`for`相对Java的灵活性大大提高，其它几操作符则基本保持不变；因此，本章节内容将重点介绍如何在Kotlin中进行`for`循环操作：

	// Java
	...
	for (int i = 0; i < arr.length; i++) {
		// Iterate arr's element by arr[i]
		...
	}

	for (int a : arr) {
		// Iterate element directly in arr.
		...
	}

上例中是我们最经常接触和使用的情况了，而在Kotlin之中，大部分的情况下会我们会采取类似Java中`for...each`的语法，但相对Java的`for...each`而言又简洁了些许：

	// Kotlin
	for (item in arr) {
		// Iterate element through for each,
		// the `item` is the element of arr
		...
	}
大多数情况下我们会使用上述方式来对可迭代对象进行操作，清晰而且简洁，而到后期我们会使用更高级的方法来替代。

#### 眼花缭乱的`for`循环
如果习惯于Java中通过数组或者集合的下标来遍历，Kotlin就像喝高了的的老司机：
	
	// Kotlin
	for (index in arr.indices) {
        println(arr[i]])
    }

会把它和之前的元素迭代弄混？你可以这样写：
	
	for (index in 0 until arr.length) {
		println("index: $index")
	}

或者就想从x数到y时：

	for (i in x..y) {
		...
	}

既要索引，又要元素？满足你：

	for ((index, value) in arr.withIndex()) {
		println("index: $index, value: $value")
	}
然而操作再多，实际使用的情况仍然是以第一种类似`for...each`的方式最为常见（*实际上使用lambda表达式更常见*），当然偶尔也会需要用到索引的情况；Kotlin的`for`循环可以对任何提供`iterator()`函数的类进行遍历，此外，仅需满足如下条件即可：

- 包含函数名为`iterator()`的成员(或扩展)函数*：
	- 包含函数名为`next()`的成员(或扩展)函数；
	- 包含函数名为`hasNext()`的成员(或扩展)函数且返回值为`Boolean`类型；
上述函数必须使用`operator`操作符修饰。


*扩展函数：当类的完成度较高，但又不方便于新添加函数时，可以为该类在独立的文件中添加和补充新的函数，且不会对已完成的类产生影响，使用范围是全局可用；后续有章节针对普通函数之外的几种函数进行详细使用讲解。*