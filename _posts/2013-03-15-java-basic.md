---

title: Java 基础

layout: post

---

##**基本数据类型**

Java里的数据类型分为基本类型和引用类型。基本类型可直接创建赋值并存储于**栈**中，引用类型通过new关键字创建对象并存储于**堆**中。

由于JVM是独立于底层的机器，基本数据类型大小是固定的，因此Java也不需要sizeof操作。基本数据类型及其大小如下所示：

* boolean，-，大小未明确定义，仅定义为字面值true/false，默认值false。

* char，16位，默认值\u0000(null)。

* byte，8位，默认值(byte)0。

* short，16位，默认值(short)0。所有数值类型都有正负号！

* int，32位，默认值0。

* long，64位，默认值0L。

* float，32位，默认值0.0f。

* double，64位，默认值0.0d。

* void，-

**基本类型在作为类的成员时会被赋予默认值，即使未进行初始化也能运行；而作为局部变量，如位于方法中而不是类中，其默认值可能是任意的，并且在运行时会报错。（类变量同理，默认赋予null值）**

除了继承自C++的单行注释//和多行注释/* */，Java提供了文档注释/** */，方便开发人员生成代码文档，注释文档分为类、域和方法三种，只能对public和protected成员进行注释。有两种使用方式：嵌入式HTML和文档标签（@开头）。一个示例如下：

```java
import java.util.*;

/** The first Java example
 *  Display today's date
 *  @author Allen Yip
 *  @version 1.0
 */
public class HelloDate {
	/** Entrv point to application
	 *  @param args array of arguments
	 */
	public static void main(String[] args) {
		System.out.println(new Date());
	}
}
```
##**操作符**

赋值时，基本数据类型会直接将值复制，而类（对象）则只赋值引用，示例如下：

```java
class Tank {
	int level;
}

public class Test {
	public static void main(String[] args) {
		Tank t1 = new Tank();
		Tank t2 = new Tank();
		t1.level = 1;
		t2.level = 2;
		System.out.println("1. t1.level=" + t1.level + " t2.level=" + t2.level);
		t1 = t2; // 使用 t1.level = t2.level 来避免别名现象
		System.out.println("2. t1.level=" + t1.level + " t2.level=" + t2.level);
		t1.level = 99;
		System.out.println("3. t1.level=" + t1.level + " t2.level=" + t2.level);
	}
} /* Output:
1: t1.level=1 t2.level=2
2: t1.level=2 t2.level=2
3: t1.level=99 t2.level=99
*/
```

关系操作符 == 和 != 经常会令人迷惑：

对于基本类型，==是比较它们的值是否相等；对于对象，==则是比较它们在内存的存放地址是否相等，若要比较值需要重写equals方法（以及hashCode方法）。

```java

class Value {
	int i;
}
public class Test {
	public static void main(String[] args) {
		int a1 = 47, a2 = 47;
		Integer b1 = new Integer(47);
		Integer b2 = new Integer(47);
		System.out.println(a1 == a2);
		System.out.println(b1 == b2);
		System.out.println(b1.equals(b2));

		// 自定义类
		Value v1 = new Value();
		v1.i = 47;
		Value v2 = new Value();
		v2.i = 47;
		System.out.println(v1.equals(v2));// 未覆盖equals方法
	}
} /* Output:
true
false
true
false
*/
```

移位操作符中，左移<<在低位补0；有符号右移>>使用符号扩展：符号位正则高位补0，为负则高位补1；Java增加了无符号右移>>>操作，使用零扩展：无论正负高位都补0。

```java
public class Test {
	public static void main(String[] args) {
		int i = -1;
		System.out.println(Integer.toBinaryString(i));
		i >>>= 10;
		System.out.println(Integer.toBinaryString(i));
	}
} /* Output:
11111111111111111111111111111111
1111111111111111111111
*/
```

Java中对于boolean类型支持的操作只有：赋予true/false值、测试其真假。除此之外，对其加减等其他任何运算都会报错。除了boolean类型外的其他基本类型都可通过类型转换转变为其他基本类型，转换过程中必须留意“窄化转换”结果，否则可能会出现编译器无法预知的严重错误。

##**控制执行流程**

以下例子展示了break和continue的区别：

```java
public class Test {
	public static void main(String[] args) {
		for (int i = 0; i < 100; i++) {
			if (i == 74) break; // out of loop
			if (i % 9 != 0) continue; // next iteration
			System.out.println(i + "");
		}
	}
} /* Output:
0 9 18 27 36 45 54 63 72
*/
```

##**初始化与清理**

Java尽力保证所有变量在使用前都能得到初始化，默认构造器（无参构造器）在未显示定义构造器时会自动创建，若已定义则不会自动创建。方法重载根据参数类型列表，而非返回值（会产生歧义）。**变量定义的先后顺序决定了它的初始化顺序，即使变量定义散布于方法定义之间，它们仍旧会在任何方法之前（包括构造器方法）被初始化**。例子如下：

```java
class Window {
	Window(int marker) {
		System.out.println("Window(" + marker + ")");
	}
}
class House {
	Window w1 = new Window(1); // before constructor
	House() {
		System.out.println("House()");
		w3 = new Window(33);
	}
	Window w2 = new Window(2); // after constructor
	void f() { System.out.println("f()");}
	Window w3 = new Window(3);
}
public class Test {
	public static void main(String[] args) {
		House h = new House();
		h.f();
	}
} /* Output:
Window(1)
Window(2)
Window(3)
House()
Window(33)
f()
*/
```

假设有一个Dog类，其对象的创建过程如下：

* 1. 首次创建或者Dog类的静态方法/静态域（构造器实际上也是静态方法）首次被访问时，Java解释器会查找类路径以定位Dog.class文件。

* 2. 载入Dog.class，有关静态初始化的所有动作都会执行，并且**只执行一次**。

* 3. 当用new Dog()创建对象时，首先在堆上为Dog对象分配足够的存储空间。

* 4. 这块存储空间被清零，即自动将Dog对象中的基本类型数据设置成默认值，引用类型设置成null。

* 5. 执行所有出现于字段定义处的初始化动作。

* 6. 执行构造器。

##**访问权限控制**

Java中的访问权限控制等级从大到小依次为：public、protected、包访问权限（没有关键词/可表示为friendly、默认）和private。

当编译一个.java源文件时（此文件被称为编译单元），文件可以有一个public类并且该类名与文件名相同（最多只能有一个public），若该编译单元内还有其他类，这些类在包之外的世界是不可见的。

Java解释器的运行过程如下：

* 1. 找出环境变量CLASSPATH，CLASSPATH包括一个或多个目录，用于查找.class文件的根目录。

* 2. 从根目录开始，解释器获取包的名称并将每个句点替换成反斜杠，产生一个路径。

* 3. 将路径与CLASSPATH链接即为所要查找的.class文件路径。

类的访问权限总是public或包访问权限（内部类除外），如果不希望该类被访问可以把所有构造器都指定为private，此时，除了通过类内部的static成员，其他类无法直接访问该类。

##**复用类**

使用组合或者继承来复用类，组合是将对象引用置于新类中，继承则是面向对象的四大特性之一，优先使用组合，如果需要向上转型（派生类可以替换基类）则使用继承。

实际项目中常常会结合使用组合和继承：

```java
class Plate {
	Plate(int i) {
		System.out.println("Plate constructor");
	}
}

class DinnerPlate extends Plate {
	DinnerPlate(int i) {
		super(i);
		System.out.println("DinnerPlate constructor");
	}
}

class Utensil {
	Utensil(int i) {
		System.out.println("Utensil constructor");
	}
}

class Spoon extends Utensil {
	Spoon(int i) {
		super(i);
		System.out.println("Spoon constructor");
	}
}

class Fork extends Utensil {
	Fork(int i) {
		super(i);
		System.out.println("Fork constructor");
	}
}

class Custom {
	Custom(int i) {
		System.out.println("Custom constructor");
	}
}

public class PlaceSetting externds Custom {
	private Spoon sp;
	private Fork fk;
	private DinnerPlate pl;
	public PlaceSetting(int i) {
		super(i + 1);
		sp = new Spoon(i + 2);
		fk = new Fork(i + 3);
		pl = new DinnerPlate(i + 4);
		System.out.println("PlaceSetting constructor");
	}
	public static void main(String[] args) {
		PlaceSetting x = new PlaceSetting(9);
	}
} /* Output:
Custom constructor
Utensil constructor
Spoon constructor
Utensil constructor
Fork constructor
Plate constructor
DinnerPlate constructor
PlaceSetting constructor
*/
```

static关键字可以将对象固定在一个存储空间，可以满足两种需要：只想为某特定域分配单一存储空间；希望某个方法不与包含它的类的任何对象关联。

```java
class Value1 {
	int x = 1;
	Integer i = new Integer(47);
}

class Value2 {
	static Integer i = new Integer(47);
}

public class House {
	public static void main(String[] args) {
		Value1 a1 = new Value1();
		Value1 a2 = new Value1();
		System.out.println(a1.x == a2.x);
		System.out.println(a1.i == a2.i);
		Value2 b1 = new Value2();
		Value2 b2 = new Value2();
		System.out.println(b1.i == b2.i);
	}
} /* Output:
true
false
true
*/
```

使用final关键字分为三种情况：数据、方法和类。

final成员变量表示常量，只能在定义处（只定义未赋值成为空白final）或者构造函数中赋值赋一次值，并且赋值后值不再改变。当final修饰参数时表示参数无法被更改。

使用final方法的原因是为了锁定方法防止被任何继承类修改，类中所有的private方法默认都是final的。

当某个类定义为final时，它无法被继承，亦即final类中所有的方法都是隐式final的。

一个既是static又是final的域（编译器常量）是恒定不变的，且存储于一段不能改变的存储空间。

```java
class Value {
	int i;
	public Value(int i) {
		this.i = i;
	}
}

public class House {
	private static Random rand = new Random(47);
	private String id;

	public House(String id) {
		this.id = id;
	}

	// 编译常量
	private final int valueOne = 9;
	private static final int VALUE_TWO = 99;
	public static final int VALUE_THREE = 39;
	// 非编译常量
	private final int i4 = rand.nextInt(20);
	static final int INT_5 = rand.nextInt(20);

	private Value v1 = new Value(11);
	private final Value v2 = new Value(22);
	private static final Value v3 = new Value(33);

	private final int[] a = { 1, 2, 3, 4, 5, 6, };

	public String toString() {
		return id + ":" + "i4=" + i4 + ". INT_5=" + INT_5;
	}

	public static void main(String[] args) {
		House fd1 = new House("fd1");
		// ! fd1.valueOne++ // Error: 不能改变值
		fd1.v2.i++; // 对象不是常量
		fd1.v1 = new Value(9);
		// ! fd1.v2 = new Value(9); // Error: 对象为final
		for (int i = 0; i < fd1.a.length; i++) {
			fd1.a[i]++;// 数组对象也不是常量
		}
		// ! fd1.a = new int[3]; // Error: 数组对象为final
		System.out.println(fd1);
		System.out.println("Create new data");
		House fd2 = new House("fd2");
		System.out.println(fd1);
		System.out.println(fd2);
	}
} /* Output:
fd1:i4=15. INT_5=18
Create new data
fd1:i4=15. INT_5=18
fd2:i4=13. INT_5=18
*/
```
