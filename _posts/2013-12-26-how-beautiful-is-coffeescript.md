---

title: CoffeeScript 有多美

layout: post

---
自从遇到 [NodeJS](http://allenyip.com/2013/01/30/meeting-nodejs.html) 之后，我对 [CoffeeScript](http://coffeescript.org/) 一直念念不忘（主要还是因为各路码农对它无尽的赞美），今天抽空稍微鼓捣了一下，权当入门了，发个帖子记录一下。

##**CoffeeScript**

CoffeeScript 是著名的 JavaScript 开发者 [Jeremy Ashkenas](http://ashkenas.com/) 的杰作（这货搞的 Backbone 和 Underscore 也都分分钟亮瞎我的狗眼）。

>CoffeeScript is a little language that compiles into JavaScript.   
>It's just JavaScript

这两句话简洁精准地定义了 CoffeeScript，它的唯一目的就是简化 JavaScript 的编写。 

##**安装**

CoffeeScript 使用 Node 包的方式进行下载安装，因此需要先安装 Node 和 npm。安装过程比较简单，不再赘述，新的版本甚至支持 Windows 的一键安装。

之后在终端输入 npm 安装语句即可。

>sudo npm install -g coffee-script

##**命令**

安装完毕即可使用 coffee 命令进行编译，常用的几个参数有：

* -c, --compile: 将 `.coffee` 脚本编译为 `.js`
* -o, --output [DIR]: 将所有 JavaScript 文件写入指定文件夹
* -w, --watch：检测文件是否更新
* -j, --join [FILE]: 多个脚本写入同一文件
* null: 输入 coffee 回车将开启 REPL（结束 Ctrl-D, 多行 Ctrl-V）

##**语法**

具体的语法可以参考 CoffeeScript 主页，它甚至提供方便的在线编译，即使像我这样的 JavaScript 菜鸟也能很好的快速学习，CoffeeScript 代码确实美丽，编译时发生的许多事也令人着迷。


##**作用域**

为了避免 JS 中全局变量的乱用，CoffeScript 变量都会使用 Immediate Function 语法进行包装。

声明变量时无需使用 `var`，编译器自动添加到同时会将所有变量放到作用域的顶部。

tbd...