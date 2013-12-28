---

title: 一个小 BUG

layout: post

---
用 Jekyll 写博客是一件很酷的事情，当初就是看到 [Github][1] 的创始人之一 [Tom Preston-Werner][2] 写的「[像黑客一样写博客][3]」一文后大为心动，立刻搭了博客改了模板买了域名。同时，用 Jekyll 写博客也是一件非常 SABIXI 的事儿，更别说提交文章之后收到 BUG 邮件时心情有多寂寥了。

这个 BUG 其实很简单，但调试它却耗费了我一番精神，原因主要有：

* 我很久没发文章了。——这是罪魁祸首
* Github 的 Jekyll 服务器更新了。——我却没收到邮件
* 我想在博客里加入 Google Analytics(GA) 服务。——尽管访问量为零

于是，当我加入 GA 代码同时发布新文章之后收到了第一封 BUG 邮件，机智如我想当然以为是 GA 服务出了问题，于是改代码改配置忙活半天，BUG 依旧；然后我把 GA 代码移除提交，仍旧无用；最后迫不得已在本地输入 `jekyll --safe` 调试，果然报了如下错误：

>Liquid Exception: undefined method 'xmlschema' for nil:NilClass in default

Google 解决后发现原来是网站的「[关于][4]」文件未使用 CCYY-MM-DD-title 命名，在传递日期时出错且这个错误在新版的 Jekyll 下是不允许的，换文件名显然不可行于是我干脆把日期删掉了，之后加入 GA 顺利提交。

不服输地 DEBUG 到深夜后我写下这篇文章顺便告诉自己：

* 不要想当然自以为是。
* 不要偷懒多写写文章。
* 不要熬夜了赶快去睡觉吧：）

顺便附上 Github 坑爹的 BUG 信息：

![BUG Email][5]

[1]:http://github.com
[2]:http://tom.preston-werner.com/
[3]:http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html
[4]:http://allenyip.com/about
[5]:http://i.imgur.com/dlD2Ox3.png