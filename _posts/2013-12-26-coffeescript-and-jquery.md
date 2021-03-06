---

title: CoffeeScript 和 jQuery

layout: post

---

* [CoffeeScript](#coff)
  * [安装](#inst)
  * [命令](#comm)
  * [语法](#gram)
* [jQuery](#jque)
  * [DomReady](#dom)
  * [Form](#form)
  * [Callback](#call)
* [总结](#summ)

自从遇到 [NodeJS](http://allenyip.com/2013/01/30/meeting-nodejs.html) 之后，我对 [CoffeeScript](http://coffeescript.org/) 一直念念不忘（主要还是因为各路码农对它无尽的赞美），今天抽空稍微鼓捣了一下，权当入门了，发个帖子记录一下。

<h2 id="coff">CoffeeScript</h2>

CoffeeScript 是著名的 JavaScript 开发者 [Jeremy Ashkenas](http://ashkenas.com/) 的杰作（这货搞的 Backbone 和 Underscore 也都分分钟亮瞎我的狗眼）。

>CoffeeScript is a little language that compiles into JavaScript.
>It's just JavaScript

这两句话简洁精准地定义了 CoffeeScript，它的唯一目的就是简化 JavaScript 的编写，JavaScript 确实有许多令人费解的地方，并且糟糕的糅合了多种语言模式，据说创造者 Brendan Eich 由于赶进度只用了十天就发布了它的雏形，与其说 CoffeeScript 是为了简化 JavaScript 编程，不如说它是为了取悦那些饱受摧残的开发者。

<h3 id="inst">安装</h3>

CoffeeScript 使用 Node 包的方式进行下载安装，因此需要先安装 Node 和 npm。安装过程比较简单，不再赘述，新的版本甚至支持 Windows 平台的一键安装。

之后在终端输入 npm 安装语句即可。

'''shell
> sudo npm install -g coffee-script
```

<h3 id="comm">命令</h3>

安装完毕即可使用 coffee 命令进行编译，常用的几个参数有：

* -c, --compile: 将 .coffee 脚本编译为 .js
* -o, --output [DIR]: 将所有 JavaScript 文件写入指定文件夹
* -w, --watch: 检测文件是否更新
* -j, --join [FILE]: 多个脚本写入同一文件
* null: 输入 coffee 回车将开启 REPL（结束 Ctrl-D, 多行 Ctrl-V）

<h3 id="gram">语法</h3>

CoffeeScript 代码确实美丽，编译时发生的许多事也令人着迷。拿函数的语法来说，简化后的语法简直跟普通的赋值语句相差无几：

```coffeescript
# function
square = (x) -> x * x

fill = (container, liquid = "coffee") ->
  "Filling the #{container} with #{liquid}..."

### JavaScirpt
var fill, square;

square = function(x) {
  return x * x;
};

fill = function(container, liquid) {
  if (liquid == null) {
    liquid = "coffee";
  }
  return "Filling the " + container + " with " + liquid + "...";
};
###
```

还可以像 YAML 那样赋值：

```coffeescript
# value
kids =
  brother:
    name: "Max"
    age:  11
  sister:
    name: "Ida"
    age:  9

### JavaScirpt
var kids;

kids = {
  brother: {
    name: "Max",
    age: 11
  },
  sister: {
    name: "Ida",
    age: 9
  }
};
###
```

数组的迭代更是强大：

```coffeescript
# array
list = [1, 2, 3, 4, 5]

cubes = (math.cube num for num in list)

copy = list[0...list.length]

countdown = (num for num in [10..1])

### JavaScript
var copy, countdown, cubes, list, num;

list = [1, 2, 3, 4, 5];

cubes = (function() {
  var _i, _len, _results;
  _results = [];
  for (_i = 0, _len = list.length; _i < _len; _i++) {
    num = list[_i];
    _results.push(math.cube(num));
  }
  return _results;
})();

copy = list.slice(0, list.length);

countdown = (function() {
  var _i, _results;
  _results = [];
  for (num = _i = 10; _i >= 1; num = --_i) {
    _results.push(num);
  }
  return _results;
})();
###
```

具体的语法可以参考 [CoffeeScript](http://coffeescript.org/) 主页，上面还提供方便的在线编译，即使像我这样的 JavaScript 菜鸟也能很好的快速学习。

<h2 id="jque">jQuery</h2>

遥想当年，年轻小伙 [John Resig](http://ejohn.org/) 在无数个寂寞的夜晚编写 jQuery 之时，他一定没想到如今这玩意儿能火成这样。现在，几乎所有写过网页的人都对 jQuery 不会陌生，作为最流行的 JavaScript 库，jQuery 为无数的前端工程师立下了汗马功劳。

此处自动隐藏关于 jQuery 的无数介绍和赞美，下面展示如何将 CoffeeScript 和 jQuery 进行合体。

<h3 id="dom">DomReady</h3>

CoffeeScript 中的 ready 函数长这样：

```coffeescript
# CoffeeScript ready
$->
  some()
  init()
  calls()

### JavaScript
$(document).ready(function() {
  some();
  init();
  calls();
})
###

# CoffeeScript ready v2
jQuery ($) ->
  # your code here!

### JavaScript
jQuery(function($) {
  // your code here!
});
###
```

<h3 id="form">Form</h3>

一条语句获取表单输入值（太尼玛神奇了）：

```coffeescript
# CoffeeScript
formValues = (elem.value for elem in $('.input'))

### JavaScript
var elem, formValues;
formValues = (function() {
  var _i, _len, _ref, _results;
  _ref = $('.input');
  _results = [];
  for (_i = 0, _len = _ref.length; _i < _len; _i++) {
    elem = _ref[_i];
    _results.push(elem.value);
  }
  return _results;
})();
###
```

<h3 id="call">Callback</h3>

看一个简单的回调函数例子：

```coffeescript
# CoffeeScript
object =
  func: -> $('#div').click => @element.css color: 'red'

### JavaScript
var object;
var __bind = function(fn, me){ return function(){ return fn.apply(me, arguments); }; };
object = {
  func: function() {
    return $('#div').click(__bind(function() {
      return this.element.css({
        color: 'red'
      });
    }, this));
  }
};
###
```

...

<h2 id="summ">总结</h3>

前端开发确实是一件很美妙的事儿，可惜我还没有视死如归踏上这条道路的决心，现在正处于想学的东西太多，时间完全不够用的尴尬地步，然而我还在不知疲倦的给自己挖坑，就像现在，CoffeeScript 这个坑才刨到一半，又想跑去挖 Ruby 的坑了，简直就像一路在捡芝麻的小猴子，早晚要虚度这人生呐。