---

title: Prolog, 逻辑编程和编程范式

layout: post

---
在人工智能课上做了一个介绍 Prolog 语言的课题，也因此才有机会去研究困惑我许久的逻辑编程到底是什么东西。（没有压力就没有动力，我是一头懒惰的猪。）本来这篇文章只想谈谈 Prolog 语言，没想到越写越多，最后成了「Prolog, 逻辑编程和编程范式」。

##**编程范式**

基于类编程和基于原型编程、声明式编程和命令式编程、面向对象编程和面向过程编程...这些所谓的编程范式实在是混乱又复杂，而维基娘对于编程范式的定义倒是简洁：

>编程范式，指的是计算机编程的基本风格或典范模式。

得，这种方法论世界观的东西向来不合我胃口，于是我又找到了 Peter Van Roy 制作的 [编程范式图][1]：

![Principal Programming Paradigms][2]

由图可知，一门语言支持多种编程范式是很常见的事，对简洁主义者来说这事儿并不那么讨人喜爱。当然，这张图并没有让我完全搞清楚编程范式的具体分类，引用它的原因除了喜欢这种花花绿绿看起来很聪明的图之外，更因为标题下的这句话：

>More is not better (or worse) than less, just different.

的确，多与少并没有好坏之分，仅仅是不同而已。我在大肆赞同这句话的同时又忍不住想往后面加上这么一句：

>but less is beautiful.

总之，经过我不完全的搜索和不耐烦的总结后，我从这 [一堆编程范式][3] 中挑出以下四个重点关注：

* 命令式编程(Imperative Programming)
* 函数式编程(Functional Programming)
* 面向对象编程(Object-oriented Programming)
* 逻辑编程(Logic Programming)

注：关于 [计算模型][4]，函数式编程基于 λ 演算，逻辑编程基于一阶逻辑，而命令式编程和面向对象编程则是基于图灵机。

##**命令式编程**

>First DO THIS and next DO THAT

遵循 [冯·诺依曼][5] 结构的传统编程范式，像是严谨规律的军队守则，编程语言代表有 Fortran，Pascal 和 C。

##**函数式编程**

>Evaluate an expression and use the resulting value for something

遵循数学运算和函数理论，将函数视为数据类型，编程语言代表有 Lisp， Erlang 和 Haskell。(函数式编程似乎将要迎来第二春呐。[为什么][6]）

##**面向对象编程**

>Send messages between objects to simulate the temporal evolution of a set of real world phenomena

近十年最火的编程范式，以对象为基础支持封装、继承和多态特性，编程语言代表有 Smalltalk， C++ 和 Java。

##**逻辑编程**

>Answer a question via search for a solution

最另类的编程范式，以数学逻辑中的一阶逻辑为基础，编程语言代表有 Prolog，Mercury 和 Logtalk。

逻辑编程通过设定规则而非设定步骤来解决问题，通过事实和规则的匹配得到结果。事实上，逻辑编程的实现机制十分简单，纯逻辑式语言能实现的功能绝大多数复合型语言都可以，然而在执行效率和运算成本上，逻辑编程语言更具优势。下面以 Prolog 语言为例进行介绍。

##**PROLOG**

Prolog(Programming in Logic)最早由法国马赛大学的 Alain Colmerauer 等人开发并与 1972 年诞生，至今仍被广泛运用于自然语言处理，专家系统等人工智能领域。我不喜欢重铸轮子，有关该语言的学习请猛戳 Patrick Blackburn 等人写的 [Learn Prolog Now!][7]，开发工具推荐强烈推荐开源小巧的 [SWI-Prolog][8]。

Prolog 简单高效，解决汉诺塔问题只需要两行代码：

{% highlight prolog %}

// prolog hanoi
move(1,X,Y,\_):-write('Move top disk from '),write(X),write(' to '), write(Y), nl.
move(N,X,Y,Z):-N>1,M is N-1,move(M,X,Z,Y),move(1,X,Y,\_),move(M,Z,Y,X).

{% endhighlight %}

举一个简单的例子演示 Prolog 仅有的三种语句：事实，规则和结果。
有如下家庭关系图：

![Family Diagram][9]

在 family.pl 中定义的事实和规则如下：

{% highlight prolog %}

/* 事实 */
male(baba).
male(yeye).
female(nainai).
female(mama).
male(wo).
father(yeye,baba).
father(baba,wo).
mother(nainai,baba).
mother(mama,wo).

/* 规则 */
grandfather(X,Y):-father(X,Z),father(Z,Y).
grandmother(X,Y):-mother(X,Z),father(Z,Y).
daughter(X,Y):-parent(X,Y),female(Y).
son(X,Y):-parent(Y,X),male(X).
parent(X,Y):-father(X,Y);mother(X,Y).
ancestor(X,Y):-parent(X,Y).
ancestor(X,Y):-parent(X,Z),ancestor(Z,Y).

{% endhighlight %}

结果或查询语句以 "?-" 开头如下所示：

![Prolog Query][10]

Prolog 还可以应用于自然语言处理和专家系统中，其主要的机制与此几乎一样，PPT 附上。

<iframe src="http://www.slideshare.net/slideshow/embed_code/16338774" width="597" height="486" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="http://www.slideshare.net/allenyip/prolog-16338774" title="Prolog" target="_blank">Prolog</a> </strong> from <strong><a href="http://www.slideshare.net/allenyip" target="_blank">allenyip</a></strong> </div>

[1]:http://www.info.ucl.ac.be/~pvr/paradigmsDIAGRAMeng108.pdf
[2]:http://i.imgur.com/oACpKEi.png
[3]:http://en.wikipedia.org/wiki/Category:Programming_paradigms
[4]:http://en.wikipedia.org/wiki/Computational_model
[5]:http://en.wikipedia.org/wiki/John_von_Neumann
[6]:http://www.cse.chalmers.se/~rjmh/Papers/whyfp.html
[7]:http://www.learnprolognow.org/
[8]:http://www.swi-prolog.org
[9]:http://i.imgur.com/ekXPwFP.png
[10]:http://i.imgur.com/PX5NBlR.png