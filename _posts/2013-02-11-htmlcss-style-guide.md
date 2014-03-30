---

title: HTML/CSS 风格指南

layout: post

---
看了 Google 的 HTML/CSS 风格指南（[戳我](https://code.google.com/p/google-styleguide/)），并根据自己的情况进行扩充，作为今后个人的编码规范（欢迎纠错）。

##**通用样式**

省略嵌入资源的协议。直接使用 '//' 相对地址。

##**通用格式化**

1，使用 2 个空格代替 Tab 键进行缩进。

2，所有代码只用小写，除非例外。

3，行末不留空格。

4，使用双引号而非单引号。

5，文件结尾添加一个空白行。

##**通用 Meta**

1，采用 UTF-8 编码。

2，多使用注释，用 &lt;!-- TODO: sth. --> 注释记录代办事项。

##**HTML**

1，首行添加文档类型，首选 &lt;!DOCTYPE html>

2，使用 [验证器](http://validator.w3.org/nu/) 验证 HTML 合法性。

3，为多媒体元素提供备选内容，如图像的 alt 属性。

4，HTML、CSS、JS 分离。

5，除 > &lt; & 和空格外不使用转义符。

6，可选标签尽量省略。

7，省略引用的类型 type。

8，block/list/table 元素另起一行并缩进。

9，meta, img等自闭合元素尾部不加斜线。

10，结束标签不能忽略。

11，为 html 标签增加 lang 属性设置页面语言（English-en, Chinese-zh）。

12，为 IE 设置版本，如 &lt;meta http-equiv="X-UA-Compatible" content="IE=Edge">。

13，属性的排列顺序：class > (id, name) > data-* > (src, for, type, href) > (title, alt) > (aria-*, role)。

14，布尔型属性不用赋值，如 checked, disabled 等。

15，代码量尽可能小，保证实用的前提下尽量遵循语义。

##**CSS**

1，使用 [验证器](http://jigsaw.w3.org/css-validator/) 验证 CSS 合法性。

2，ID 和 class 命名保证有意义的同时尽可能短。

3，带前缀或者需要分隔的命名使用 '-' 连接。

4，尽量减少选择器的层级。

5，尽量使用属性连写，常见的有 padding, margin, font, background, border, border-radius。

6，属性值为 0 时不加单位。

7，省略小数点前的 0。

8，省略 URL 中的引号。

9，#ebc 好于 #eebbcc。

10，尽量不用 CSS Hacks。

11，按字母排列进行声明。

12，每条声明独占一行，声明结束加分号。

13，标点符号前（后）添加空格，右花括号单独一行。

14，不要在 rgb(), rgba() 等值的内部逗号后加空格，如 rgb(0,0,0)。

15，选择器和声明分行，多个选择器各占一行。

16，选择器分组时，每个选择器各占一行。

17，相关的属性归为一组，并按照顺序排列：Positioning > Box model > Typographic > Visual。

18，不使用 import。

19，Media query 语句放在相关代码之后。

**如果你有自己的风格并且它很美，那么以上一切都是浮云。**
