---

title: HTML/CSS 基础知识

layout: post

---
基础知识太薄弱了，学了 [Shay Howe](http://learn.shayhowe.com/html-css/) 制作的HTML/CSS新手教程后受益匪浅，发个文章总结一下。

##**HTML**

###**术语**

* **Elements（元素）**：HTML文档由元素构成，元素指的是开始标签到结束标签之内的所有内容。如&lt;a href="//allenyip.com/">Allen Yip&lt;/a> 表示一个链接元素。

* **Tags（标签）**：用来标记元素，通常以开始标签和结束标签成对出现。如&lt;a>和&lt;/a>
* **Attributes（属性）**：为元素提供额外信息，在开始标签中以Name/Value对定义。如href="//allenyip.com/"

###**结构和语法**

{% gist 9802636 html0.html %}

所有HTML文档都必须包含doctype, html, head和body标签。

* **doctype**：位于第一行，指示浏览器当前HTML 文档的版本。

* **html**：跟在doctype之后的第一行为&lt;html>标签，文档最后一行为&lt;/html>标签。

* **head**：包含meta数据，文档title以及一些内页CSS/JS和一些外部文件。head标签中的内容都不显示在网页上。

* **body**：包含所有网页内容。

（更新：HTML5结构中的语义化标签）

![html5](http://learn.shayhowe.com/assets/courses/html-css-guide/elements-semantics/html5.png)

* **header**：文档头部区域。

* **nav**：文档导航区域。

* **article**：正文区域。

* **section**：正文区域下的内容区域。

* **aside**：侧边栏区域。

* **footer**：文档尾部区域。

注：article, section, aside和div的区别。

div与其他三个不同，一般使用div包含内容是为了对内容的样式进行修改，div属于可以在HTML文档中自由使用的非语义化标签；section通常表示为文档中的一节，可包含一个标题和一段文本；article通常表示一个文档内容的独立片段，可包含多个section；aside则表示文档中的附属信息。 

##**CSS**

###**术语**

* **selectors（选择器）**：选择该CSS语句所要作用的元素。如div, #id, .class...

* **properties（属性）**：即要作用的样式，如color, font, background...

* **values（值）**：不同属性具有不同的取值，如 #ccc, 16px...

###**结构和语法**

{% gist 9802636 html1.css %}

####属性连写

{% gist 9802636 html2.css %}

####选择器

* **类型（元素）选择器**：元素名，作用于所有的元素。如p {...}

* **ID选择器**：#号加ID名，作用于文档中ID相同的唯一元素。如#id {...}

* **类选择器**：.号加类名，作用于文档中类名相同的所有元素。如 .class {...}

####选择器组合

选择器可以有多种组合方式。如通过逗号或者上下文等方式进行组合：

{% gist 9802636 html3.css %}

####引用CSS

* **行内样式**：写在开始标签中，相当于为元素增加style属性。如<p style="color: #ccc;" {...}

* **内页样式**：写在head标签中。如：

{% gist 9802636 html4.css %}

* **外部样式**：写在单独的文件中，在head标签中引入。如&lt;link ref="stylesheet" href="style.css" />

####CSS重置&lt;a id="reset" />

CSS重置的目的是为了防止浏览器默认样式对HTML文档进行修改，因此通常会为自己的HTML文档添加一个reset.css样式，比较出名的有 [Eric Meyers reset](http://meyerweb.com/eric/tools/css/reset/)。

###**框模型**

**页面上的每个元素都是一个矩形框**。可通过margin, border, pdding, width, height等属性对其大小进行定义。计算一个元素真正的宽度和高度可采用以下公式：

宽度：margin-right + border-right + padding-right + width + padding-left + border-left + margin-left

高度：margin-top + border-top + padding-top + height + padding-bottom + border-bottom + margin-bottom

####**width和height**

每个元素都有一个默认的继承的宽度和高度，对于文档中的关键元素我们可能会指定它的宽度和高度。

元素默认的高度取决于它的内容；默认的宽度取决于它的显示类型，块元素默认宽度为100%，行内元素则根据内容调整宽度。行内元素无法使用固定大小，因此，**width和height属性只对块元素起作用**。

####**margin, padding和border**

每个浏览器都有默认的margin和padding值，因此需要对其进行[重置](#reset)。属性border界于margin和paddnig之间并且对元素提供了一个轮廓，通常有三个值：宽度、类型和颜色。

需要注意，两个相邻的margin会叠加为最大值。

####**浮动**

![float](http://learn.shayhowe.com/assets/courses/html-css-guide/box-model/floats.png)

**浮动元素会被自动设置成块元素，块元素声明了float属性之后将不再独占一行，而脱离文档的普通流浮动到左侧或者右侧，直到它的外边缘碰到包含框或另一个浮动框的边框为止。**

浮动元素破坏了文档的普通流，此时其他元素会自动围绕浮动元素，应此对其他元素应用clear属性可使其返回普通流中（通常使用clearfix类）。

####**定位**

除了浮动外也可以使用定位进行布局设置。默认的position值是static，表示元素位于普通流中；relative则允许通过top, right, bottom和left等属性进行位置偏移设置；absolute和fixed与erlative一样采用偏移机制，但此时元素已经不处于普通流中，而是相对于上一个relative定位的父元素进行偏移。

##**代码验证**

代码验证器可以为我们找出HTML或CSS文档中的语法错误。W3C为开发者提供了简单强大的 [HTML验证器](http://validator.w3.org/) 和 [CSS验证器](http://jigsaw.w3.org/css-validator/)。

##**排版**

###**颜色**

完整的颜色值由一个RGB组成的十六进制符号来定义，如#000000。同时也存在多种写法，如#000与#000000等价，red与#ff0000等价，也与rgb(ff, 0, 0)。

###**字体**

{% gist 9802636 html5.css %}

* font-family属性设置字体系列并用逗号隔开，若第一个值不可用则采用第二个，以此类推。并且始终提供一个类族名称作为最后的选择。

**Web安全字体***指的是每个浏览器（PC/Pad/Phone）都默认支持的字体，包括Arial, Times New Roman, Verdana, Georgia, Tahoma...

**Web嵌入字体**指的是可通过CSS@font-face属性载入的在线字体，目前中文字体还比较少。

* font-size属性设置字体大小，一般用px为单位，其次是em，不使用pt和百分比。

* font-style属性设置字体风格，一般使用normal和italic。

* font-weight属性设置文本的粗细。

* line-height属性设置行间的距离（行高），一般设置成1.5倍行距。

###**文本**

{% gist 9802636 html6.css %}

* text-align属性规定元素中的文本的水平对齐方式。

* text-decoration属性规定添加到文本的修饰。

* text-indent属性规定文本块中首行文本的缩进。

* text-shadow属性规定文本阴影。

* text-transform 属性控制文本的大小写。

* letter-spacing 属性增加或减少字符间的空白（字符间距）。

* word-spacing属性增加或减少单词间的空白（即字间隔）。

###**列表**

{% gist 9802636 html7.css %}

无序列表ul和li，有序列表ol和li，定义列表dl和dt, dd，三种列表可以相互嵌套。

{% gist 9802636 html8.css %}

一个导航列表的设计。list-style-type属性设置列表项标记的类型，也可通过背景图片自行定义。

TBD 媒体文件，表单，表格，编程陋习