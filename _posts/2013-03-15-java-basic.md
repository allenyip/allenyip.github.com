---

title: Java 基础

layout: post

---

##**基本数据类型**

1，Java里的数据类型分为基本类型和引用类型。基本类型可直接创建赋值并存储于**栈**中，引用类型通过new关键字创建对象并存储于**堆**中。

2，由于JVM是独立于底层的机器，基本数据类型大小是固定的，因此Java也不需要sizeof操作。基本数据类型及其大小如下所示：

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

3，除了继承自C++的单行注释//和多行注释/* * /，Java提供了文档注释/** * /，方便开发人员生成代码文档，注释文档分为类、域和方法三种，只能对public和protected成员进行注释。有两种使用方式：嵌入式HTML和文档标签（@开头）。一个示例如下：

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

1，赋值时，基本数据类型会直接将值复制，而类（对象）则只赋值引用，示例如下：

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

2，关系操作符 == 和 != 经常会令人迷惑：

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

3，移位操作符中，左移<<在低位补0；有符号右移>>使用符号扩展：符号位正则高位补0，为负则高位补1；Java增加了无符号右移>>>操作，使用零扩展：无论正负高位都补0。

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

4，Java中对于boolean类型支持的操作只有：赋予true/false值、测试其真假。除此之外，对其加减等其他任何运算都会报错。除了boolean类型外的其他基本类型都可通过类型转换转变为其他基本类型，转换过程中必须留意“窄化转换”结果，否则可能会出现编译器无法预知的严重错误。

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

1，使用组合或者继承来复用类，组合是将对象引用置于新类中，继承则是面向对象的四大特性之一，优先使用组合，如果需要向上转型（派生类可以替换基类）则使用继承。

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

2，static关键字可以将对象固定在一个存储空间，可以满足两种需要：只想为某特定域分配单一存储空间；希望某个方法不与包含它的类的任何对象关联。static修饰的变量或方法与类打交道，类无需实例化；非static变量或方法则与对象打交道。

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

3，使用final关键字分为三种情况：数据、方法和类。

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

4，类的构造器调用遵循以下顺序

* 0. 调用static初始化语句。

* 0.5. 将要分配给对象的存储空间初始化成二进制零。

* 1. 调用基类构造器。

* 2. 按声明顺序调用成员的初始化方法。

* 3. 调用导出类构造器的主体。

##**接口**

1, Java接口中的成员变量默认都是public,static,final类型的(都可省略)，必须被显示初始化，即接口中的成员变量为常量(大写，单词之间用"_"分隔)。Java接口中的方法默认都是public,abstract类型的(都可省略)，没有方法体，不能被实例化。

```java
public interface A {
    int CONST = 1; //合法,CONST默认为public,static,final类型
    void method(); //合法,method()默认为public,abstract类型
    public abstract void method2(); //method2()显示声明为public,abstract类型
}
```

3, Java接口中只能包含public,static,final类型的成员变量和public,abstract类型的成员方法。接口中不能有构造方法，不能被实例化。

```java
public interface A {
   int var; //错,var是常量,必须显示初始化
   public A(){...}; //错,接口中不能包含构造方法
   void method(){...};   //错,接口中只能包含抽象方法
   protected void method2(); //错,接口中的方法必须是public类型
   static void method3(){...};   //错,接口中不能包含静态方法
}
```

5, 一个接口不能实现(implements)另一个接口,但它可以继承多个其它的接口（多继承！！！）。

6, 当类实现了某个Java接口时,它必须实现接口中的所有抽象方法,否则这个类必须声明为抽象的。

8, 不允许创建接口的实例(实例化),但允许定义接口类型的引用变量,该引用变量引用实现了这个接口的类的实例（多态）。

比较抽象类和接口：

相同点

* 1. 都代表系统的抽象层,当一个系统使用一颗继承树上的类时,应该尽量把引用变量声明为继承树的上层抽象类型,这样可以提高两个系统之间的松耦合

* 2. 都不能被实例化

* 3. 都包含抽象方法,这些抽象方法用于描述系统能提供哪些服务,但不提供具体的实现

不同点

* 1. 抽象类中可以对部分方法进行实现，而接口中全部是抽象方法。

* 2. 一个类只能继承一个父类（抽象类），却可以实现多个接口。

* 3. 接口可以继承多个接口，抽象类只能继承一个抽象类。

* 4. 接口中的变量默认都是public static final常量，方法都是public abstract。

##**内部类**

内部类是一个编译时的概念，一旦编译成功，就会成为完全不同的两类。对于一个名为outer的外部类和其内部定义的名为inner的内部类。编译完成后出现outer.class和outer$inner.class两类。所以内部类的成员变量/方法名可以和外部类的相同。

1，内部类可以分为在类中的内部类（成员内部类和静态内部类）以及在方法中的内部类（局部内部类和**匿名内部类**）。

2，成员内部类和静态内部类都可以与外围类相互访问，即使是私有域，静态内部类只能访问其外围类的静态成员，除此之外与成员内部类没有任何区别。

```java
// 代码1：内部类对外部类可见
class Outer{
    //创建私有内部类对象
    public Inner in=new Inner();

    //私有内部类
    private class Inner{
         ...
    }
}

//代码2：外部类对内部类可见
//(内部类可以访问外部类的所有成员变量和方法)
class Outer{
    //外部类私有数据域
    private int data=0;

    //内部类
    class Inner{
        void print(){
            //内部类访问外部私有数据域
            System.out.println(data);
        }
    }
}

//代码3：静态内部类对外部变量的引用
public class Outer{
    private static int i=0;

    //创建静态内部类对象
    public Inner in=new Inner();

    //静态内部类
    private static class Inner{
        public void print(){
            System.out.println(i);   //如果i不是静态变量，这里将无法通过编译
        }
    }
}
```

3，局部内部类没有访问修饰符，对包围它的方法之外的任何东西都不可见；局部内部类只能够访问该方法中的局部变量，且这些局部变量一定要是final修饰的常量。匿名内部类就是没有名字的局部内部类，它没有构造器，**它隐式的继承了一个父类或者实现了一个接口**。

```java
class Outter{
    public void outMethod(){
        final int beep=0;
        class Inner{
            //使用beep
        }

        Inner in=new Inner();
    }
}
```

##**容器类**

1，数组（非容器）：将数字与对象联系起来，保存类型明确的对象，可以是多维的，但是数组一旦生成其大小不能改变。

2，容器可以分为Collection和Map：Collection保存单一的元素，主要有List、Set等派生接口；Map则以名值对的方式存储。常用的容器有ArrayList/LinkedList、HashSet和HashMap。

3，List的特点是有顺序而可以重复，Set的特点是无顺序而不可重复。重复不是指容器中装了两个相同的对象，而是他们的值相等，因此自定义对象作为容器的储存元素时，我们必须重写java.lang.Object的equals方法（同时也应重写hashCode方法），Object的默认equals只有当比较的参数是本身时才返回true。

4，ArrayList实现了大小可变的数组，get/set方法均为线性时间，add/remove则比较费时；LinkedList可以快速的add/remove，但get/set方法比较慢。也因此LinkedList常被用作栈和队列使用。

5，Set有两个派生接口HashSet和TreeSet。HashSet是为快速查找设计的Set，存入HashSet的对象必须定义hashCode()；Tree是保存次序的Set，底层为树结构，可以提取有序序列。LinkedHashSet派生自HashSet，具有HashSet的查询速度，同时维护了元素的插入顺序。

6，Map有两个派生接口HashMap和TreeMap。 HashMap使用对象的hashCode()进行快速查询，TreeMap基于红黑树，维护了名值对的次序。LinkedHashMap派生自HashMap，维护了元素的顺序。

7，一般情况下数据结构的选择：多查少改选ArrayList；多改少查选LinkedList；如果大量数据进行检索选Map。

8，一些开销较大的同步接口：Vector(--ArrayList)、HashTable(--HashMap)、Stack(--LinkedList)。

9，java.util.Collection是一个集合接口，定义了一些集合基本操作的通用方法（add/remove）；java.util.Collections是一个包装类，定义了一些集合操作的静态多态方法（sort/shuffle/synchronizedXXX），类似一个工具类。另：Arrays工具类也提供了一些对数组的静态操作方法。

##**异常**

1，Throwable类是所有异常类的基类，他有两个派生类Error和Exception：Error是程序无法处理的错误，如StackOverFlow/OutOfMemoryError等，此类错误通常交给JVM处理。Exception则是程序本身可以处理的异常。

2，Exception又可以分为CheckedException和UncheckedException，其中UncheckedException（或叫RuntimeException）和Error一样是程序无法处理的，如ClassNotFoundException/IllegalArgumentException/NullPointerException/IndexOutOfBoundsException等。

3，使用try...catch...finally使用异常，可以更好的处理程序：

```java

public static void testException1() {
    int[] ints = new int[] { 1, 2, 3, 4 };
    System.out.println("异常出现前");
    try {
        System.out.println(ints[4]);
        System.out.println("我还有幸执行到吗");// 发生异常以后，后面的代码不能被执行
    } catch (IndexOutOfBoundsException e) {
        System.out.println("数组越界错误");
    }
    System.out.println("异常出现后");
} /*
异常出现前
数组越界错误
异常出现后
*/

public static void testException2() {
    int[] ints = new int[] { 1, 2, 3, 4 };
    System.out.println("异常出现前");
    System.out.println(ints[4]);
    System.out.println("我还有幸执行到吗");// 发生异常以后，他后面的代码不能被执行
} /*
异常出现前
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 4
*/
```

4，在catch语句块中可以使用throw语句重新抛出异常（方法中用throws声明抛出的异常），将异常抛出给上一个调用者；三个以上的异常链会丢失异常，使用x.initCause(xx);方法可以保存异常信息，使得在抛出另外一个异常的同时不丢失原来的异常。

5，try...catch...finally细节

* try 块：用于捕获异常。其后可接零个或多个catch块，如果没有catch块，则必须跟一个finally块。

* catch 块：用于处理try捕获到的异常。

* finally 块：无论是否捕获或处理异常，finally块里的语句都会被执行。**当在try块或catch块中遇到return语句时，finally语句块将在方法返回之前被执行**。在以下4种特殊情况下，finally块不会被执行：在finally语句块中发生了异常；在前面的代码中用了System.exit()退出程序；程序所在的线程死亡；关闭CPU。

##**字符串**

1，Java字符串类(java.lang.String)是Java中使用最多的类，也是最为特殊的一个类。

* 1、String类是final的，不可被继承。public final class String。

* 2、String类是的本质是字符数组char[], 并且其值不可改变。private final char value[];

* 3、String类对象有个特殊的创建的方式，就是直接指定比如String x = "abc"，"abc"就表示一个字符串对象。而x是"abc"对象的地址，也叫做"abc"对象的引用。

* 4、String对象可以通过“+”串联。串联后会生成新的字符串。也可以通过concat()来串联。

* 5、创建字符串的方式很多，归纳起来有三类：1）使用new关键字创建字符串，比如String s1 = new String("abc");2）直接指定。比如String s2 = "abc";3）使用串联生成新的字符串。比如String s3 = "ab" + "c";

* 6、Java运行时会维护一个String Pool（String池），用来存放运行时中产生的各种字符串	，并且池中的字符串的内容不重复。

* 7、intern方法用于常量池中是否有相同Unicode的字符串常量，如果有，则返回其的引用， 如果没有，则在常量池中增加一个Unicode等于str的字符串并返回它的引用

```java
public class Test {
	public static void main(String[] args){
		String s0 = "allen";
		String s1 = "allen"; // String也是对象，但与其他对象不同，字符串存在一个字符串池中。
		String s2 = "al" + "len";
		System.out.println( s0 == s1 );
		System.out.println( s0 == s2 );

		String s3 = new String("allen"); // 运行时才创建的对象
		System.out.println( s0 == s3 );

		String s4 = new String("allen");
		s4 = s4.intern();
		System.out.println( s0 == s4 );
	}
} /* Output:
true
true
false
true
*/
```

2，String对象的创建根据以下原理：

* 原理1：当使用任何方式来创建一个字符串对象s时，Java运行时（运行中JVM）会拿着这个X在String池中找是否存在内容相同的字符串对象，如果不存在，则在池中创建一个字符串s，否则，不在池中添加。

* 原理2：Java中，只要使用new关键字来创建对象，则一定会（在堆区或栈区）创建一个新的对象。

* 原理3：使用直接指定或者使用纯字符串串联来创建String对象，则仅仅会检查维护String池中的字符串，池中没有就在池中创建一个，有则罢了！但绝不会在堆栈区再去创建该String对象。

* 原理4：使用包含变量的表达式来创建String对象，则不仅会检查维护String池，而且还会在堆栈区创建一个String对象。

3，String类的主要用法：

* 获取长度 str.length()。（数组是arr.length）

* 比较字符串：s1.equals(s2)判断内容是否相同、s1.compareTo(s2)判断大小关系、==判断内容与地址是否相同、s1.reagionMatches(...)判断部分相同

* 查找字符 s.charAt(index)

* 查找字符串位置 s1.indexOf(s2)返回s1中第一次s2出现位置、s1.indexOf(s2, fromIndex)从字符串的第fromIndex个字符开始检索。（类似有lastIndexOf）

* 字符串包含 s1.conatins(s2)

* 截取子串 s1.subString(beginIndex)/s1.subString(beginIndex, endIndex)

* 字符串替换 replace(oldChar, newChar)、replaceAll(regex, replacement)

* 去除首尾空格 s.trim()

* 转换成字符数组 s.toCharArray();

* 分割成字符串数组 s.split(regex);