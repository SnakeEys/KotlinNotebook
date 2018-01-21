在Kotlin中，一切都是对象。没有像Java中那样的原始基本类型。

### BasicTypes
当然，诸如`int`，`float`或者`boolean`等类型仍然存在，但是全部以对象的形式存在，而且其名字以及工作方式都与Java非常相似，但仍有一些不同之处。比如数字类型不会自动转型。举个例子，不能给`Double`变量分配一个`Int`类型的值。因此Java中的自动向上转型，在Kotlin中就需要显式的类型转换：

	val i:Int=7
	val d: Double = i.toDouble()

字符（`Char`）不能直接作为一个数字来处理。在需要时我们需要转换为数字：

	val c:Char='c'
	val i: Int = c.toInt()

位运算也有一点不同。在Android中，我们经常在flags中使用`&`、`|`表示`与`、`或`，在Kotlin中使用`and`和`or`：

	// Java
	int bitwiseOr = FLAG1 | FLAG2;
	int bitwiseAnd = FLAG1 & FLAG2;
  
<line>

	// Kotlin
	val bitwiseOr = FLAG1 or FLAG2
	val bitwiseAnd = FLAG1 and FLAG2

大部分情况下，不用显式的声明变量类型，我们可以让编译器自己去推断出具体的类型；有几点需要值得注意的地方是，当声明`Float`类型的变量时，一定要在值的最后添加``f或者``F，否则编译器会将其推断为`Double`类型：

	val i = 12 // An Int
	val iHex = 0x0f // 一个十六进制的Int类型
	val l = 3L // A Long
	val d = 3.5 // A Double
	val f = 3.5F // A Float

字符串可以直接被当成数组来进行访问和迭代：

	val s = "Example"
	val c = s[2] // 这是一个字符'a'
	// 迭代String
	val s = "Example"
	for(c in s){
	    print(c)
	}


### Variable，Value
Kotlin使用`var`和`val`来声明变量
当使用`var`（variable）声明变量时，表示该变量是一个可变的，即声明该变量后，仍然可以被重新赋值：

	// This works fine.
	fun example() {
	    var a = 1
	    println(a)
	    a = 10
	    println(a)
	    a *= 10
	    println(a)
	} 

而使用`val`（value）来声明变量且一旦在给该变量赋值后，就不能再重新对变量进行赋值操作了，类似于在Java中使用了`final`修饰符：

	// This doesn't work 
	fun example2() {
	    val a: Double = 1.0
	    println(a)
	    a = 2.0	// error
	}

简而言之，`var`和`val`的区别就是，前者声明的是可变(mutable)变量，后者则是不可变(immutable)变量；更多关于`var`和`val`的区别可以在官网进行查看。

### 属性
Kotlin中的属性和Java中的字段是相同的，但是属性更加强大，也更加简洁。属性所做的事情就是加上`getter`和`setter`；在Java中，我们对字段的安全访问方式如下：

	public class Person {
	    private String name;
	    public String getName() {
	        return name;
	    }
	    public void setName(String name) { 
	        this.name = name;
	    }
	}
	...
	Person person = new Person();
	person.setName("name");
	String name = person.getName();

在Kotlin中，只需要一个属性就可以了：

	public class Person {
    	var name: String = ""
	}
	...
	val person = Person()
	person.name = "name"
	val name = person.name
在不做任何指定的情况下，属性会自带`getter`和`setter`。  
但假设我们需要修改setter或者getter的实现，同样也很方便，而且也来得比Java更有意思：

	class Person() {
	    var name = ""
	    get() = field.decapitalize()
	    set(value) {
	       field = "Name: $value"
	    }
	}

只需要在属性下方继续重写get或者set方法即，而且在Kotlin中还有backing filed的概念，关于这一点希望大家自行学习掌握。值得注意的是，getter的访问可见性必须与属性保持一致，setter的访问可见性刚必须小于或等于属性的访问修饰符(public > internal(protected) > private)。

### 补充
在Java访问Kotlin的属性时，编译器会自动链接到它原始的setter/getter。而Kotlin访问Java时，也将会以属性的方式链接其setter/getter。所以在直接访问属性时不会产生性能开销。
