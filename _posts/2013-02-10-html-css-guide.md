---

title: HTML/CSS基础知识

layout: post

---

##**HTML**

###**术语**

* *Elements（元素）*：HTML文档由元素构成，元素指的是开始标签到结束标签之内的所有内容。如&lt;a href="//allenyip.com/">Allen Yip&lt;/a> 表示一个链接元素。

* *Tags（标签）*：用来标记元素，通常以开始标签和结束标签成对出现。如&lt;a>和&lt;/a>
* *Attributes（属性）*：为元素提供额外信息，在开始标签中以Name/Value对定义。如href="//allenyip.com/"

###**结构和语法**

{% gist 9802636 html0.html %}

所有HTML文档都必须包含doctype, html, head和body标签。

* *doctype*：位于第一行，指示浏览器当前HTML 文档的版本。

* *html*：跟在doctype之后的第一行为&lt;html>标签，文档最后一行为&lt;/html>标签。

* *head*：包含meta数据，文档title以及一些内页CSS/JS和一些外部文件。head标签中的内容都不显示在网页上。

* *body*：包含所有网页内容。


##**CSS**

###**术语**

* *selectors（选择器）*：选择该CSS语句所要作用的元素。如div, #id, .class...

* *properties（属性）*：即要作用的样式，如color, font, background...

* *values（值）*：不同属性具有不同的取值，如 #ccc, 16px...

###**结构和语法**

{% gist 9802636 html1.css %}

####属性连写

{% gist 9802636 html2.css %}

####选择器

* *类型（元素）选择器*：元素名，作用于所有的元素。如p {...}

* *ID选择器*：#号加ID名，作用于文档中ID相同的唯一元素。如#id {...}

* *类选择器*：.号加类名，作用于文档中类名相同的所有元素。如 .class {...}

####选择器组合

选择器可以有多种组合方式。如通过逗号或者上下文等方式进行组合：

{% gist 9802636 html3.css %}

####引用CSS
* *行内样式*：写在开始标签中，相当于为元素增加style属性。如<p style="color: #ccc;" {...}

* *内页样式*：写在head标签中。如：

{% gist 9802636 html4.css %}

* *外部样式*：写在单独的文件中，在head标签中引入。如&lt;link ref="stylesheet" href="style.css" />

####CSS重置

CSS重置的目的是为了防止浏览器默认样式对HTML文档进行修改，因此通常会为自己的HTML文档添加一个reset.css样式，比较出名的有 [Eric Meyers reset](http://meyerweb.com/eric/tools/css/reset/)。

##**代码验证**

代码验证器可以为我们找出HTML或CSS文档中的语法错误。W3C为开发者提供了简单强大的[HTML验证器](http://validator.w3.org/)和[CSS验证器](http://jigsaw.w3.org/css-validator/)。