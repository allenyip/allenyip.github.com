---

title: JavaScript 陷阱

layout: post

---

* [注释](#comm)
* [变量](#var)
* [数据类型](#data)
* [操作符](#oper)
* [函数](#func)

鉴于 JavaScript(JS) 实在太多小毛病，现将其整理到这个帖子，便于日后翻看纠错。

<h2 id="comm">注释</h2>

JS 使用 // 进行单行注释，使用 /\* \*/ 进行多行注释。

<h2 id="var">变量</h2>

JS 支持一条语句声明多个变量：

{% highlight javascript %}
// 单行
var name="Gates", age=56, job="CEO";

/* 多行
var name="Gates",
          age=56,
          job="CEO";
*/
{% endhighlight %}

**建议所有变量都使用 var 进行声明**，避免如下错误：

{% highlight javascript %}
function test(){
	message = "hi"; // 不加 var，message 成为全局变量
}
{% endhighlight %}

**在函数内加 var 关键字进行声明的变量为局部变量，除此之外所有的变量声明都是全局变量的**。

<h2 id="data">数据类型</h2>

JS 共有Undefined、Null、Boolean、Number、String、Array、Object对象七种数据类型，动态类型支持相同变量的不同类型赋值：

{% highlight javascript %}
var x; // x 为 undefined
var x = 6; // x 为数字
var x = "Bill"; // x 为字符串
{% endhighlight %}

###**Undefined**

Undefined 类型只有一个特殊值 undefined，通过 typeof 判断变量类型，若得到的结果是 Undefined 类型，则变量可能已声明未赋值，也可能变量不存在，因此建议对每个变量进行显式初始化。

###**Null**

Null 类型也只有一个特殊值 null，表示空对象，因此使用 typeof 判断会返回 object 类型。**实际上， undefined 派生自 null，因此 null == undefined**。

**尽管声明变量时无需显示赋值为 undefined，对于对象变量却建议赋值为 null，有助于区分 null 和 undefined**。

###**Boolean**

Boolean 类型有两个字面值 true 和 false，其他数据类型的值都可以转换成 Boolean 类型，**其中，除了空字符串（String）、0/NaN（Number）、null（Object）、undefined（Undefined）转换为 false，其他值都为 true**。

###**Number**

**1，表示**

Number 使用 IEEE754 标准表示整数和浮点数，除了基本的十进制，也支持八进制（0开头）和十六进制（0x开头）表示，**若字面值超出范围则忽略前导作十进制计**。

{% highlight javascript %}
var intNum = 55; // 整数
var octalNum1 = 070; // 八进制的 56
var octalNum2 = 079;  // 无效的八进制数值 -- 解析为79
var 0ctalNum3 = 08;  // 无效的八进制数值 -- 解析为 8
var hexNum1 = 0xA; // 十六进制的 10
var hexNum2 = 0x1f; // 十六进制的 31
{% endhighlight %}

**2，NaN**

NaN 是一个特殊的数值，为了不抛出错误 JS 增加了这个值，**首先，任何涉及NaN的操作 (例如 NaN/10) 都会返回 NaN ，这个特点在多步计算中有可能导致问题。其次，NaN 与任何值都不相等，包括 NaN 本身**。isNaN() 函数接受任意类型的参数以确定这个参数是否“不是数值”。

**3，数值转换**

Number()、parseInt() 和 parseFloat() 三个函数将其他类型的值转换成数值。

**不建议使用 Number() 函数，转换整数使用 parseInt()，转换浮点数使用 parseFloat()，其中，为了防止对于不同进制数的转换错误，parseInt() 函数还可以指定基数作为第二个参数，而 parseFloat() 只解析十进制值**。

{% highlight javascript %}
var num1 = parseInt("1234blue");  // 1234
var num2 = parseInt(""); // NaN
var num3 = parseInt("0xA"); // 10(十六进制数)
var num4 = parseInt(22.5); // 22
var num5 = parseInt("070"); // 56(八进制数)
var num6 = parseInt("70"); // 70(十进制数)
var num7 = parseInt("0xf"); // 15(十六进制数)
var num7 = parseInt("f", 16); // 15(十六进制数)
var num1 = parseFloat("1234blue"); // 1234 (整数)
var num2 = parseFloat("0xA"); // 0
var num3 = parseFloat("22.5"); // 22.5
var num4 = parseFloat("22.34.5"); // 22.34
var num5 = parseFloat("0908.5"); // 908.5
var num6 = parseFloat("3.125e7"); // 31250000
{% endhighlight %}

###**String**

String 类型可以用单引号或者双引号表示，**字符串是不可变的**，因此：

{% highlight javascript %}
var lang = "Java";
lang = lang + "Script";
{% endhighlight %}

这个操作实际上还生成并销毁了一个 "Script" 字符串。

字符串转换可以使用转型函数 String()，也可以使用每个类型的 toString() 方法。

###**Object**

JS 拥有诡异的面向对象特性，JS 中的所有事物都是对象：字符串、数值、数组、函数... 同时提供多个内建对象，比如 String、Date、Array 等等，对象只是带有属性和方法的特殊数据类型，同时，JS 允许自定义对象。

{% highlight javascript %}
// 定义一个对象
var person={firstname:"Bill", lastname:"Gates", id:5566};

// 两种寻址方式
name=person.lastname;
name=person["lastname"];
{% endhighlight %}

<h2 id="oper">操作符</h2>

**1，递增递减操作符**

借鉴自 C，同时支持字符型、布尔值、浮点数和对象。

{% highlight javascript %}
ar s1 = "2";
var s2 = "z";
var b = false;
var f= 1.1;
var o = {
  valueOf: function(){
    return -1;
  }
};

s1++; // 值变成数值3
s2++; // 值变成 NaN
b++; // 值变成数值1
f--; // 值变成 0.100000000000000000000009 (由于浮点舍入错误所致)

o--; // 值变成数值 -2
{% endhighlight %}

**2，逻辑操作符**

{% highlight javascript %}
var myObject = preferredObject || backupObject;
{% endhighlight %}

可以通过这样赋值以避免 null 或 undefined 值。

**3，关系操作符**

**字符串比较的是两个字符串中对应位置的每个字符的字符编码值**，如：

{% highlight javascript %}
// 所有大写字母编码值都小于小写字母
var result = "Brick" < "alphabet" ; // true
// 2 的编码值小于 3
var result = "23" < "3" ; // true
// "23"会被转换成 23
var result = "23" < 3; // false
// "a"会被转换成 NaN
var result = "a" < 3 ; // false
{% endhighlight %}

**4，相等操作符**

**相等（==）和不相等（!=）会先转换再比较，全等（===）和不全等（!==）仅比较不转换**。

{% highlight javascript %}
null == undefined; // true
"NaN" == NaN; // false
NaN == NaN; // false
"5" == 5; // true
"5" === 5; // false
{% endhighlight %}

<h2 id="func">函数</h2>

**1，参数**

**JS 中所有参数传递的都是值，不可能通过引用传递参数，同时，参数的个数并不做限制，也不影响运行，或者说，JS 的函数是没有参数的**。

{% highlight javascript %}
function sayHi () {
// arguments 参数数组
  alert("Hello " + arguments[0] + "," + arguments[1]);
}
{% endhighlight %}

**另外， arguments 对象可以与命名参数一起使用**，如：

{% highlight javascript %}
function doAdd(num1, num2) {
  if(arguments.length == 1) {
    alert(num1 + 10);
  } else if (arguments.length == 2){
    alert(arguments[0] + num2);
  }
}

doAdd(10); // 20
doAdd(30, 20); // 50
{% endhighlight %}

上例中，由于 num1 的值与 arguments[0] 的值相同，因此它们可以互换使用。同时，由于没有传递值的命名参数将自动被赋予 undefined 值，因此，如果只给 doAdd() 函数传递了一个参数，则 num2 中就会保存 undefined 值。

**2，没有重载**

JS 的参数特性注定了它无法实现像 Java 语言那样的重载，如果在 JS 中定义了两个名字相同的函数，则该名字只属于后定义的函数。

...