---

title: CSS3 自适应网页设计

layout: post

---
随着手机、平板等不同分辨率的移动设备的普及，针对不同终端的网页设计变得极为困难。2010 年，Ethan Marcotte 提出了「自适应网页设计」（[Responsive Web Design](http://www.alistapart.com/articles/responsive-web-design/)）的概念，即一种可以自动识别屏幕宽度并做出相应调整的网页设计技术。

本文主要通过 CSS3 的 Media Queries 技术实现自适应网页设计。

##**工具**

首先可以戳 [这里](http://mediaqueri.es/) 查看具体的示例，改变浏览器尺寸后确实可以看到有趣的效果，其实背后的实现原理并不复杂，也不需要特殊的工具，熟悉的文本工具以及一个跟得上时代的浏览器即可。

（推荐一个测试多个尺寸的 JS 书签工具（[戳我](http://www.benjaminkeen.com/open-source-projects/smaller-projects/responsive-design-bookmarklet/)），方便至极

###**Meta**

首先，在代码头部加入 meta 标签：

> <meta name="viewport" content="width=device-width, initial-scale=1" />

viewport 是网页默认的宽度和高度，上面这行代码的意思是，网页宽度默认等于屏幕宽度，原始缩放比例为 1.0，即网页初始大小等于屏幕面积。

对于不支持 Media Queries 的 IE8 及其更低版本可以使用 media-queries.js 或 respond.js 脚本实现支持。

> <!--[if lt IE 9]> <script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></script> <![endif]-->

###**HTML**

{% gist 8970320 adaptive.html %}

示例代码（[Demo](http://webdesignerwall.com/demo/responsive-design/index.html)）为最常用的页面布局，包括 Header、Content、Sidebar 和 Footer。

###**Media Queries**

{% gist 8970320 adaptive.css %}

这段 CSS3 Media Queries 代码是实现自适应网页设计的关键，它通过页面宽度自动渲染网页，实现自适应。

注：本文基于 Nick La 的这篇 [文章](http://webdesignerwall.com/tutorials/responsive-design-in-3-steps)，更详细的方案可以 [戳此](http://webdesignerwall.com/tutorials/responsive-design-with-css3-media-queries) 查看。

TBD: 图片、视频的自适应