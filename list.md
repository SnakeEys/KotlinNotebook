Set，List，Map都是日常开发中经常要接触的到内容；本章节主要对更为常用的List和Map的使用进行介绍。

### List
**创建List**：

	val list = listOf(1, 2, 3, 4, 5)

**访问List**：

	val element = list[0]

在Kotlin中，对于list元素的访问，以访问数组元素的形式，使用`[]`操作符简化代替`get()`方法。

**操作List**：

在上面的代码中，我们所创建的list，被称之为`Read-Only List`，表示此种类型的List在被创建之后，是无法添加`add()`或删除`remove()`元素的，甚至要改变某个下标对应的元素也是不允许的；若需要创建可变List，则使用内置`mutableListOf()`方法或`arrayListOf()`即可，二者均可得到`ArrayList`：

	val arrayList = arrayListOf('a', 'b', 'c')
	// or
	val arrayList2 = mutableListOf('a', 'b', 'c')
	
	arrayList.add('d')
	arrayList[1] = 'x'	// Update the second element to another value
	arrayList.remove('c')
	arrayList.removeAt(0)


实际上调用`listOf()`函数所传入的参数，就是**变长数组**，只要传入的变长数组长度不为空，直接将数组进行了`asList()`操作：

	public fun <T> listOf(vararg elements: T): List<T> = if (elements.size > 0) elements.asList() else emptyList()


### Map

**创建Map**：

	val map = mapOf("A" to 68, "B" to 69)

**访问Map中的元素**：
	
	val element = map["B"]

map中的`get(key)`方法同样使用了`[key]`操作符替换。

**操作Map**：

与List有着一致的情况，使用`mapOf()`所得到的Map对象也是**Read-Only Map**，也是不可变的，同样的不可变map也无法修改已有元素key所对应的value；使用`mutableMapOf()`或`hashMapOf()`创建可变Map：

	val map = mutableMapOf("A" to 1, "B" to 2, "C" to 3)
	map.put("D", 4)
	map["E"] = 5
	map.remove("C")


#### 补充
在mutable和immutable集合之间，主要的区别就在于能否随意操作（修改，增加以及减少）集合中的元素，在检查和遍历元素时的操作中并无差池。