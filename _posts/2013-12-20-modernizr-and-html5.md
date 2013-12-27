---

title: Modernizr 和 HTML5

layout: post

---
今天看了 [HTML5 Boilerplate](http://html5boilerplate.com/) 对 HTML5 又重燃热情，甚至妄想从明天起所有桌面/平板/手机浏览器都能完整地支持 HTML5。

可惜现实永远是残酷的，去[百度](http://tongji.baidu.com/data/browser)看看上个月的浏览器市场份额吧，中国每十个网民中就有一个还在使用 IE6，这叫我如何相信。

![Borwser Share](https://dl.dropboxusercontent.com/u/36470533/Photos/browser_201311.jpg)

难道所有的前端开发者们都必须实现两个版本，在每次请求响应中判断浏览器的类型去做返回吗，当然不是，使用 [Modernizr](http://modernizr.com/) 吧，它~~无法解决 HTML5 的兼容问题~~（加载脚本），但提供了一个优雅方便的解决之道。

##**MODERNIZR**

Modernizr 是一个用来检测浏览器HTML5特性的开源 JavaScript 库，有了它之后，我们不再需要手动编写代码判断某浏览器是否支持某 HTML5 特性，而只需专注于具体实现，大大提高了开发效率。

###**下载**

与大部分 JS 库相同，Modernizr 提供了 Development 和 Production 两种版本供用户选择，Development 版本是给我这种菜鸟使用的完整包，Production 版本则是定制的无注释换行的精简包。下载之后放入项目 js 文件夹，打开页面在 <head> 元素间加入如下代码：

{% gist 8144258 modernizr1.htm %}

然后在 <html> 根元素中添加一个 no-js 类：

{% gist 8144258 modernizr2.htm %}

如果 Modernizr 正常运行，它会移除 no-js，添加一个 js 类；假如用户浏览器禁用 JavaScript，则 Modernizr 无法运行，html 标签就有 no-js。这是如何实现的呢，其实也不难。

###**机制**

页面在加载完 Modernizr.js 后，会自动运行生成一个包含浏览器支持特性的 Modernizr 对象，另外，它还会给 <html> 标签添加新的类(class)，以标明浏览器对 HTML5、CSS3 特性的支持情况。下面是 Chrome 31 for Windows 的界面（顺便赞一下Chrome）：

{% gist 8144258 modernizr3.htm %}

###**DEMO**

以一个输入界面为例：

{% gist 8144258 modernizr4.css %}

若浏览器支持 box-shadow ，<html> 中的 class 会有 boxshadow 类，否则的话会有no-boxshadow类，从而在 CSS 文件中依据浏览器特性进行输出，在整个判断或者匹配的过程中我们甚至不需要编写任何 JS 代码。

###**加载脚本**

针对一些不兼容的浏览器，Modernizr 可以通过 Modernizr.load() 功能加载 [shim/polyfills](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills) 脚本，具体使用方法如下：

{% gist 8144258 modernizr5 %}

* test：测试的特性
* yep：浏览器支持加载的脚本
* nope：浏览器不支持加载的脚本
* complete：加载之后运行的函数

Modernizr.load() 的优点在于可以根据测试浏览器的支持度，有条件地加载脚本，还支持异步地加载外部脚本，有助于提高网站性能。更多的脚本还可访问 [yepnope](http://yepnopejs.com/) 获取。

###**总结**

Modernizr 对前端开发者来说绝对是一个利器，节省了大量的代码及工作量，甚至可以为旧浏览器实现新特性。然而作为一个客户端 JS 库，实现功能的同时更应保证快速响应，是否使用 Modernizr 帮助开发还应仔细斟酌，比如，使用 IE6 的用户还需要为他加载脚本吗，呵呵。 