---

title: JavaScript 风格指南

layout: post

---
抽空学习了一下 JavaScript(JS) 的风格指南（[戳我](https://code.google.com/p/google-styleguide/)），下面是我觉得十分受用的精简版。

##**语言**

1，每个变量声明都加上 var 关键字。

2，常量命名使用 NAMES_LIKE_THIS，闲的话可在注释中加上 @const 注释。

3，每个语句结尾加上分号。

4，使用嵌套函数是极好的。

5，永远不要在块内声明函数，一定要的话请用变量来定义函数。

6，try-catch 尽量不用。

7，尽量使用标准。

8，基本数据类型就别 new Type() 了。

9，尽量不要使用继承。

10，正确使用闭包，则威力无穷。

11，eval() 很不稳定，只用于反序列化。

12，别用 with()。

13，this 只用于构造函数、方法和闭包。

14，for-in 只用于遍历 object/map/hash。

15，不要用 Array 代替 map/hash/associative。

16，多行字符串用 '+' 号连接。

17，用字面量代替 Array 和 Object 构造函数。

##**代码**

1，建议使用驼峰命名法，如 functionNamesLikeThis、variableNamesLikeThis、methodNamesLikeThis、ClassNamesLikeThis 和 SYMBOLIC_CONSTANTS_LIKE_THIS。

	1-1，属性和方法：私有的属性、变量和方法命名以下划线开头。

	1-2，方法和函数参数：可选参数以 opt_ 开头；参数不固定应有 var_args 传参，或者使用 arguments 伪数组；可选和可变参数都用 @param 注释。

	1-3，属性访问：不推荐使用 getters 和 setters。

	1-4，函数访问：若使用 Getters 和 Setters，采用 getFoo()、setFoo() 和 isFoo()。

	1-5，命名空间：JS 不支持包和命名空间，使用 var sloth = {}; sloth.something = sth; 在全局作用域下使用伪命名空间。

	1-6，文件名：全部小写，最好不含 '-'，一定别用 '_'。

2，自定义 toString() 方法一定要能实现且无副作用。

3，明确变量的作用域。

4，代码请进行格式化。

	4-1，中括号：'{' 应与前面的代码在同一行。

	4-2，数组和对象的初始化：尽量单行，多行最好缩进两个空格。

	4-3，函数参数：尽量单行，多行最好跟左括号对齐。

	4-4，传递匿名函数：缩进两个空格。

	4-5，除数组和对象的初始化以及传递匿名函数外，其余都缩进四个空格。

	4-6，空行：用于划分一组逻辑上相关的代码。

	4-7，二元三元操作符：操作符始终在最前一行。

5，不该使用括号时别乱用。

6，单引号字符串好于双引号。

7，使用 JSDoc 进行注释。

8，推荐使用 [编译](https://developers.google.com/closure/compiler/?hl=zh-CN&csw=1) 压缩后的 JS 代码。

9，如果你有自己的风格并且它很美，那么以上一切都是浮云。