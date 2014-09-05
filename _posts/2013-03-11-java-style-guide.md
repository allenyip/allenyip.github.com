---

title: Java 风格指南

layout: post

---
以下为个人精简的 Java 的风格指南（[戳我](https://code.google.com/p/google-styleguide/)）。

* [源文件基础](#basi)
* [源文件结构](#stru)
* [格式](#form)
* [编程实践](#prac)
* [JavaDoc](#doc)

<h2 id="basi">源文件基础</h2>

1，源文件名为其最顶层类名，编码采用 UTF-8。

2，除行结束符序列和空格外的空白字符都需要进行转义。

3，优先使用转义序列，而非八进制或 Unicode。

<h2 id="stru">源文件结构</h2>

1，一个源文件应包含：许可证或版权信息、package 语句、import 语句和一个顶级类，这四部分用空行分隔。

	1-1，许可证或版权信息应放在最前。

	1-2，package 语句不换行。

	1-3，import 语句不使用通配符（.*）、不换行、按照此顺序以空行分隔的分组进行导入：

		* 所有的静态导入独立成组

		* com.xxxxx imports

		* 第三方的包。每个顶级包为一组，字典序。例如：android, com, junit, org, sun

		* java imports

		* javax imports

2，只有一个与源文件同名的顶级类声明。

3，类成员应按照一定顺序排列。

4，类中的多个构造函数或同名方法应顺序出现在一起。

<h2 id="form">格式</h2>

1，大括号。

	1-1，if, else, for, do, while 语句应使用大括号。

	1-2，非空块和块状结构中大括号遵循 Kernighan 和 Ritchie 风格（[Egyptian Brackets](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html)）。

		* 左大括号前不换行

		* 左大括号后换行

		* 右大括号前换行

		* 如果右大括号是一个语句、函数体或类的终止，则右大括号后换行，否则不换行。

	1-3，空块可以不换行简洁地使用 {}，除非它处于多块语句（if/else, try/catch/finally）中。

2，块使用 2 个空格进行缩进。

3，每行一条语句。

4，列长度限制使用 80 或 100个字符，超过应换行，除以下例外：

	* 不可能满足列限制的行，如 Javadoc 中的一个长 URL，或是一个长的 JSNI 方法参考)。

	* package 和 import 语句。

	* 注释中那些可能被剪切并粘贴到 shell 中的命令行。

5，自动换行

	5-1，在更高的语法级别处断开：

		* 如果在非赋值运算符处断开，那么在该符号前断开，如 + 将位于下一行。

		* 如果在赋值运算符处断开，通常的做法是在该符号后断开，如 = 与前面的内容留在同一行。

		* 方法名或构造函数名与左括号留在同一行。

		* 逗号与其前面的内容留在同一行。

	5-2，自动换行时缩进至少加 4 个空格。

6，空白

	6-1，垂直空白：以下情况需要使用一个空行：

		* 类内连续的成员之间：字段，构造函数，方法，嵌套类，静态初始化块，实例初始化块。

		* 在函数体内，语句的逻辑分组间使用空行。

		* 允许但不鼓励使用连续空行。

	6-2，水平空白：除了语言需求和其它规则，并且除了文字，注释和 Javadoc 用到单个空格，单个 ASCII 空格也出现在以下几个地方：

		* 分隔任何保留字与紧随其后的左括号，如 if, for catch 等。

		* 分隔任何保留字与其前面的右大括号，如 else, catch。

		* 在任何左大括号前，除了两种例外：@SomeAnnotation({a, b}) 和 String[][] x = foo。

		* 在任何二元或三元运算符的两侧。

		* 在 , : ; ) 后。

		* 如果在一条语句后做注释，则双斜杠两边都要空格。

		* 类型和变量之间：List list。

		* 数组初始化中，大括号内的空格是可选的，即 new int[] {5, 6} 和 new int[] { 5, 6 } 都是可以的。

	6-3，并不要求使用水平对齐。

7，尽量使用小括号来限定组。

8，枚举类中的枚举常量间用逗号隔开。

9，变量声明

	9-1，每次只声明一个变量。

	9-2，需要时才声明，并尽快初始化。

10， 数组

	10-1，数组初始化可写成块状结构。

	10-2，使用 String[] args，而非 String args[]。

11，Switch 语句

	11-1，使用 2 个空格进行缩进。

	11-2，在一个 switch 块内，每个语句组要么通过 break, continue, return 或抛出异常来终止，要么通过一条注释来说明程序将继续执行到下一个语句组（如 // fall through），这个特殊的注释并不需要在最后一个语句组（default）中出现。

	11-3，必须包含 default。

12，一个注解独占一行。

13，块注释与其周围的代码在同一缩进级别。

14，类和成员的 modifiers 如果存在，则按Java语言规范中推荐的顺序（public protected private abstract static final transient volatile synchronized native strictfp）出现。

<h2 id="prac">编程实践</h2>

1，能用 @Override 则用。

2，每个捕获的异常都不能被忽略。

3，使用类调用静态成员。

4，禁用 Finalizers。

<h2 id="doc">JavaDoc</h2>

1，基本形式如下

	/** An especially short bit of Javadoc. */

	/**
 	  * Multiple lines of Javadoc text are written here,
 	  * wrapped normally...
	  */

2，标准的 Javadoc 标记按以下顺序出现：@param, @return, @throws, @deprecated, 这四种标记如果出现，描述都不能为空。 当描述无法在一行中容纳，连续行需要至少再缩进四个空格。

3，每个类或成员的 Javadoc 以一个简短的摘要片段开始。这个小片段可以是一个名词短语或动词短语，但不是一个完整的句子。它不会以 A {@code Foo} is a... 或 This method returns... 开头，也不会是一个完整的祈使句，如 Save the record... 然而，由于开头大写及被加了标点，它看起来就像是个完整的句子。

4，至少在每个 public 类及它的每个 public 和 protected 成员处使用 Javadoc。

**如果你有自己的风格并且它很美，那么以上一切都是浮云。**