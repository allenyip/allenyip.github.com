---

title: 拥抱 Python

layout: post

---
由于第三方势力的良性压迫，最近终于有时间正经学学 [Python](http://www.python.org/) 了，本文用于记录我所了解的Python。

##**学习资料**

* **[A Byte of Python](http://swaroopch.com/book/python/)** by Swaroop C H ([中文](http://sebug.net/paper/python/))

* **[Learn Python The Hard Way](http://learnpythonthehardway.org/)** by Zed. A. Shaw ([中文](http://sebug.net/paper/books/LearnPythonTheHardWay/))

* **Learning Python** by Mark Lutz

* **Dive into Python** by Mark Pilgrim

* **Python Cookbook** by  Alex Martelli

* **官方文档，Pydoc等**

##**引言**

著名码农 [Eric Raymond](http://www.linuxjournal.com/article/3882) 早在千禧年就建议过大家使用Python进行编程。与Java的成熟和复杂不同，Python简单易用，并且仍在快速的发展中（3.x的变革），用Python实现了几个功能之后不禁感叹其简单的代码结构和强大的开发效率，完全颠覆了之前对其「脚本语言」的定义，除去脚本语言，Python至少还支持了动态语言，面向对象，函数式编程等特性。同时，它“只用一种方法完成一件事”的哲学尽管损失了创造性，却保证了精益求精的高效性和实用性。

##**获取Python**

Python完全免费且开源，大部分Linux/Mac系统都默认安装了Python，在 [官方网站](http://www.python.org/) 上也可以轻松地获取最新版本的Python，支持Windows, Linux/Unix, Mac等操作系统。除此之外，官网还提供了详尽的文档共开发者阅读。

##**运行Python**

和Java类似，Python程序执行时，会先将源码(.py)编译成字节码(.pyc)，之后在PVM（Python虚拟机）上运行。它没有像C/C++那样编译成特定机器的二进制代码，尽管牺牲了一定的运行速度，却保证了跨平台的特性。

另外，Python语言包括三种实现方式：CPython（标准Python）、Jython（与Java集成的Python）和IronPython（与.NET/Mono集成的Python）。

##**动态类型**

与Java静态类型不同，Python的类型是动态的。即类型属于对象，而不是变量。Python同样支持自动垃圾回收，因此开发者不需要考虑内存释放问题。

##**类型和运算**

**Python程序可以分解为模块、语句、表达式和对象**，其中内置对象类型有以下几种：数字、字符串、列表、字典、元组、文件、集合和其他类型。

###**数字**

数字类型包括：整数和浮点数、复数和分数等，提供的运算符包括+、-、*、/、%、>>、**、&等，内置数学函数包括pow、abs、round等，还可加载random、math等模块进行数字处理。

对数字的操作与Java基本一致，不同类型的数字计算会自动升级为更复杂的数字，也可以通过int(), float()等方法进行类型转换。 比较特别的是Python的除法分为Floor除法和真除法，举个栗子：

> 5 /  2 = 2, 5 /  2.0 = 2.5, 5 /  -2.0 = -2.5
> 5 // 2 = 2, 5 // 2.0 = 2.0, 5 // -2.0 = -3.0

以及它加入的分数类型：

```python
from fractions import Fraction
x = Fraction(1, 3)
y = Fraction(4, 6)
print (x + y)
```

###**字符串**

字符串包括：单引号/双引号字符串('python', "python")、三引号字符串('''python''')、转义字符('\t')、Raw字符串(r'python')等。单引号和双引号字符串在Python中是一样的；三引号常用来编写多行字符串块和注释；特殊字节可以使用转义序列表示，同时可以使用Raw字符串抑制转义。

常用的字符串操作包括求长(len(S))、相加(S1+S2)、重复(S*2)、索引和分片(S[0], S[-1], S[1:3], S[0:5:2])...同时支持格式化输

需要注意到是，字符串是不可变类型，要修改一个字符串（字符序列），需要使用合并、分片等工具来建立并赋予新的字符串。（Python中的不可变类型包括数字、字符串、元组和不可变集合；可变类型包括列表、字典和可变集合）

###**列表**

列表是任意对象的有序集合，和字符串一样可通过偏移读取，不同的是列表是长度和值都是可变的。

常用的列表操作包括：求长(len([1,'a',3]))、合并(L1+L2)、重复(L*2)、迭代(for x in L:)、增长(L.append)、插入(L.insert())、排序(L.sort())、删除(L.pop())...

###**字典**

字典是任意对象的无序集合，可通过键来访问，长度和值也可改变。

常用的字典操作包括：求长(len({'k1':'v1','k2':2}))、获取(D.get(key, default))、合并(D.update(D2))、新增/修改(D[key]=1)、删除(D.pop(key))...

###**元组**

元组是任意对象的有序集合，和列表一样可通过偏移读取，不同的是元组是不可变的。除了改变值的方法外，其基本操作与列表类似。

###**文件**

Python内置了简单的文件操作方法，如：

```python
in = open(r'/usr/...','r') # 读（默认）
str = in.read() # 读取整个文件到单一字符串
str = in.read(N) # 读取之后的N个字节
str = in.readline() # 读取下一行
l = in.readlines() # 读取整个文件到列表
out = open(r'/usr/...','w/a') # 写/追加
out.write(str) # 写入整个字符串
out.writelines(l) # 写入列表
out.seek(N) # 修改文件位置到偏移量N
out.flush() # 输出缓冲区刷到硬盘，但不关闭文件
out.close() # 关闭文件
```

###**注意**

赋值操作总是存储对象的引用，而非拷贝，比如：

```python
X = [1,2,3]
L = ['a',X,'b'] # L is ['a',[1,2,3],'b']
X[1] = 'z' # L changes to ['a',[1,'z',3],'b']
```

因此，在不需要保持原值时，应当赋予其拷贝或者新值，而非直接赋值。

```python
X = [1,2,3]
L = ['a',X[:],'b'] # L is ['a',[1,2,3],'b']
X[1] = 'z' # L is still ['a',[1,2,3],'b']
```

另外，相等比较时，==操作符测试值的相等性，is表达式则测试对象的一致性，比如：

```python
L1 = [1,2,3]
L2 = [1,2,3]
L1 == L2, L1 is L2 # True, False
```

##**语句和语法**

**Python程序可以分解为模块、语句、表达式和对象**，语句包括：赋值、调用、if/elif/else、for/else、while/else、pass（空占位符）、break、continue、def（函数和方法）、return、yeild（生成器函数）、global、import、from、class、try/except/finally、raise（触发异常）、assert（调试检查）、with/as、del

与Java不同，Python添加了冒号来区分复合语句，Python代码块没有括号({})而采用缩进，语句结尾没有分号，条件语句中的括号是可选的（一般不加）

TBD：其余语句语法大同小异，暂不赘述。

##**函数**

* def是可执行的代码，就是说Python执行了def语句后函数才存在。

* def创建了一个对象并将其赋值给某一变量名。

* lambda创建一个对象但将其结果返回。

* return将一个结果对象发送给调用者。

* yeild向调用者发回一个结果对象，但记住它离开的地方。

* global声明了一个模块级的比啊两并被赋值。

* nonlocal声明了将要赋值的一个封闭的函数变量。

* 函数是通过赋值（对象引用）传递的。

* 参数、返回值以及变量不是声明。

###**多态**

多态指的是一个操作的意义取决于被操作对象的类型，比如：

```python
def times(x, y):
	return x * y
times(2,4) # 8
times('a',4) # aaaa
```

*操作对不同的类型产生了与之匹配的不同运算，表现出多台。

```python
def intersect(seq1, seq2):
    res = []
    for x in seq1:
        if x in seq2:
            res.append(x)
    return res
intersect("SPAM","SPCM") # ['S','P','M']
intersect([1,2,3],(1,4)) # [1]
```

###**作用域**

变量名解析的LEGB法则：

* 在函数中使用未认证的变量名时，Python搜索4个作用域：本币作用域(L)，之后是上一层结构中def或lambda的本地作用域(E)，之后是全局作用域(G)，最后是内置作用域(B)。

* 在函数中给一个变量名赋值时，除非它已在函数中使用global声明为全局变量，否则Python总是创建或改变本地作用域的变量名.

* 在函数外给一个变量名赋值时，本地作用域与全局作用域相同。

```python
x = 22 # global in default
y = 22
def func():
    glocal x
    x = 99
    y = 99
func()
print(x,y) # 99,22
```

TBD...
