---

title: 微信公众号

layout: post

---
这几天在实验室一如既往地打杂的同时，也忙着写一个坑爹的PHP项目。

里头唯一让我感点儿兴趣的就是微信公众号的开发，今天大致整了一下，发篇日志记录下流程。

微信5.0将公众号分成了服务号和订阅号（为了商业化？），个人用户只能申请订阅号。在公众平台首页注册，上传一张二到不行的手持身份证照之后便注册成功。
微信公众号除了基本的群发功能外，提供了两种模式：编辑模式和开发模式。编辑模式所支持的功能少得可怜，屌丝程序员果断选择了开发模式进行开发。

DENGDENG，还得先申请成为开发者，填写服务器的URL和Token，正好这个项目是要放到SAE上，于是乎我直接在SAE上新建了一个应用。服务器的配置直接在微信给出的示例上修改，代码如下：

/**
  * wechat php test
  */

//define your token

define("TOKEN", "YOUR_TOKEN");
$wechatObj = new wechatCallbackapiTest();
$wechatObj->valid();

class wechatCallbackapiTest
{
	public function valid()
    {
        $echoStr = $_GET["echostr"];

        //valid signature , option
        if($this->checkSignature()){
        	echo $echoStr;
        	exit;
        }
    }
		
	private function checkSignature()
	{
        $signature = $_GET["signature"];
        $timestamp = $_GET["timestamp"];
        $nonce = $_GET["nonce"];	
        		
		$token = TOKEN;
		$tmpArr = array($token, $timestamp, $nonce);
		sort($tmpArr);
		$tmpStr = implode( $tmpArr );
		$tmpStr = sha1( $tmpStr );
		
		if( $tmpStr == $signature ){
			return true;
		}else{
			return false;
		}
	}
}

验证通过之后即可进行开发模式的编写，但是目前订阅号还不支持自定义菜单，微信提供了测试帐号供开发者使用，填写手机号申请成功后开始在测试帐号上进行开发。

作为一个PHP菜鸟，我直接上[Github](http://github.com/)找了几个Demo放到SAE上。感谢所有开源者，测试结果如下...

![wechatrobot](https://dl.dropboxusercontent.com/u/36470533/Photos/wechatrobot.jpg)

另外，微信提供的接口毕竟有限，但是以链接的形式在微信中直接打开HTML5网页却是极好的。可以说微信开发的门槛较低，但是目前还没有看到成功的杀手级公众号出现。

想想江湖上对微信团队的流言蜚语，感慨唏嘘的同时也只能说关我屁事。