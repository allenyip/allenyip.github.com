---

title: 坑爹的 SAE

layout: post

---
云计算火了这么多年，各大公司的云平台也纷纷上线，从国外的 GAE/Azure/EC2/Heroku/Appfog/OpenShift 到国内的 SAE/BAE/ACE/TAE 等，支持的语言、服务、资费等都各不相同，如何选好云平台作为托管商，确实是一个另开发者和企业头疼的问题。

说实话，要是没有 GFW 和较快的网速，国内的这些 XAE 估计都难有出路，起步较晚技术落后价格昂贵等缺点不说，单单看到他们糟糕的主页都会令开发者糟心（GAE 也丑，但人家丑得简洁）。

又扯远了，本文只是为了记录一下最近将 PHP 项目放到 SAE 时的种种，言归正传吧。  
早就听说这些坑爹的平台都存在各式各样的「规则」，在我看来其中有大部分规则压根不是为了云，而是平台底层在技术上难以快速有效地虚拟不同的架构（之前把玩 GAE 的时候就很少遇到这类问题·>·）。

直接将源码放到 SAE，毫无意外地各种报错。  
首先是 MySQL，开启服务后手动建立 MySQL 数据库，将代码中的数据库配置改为：

{% highlight php %}
<?php
// 数据库主机
define('C_DB_HOST', SAE_MYSQL_HOST_M);
//sae:数据库端口
define('C_DB_PORT', SAE_MYSQL_PORT);
// 数据库用户名
define('C_DB_USER', SAE_MYSQL_USER);
// 数据库密码
define('C_DB_PASSWORD', SAE_MYSQL_PASS);
// 数据库名称
define('C_DB_NAME', SAE_MYSQL_DB);
?>
{% endhighlight %}

其次是 Memcache，由于 SAE 不支持直接写文件，Smarty 模板引擎也不支持，需将缓存写到 Memcache 中，开启 Memcache 服务后，将代码中的 Smarty 配置改为：

{% highlight php %}
<?php
// smartytpl为存放缓存的文件夹
$smarty->compile_dir = 'saemc://smartytpl/';
$smarty->cache_dir = 'saemc://smartytpl/';
$smarty->compile_locking = false;
?>
{% endhighlight %}

然后是 Storage，由于平台限制，写入的文件都要放到 Storage 中，开启服务后新建 domain，将代码中的文件写入相关改为：

{% highlight php %}
<?php
///...
// 判断文件是否存在
bool fileExists(string $domain, string $filename)
// 获取指定domain下的文件名列表，可用于判断文件夹存在
array getList(string $domain, [string $prefix = '*'], [int $limit = 10], [int $skip = 0])
// 获取文件内容
mixxed read (string $domain, string $filename)
// 将数据写入存储
string write (string $domain, string $destFile, string $content, [int $size = -1], [array $attr = array()], [bool $compress = false])
// 将文件上传入存储
 string upload (string $domain, string $destFile, string $srcFile, [array $attr = array()], [bool $compress = false])
//...
 ?>
{% endhighlight %}

其他 SAE 服务有待添加...