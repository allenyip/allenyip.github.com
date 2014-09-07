---

title: JavaEE 相关

layout: post

---

* [Servlet](#serv)
* [JSP](#jsp)
* [JDBC](#jdbc)
* [Struts](#stru)
* [Sprint](#spri)
* [Hibernate](#hibe)

<h2 id="serv">Servlet</h2>

Servlet是运行在Web服务器或应用服务器上的Java程序，是Java Web技术的核心基础，它是连接Web浏览器或其他HTTP客户程序和HTTP服务器上的数据库或应用程序的中间层。主要任务包括：

1. 读取客户发送的HTTP请求

2. 生成结果，可能需要访问数据库/执行RMI/调用Web服务等

3. 向客户发送HTTP响应

Servlet与Applet比较：

* 都不是独立的应用程序，没有main()方法

* 不是由用户调用，而是由另一个应用程序（容器）调用

* 都有一个生命周期，包含init()/destroy()等方法

* 区别是Applet有图形界面AWT，与浏览器一起运行在客户端；而Servlet没有图形界面，运行在服务端

Servlet与CGI比较：

* Servlet具有更高的侠侣，更易使用，更加强大，具有更好的可移植性

* 传统的CGI对每个请求都启动一个新进程，而Servlet把每个请求交给一个对应的线程处理

* 传统的CGI对N个并发请求会在内存中重复装载N次代码，而Servlet只需要一份代码

Servlet与JSP比较：

* Servlet是纯Java代码（尽管可以用它输出HTML代码），保存在.java文件里，而JSP是HTML混合Java代码，保存在.jsp文件里

* JSP的本质也是Servlet，但更注重展现，通常使用Servelt控制业务流程，而用JSP编写动态网页

* 实际项目中总是将JSP和Servlet结合使用

Servlet没有main()方法，它们受控于一个称为容器的Java应用，如Tomcat。当Web服务器得到一个指向servlet请求时（若是静态资源请求则由Web服务器本身自己处理），会将这个请求交给servlet容器，由容器管理servlet的生命周期。

容器的作用包括：通信支持、生命周期管理、多线程支持、JSP支持、声明方式实现安全。处理请求的过程如下：

1. 用户点击一个链接，指向一个Servlet而不是一个静态页面

2. 容器判断出请求指向Servlet，创建出两个对象HttpServletRequest/HttpServletResponse

3. 容器根据请求URL找到正确的Servlet（如何找到？），为请求创建或分配一个线程，并把HttpServletRequest/HttpServletResponse两个对象传递给这个Servlet线程

4. 容器调用Servlet的service()方法，根据请求的不同类型（GET/POST），service()方法会调用doGet/doPost方法

5. doGet/doPost方法生成动态页面，并将其放到HttpServletResponse对象里

6. 容器把HttpServletResponse对象转换为HTTP响应，发回给客户后，删除HttpServletRequest/HttpServletResponse对象，线程或者撤销或者返回到容器管理的一个线程池

通常容器会使用部署文件把URL映射到Servlet

```java
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		resp.setContentType("text/html");
		PrintWriter out = resp.getWriter();
		String docType = "<!DOCTYPE html>";
		out.println(docType + "<html>\n"
				+ "<head><title>hello</title></head>\n" + "<body>hello</body>");
	}

}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
  <!-- servlet元素指定有哪些servlet类文件 -->
  <servlet>
  	<!-- 这里是自定义的名字，用于绑定servlet和servlet-mapping -->
    <servlet-name>FirstServlet</servlet-name>
    <!-- 这里是类的全名（包名加类名） -->
    <servlet-class>com.allenyip.test.HelloServlet</servlet-class>
  </servlet>

  <!-- URL和Servlet的映射 -->
  <servlet-mapping>
    <servlet-name>FirstServlet</servlet-name>
    <!-- 客户看到的Servlet不是真正的Servlet名 -->
    <url-pattern>/OtherServlet</url-pattern>
  </servlet-mapping>
</web-app>
```

Servlet生命周期是由容器控制的，分为加载和实例化阶段、初始化阶段、运行阶段和销毁阶段。

加载和实例化阶段：Servlet容器启动时会创建Servlet实例，

初始化阶段：

1. Servlet容器加载Servlet类，读取它的.class文件数据到内存中

2. Servlet容器创建servletConfig对象（包含servlet初始化配置信息），将其与当前web应用的servletContext对象关联

3. Servlet容器创建Servlet对象

4. Servlet容器调用Servlet对象的init(servletConfig)方法

运行阶段：

1. Servlet容器收到特定的Servlet请求时，创建针对这个请求的servletRequest和servletResponse对象，然后调用service(servletRequest, servletResponse)方法，由该方法调用doGet/doPost

2. 当Servlet容器把响应结果发给客户后，Servlet容器会销毁servletRequest和servletResponse对象

销毁阶段：Servlet容器检测到一个Servlet实例应该被移除时，rvlet容器先调用其destroy()方法，然后销毁Servlet对象。

在一个Servlet的整个生命周期中，加载和实例化、初始化、销毁都只执行一。

<h2 id="jsp">JSP</h2>

基本语法

* HTML文本：与原生HTML一样，不加更改的发送给客户

* HTML注释：与原生HTML一样，不加更改的发送给客户

* 模板文本：不加更改的发送给客户的文本（如HTML文本和HTML注释）

* JSP注释：开发人员注释，不发送给客户 <%-- Blah --%>

* JSP表达式：每次请求页面时都计算值并发送给客户 <%= Java Value %>

* JSP Scriptlet：每次请求页面时执行的一个或多个语句 <% Java Statement %>

* JSP声明：JSP转换成Servlet时称为类定义的一部分 <%! Field Definition %>

* JSP指令：Servlet代码的高层结构信息 <%@ xxx %>

* JSP动作：页面被请求时采取的动作 <jsp:xxx>...</jsp:xxx>

* JSP表达式语言：简写的JSP表达式 ${EL Expression}

<h2 id="jdbc">JDBC</h2>

JDBC/Java Database Connectivity是一组专门负责连接并操作数据库的Java API，可以为多种关系数据库提供统一访问。

操作步骤包括：

1. 加载数据库驱动程序，加载的时候需要将驱动程序配置到classpath之中

2. 连接数据库，通过Connection接口和DriverManager类完成

3. 操作数据库，通过Statement、PreparedStatement、ResultSet三个接口完成

4. 关闭数据库，在实际开发中数据库资源非常有限，操作完之后必须关闭

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

public class Demo {

	// 驱动程序就是之前在classpath中配置的JDBC的驱动程序的JAR 包中
	public static final String DBDRIVER = "com.mysql.jdbc.Driver";
	// 连接地址是由各个数据库生产商单独提供的，所以需要单独记住
	public static final String DBURL = "jdbc:mysql://localhost:3306/test";
	// 连接数据库的用户名
	public static final String DBUSER = "root";
	// 连接数据库的密码
	public static final String DBPASS = "";

	public static void main(String[] args) throws Exception {
		// 表示数据库的连接对象
		Connection con = null;
		Statement stmt = null;
		// 1、使用CLASS 类加载驱动程序
		Class.forName(DBDRIVER);
		// 2、连接数据库
		con = DriverManager.getConnection(DBURL,DBUSER,DBPASS);
		// 3、Statement 接口需要通过Connection 接口进行实例化操作
		stmt = con.createStatement();
		// 执行SQL 语句，插入、更新、删除数据
		stmt.executeUpdate("insert into java_study.person values(\'Tom\',20,\'SH\')");
		stmt.executeUpdate("update java_study.person set name='Jery' where age = 20");
		stmt.executeUpdate("delete from java_study.person where age = 20");
		// 4、关闭数据库
		con.close();
	}
}
```

<h2 id="stru">Struts</h2>

Struts是一个免费开源的用于企业级Java Web应用的MVC框架，Struts2是Struts1与WebWork结合的产品。

1，通过以下步骤开始一个Struts Demo：

1. 在Apache Struts官网下载最新的Struts.zip

2. 新建一个动态Web项目，将Struts.zip中lib目录下的文件拷贝到项目/WebContent/WEB-INF/lib文件夹下，至少包含commons-fileupload/commons-io/commons-lang/commons-logging/freemaker/javassist/ognl/struts2-core/xwork-core这几个jar包

3. 在src文件夹下创建一个模型（Model）类MessageStore.java

```java
package com.allenyip.helloworld.model;

public class MessageStore {
	private String message;
	public MessageStore() {
		setMessage("Hello Struts User");
	}
	public String getMessage() {
		return message;
	}
	public void setMessage(String message) {
		this.message = message;
	}
}
```

4. 在src文件夹下建包，新建一个控制器（Controller）类HelloAction.java

```java
package com.allenyip.helloworld.action;
import com.allenyip.helloworld.model.MessageStore;
import com.opensymphony.xwork2.ActionSupport;

public class HelloAction extends ActionSupport {
	private static final long serialVersionUID = 1L;
	private MessageStore messageStore;
	public String execute() throws Exception {
		messageStore = new MessageStore() ;
		return SUCCESS;
	}
	public MessageStore getMessageStore() {
		return messageStore;
	}
	public void setMessageStore(MessageStore messageStore) {
		this.messageStore = messageStore;
	}
}
```

5. 在WebContent文件夹下创建模型（View）文件HelloWorld.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Hello World!</title>
</head>
<body>
    <h2><s:property value="messageStore.message" /></h2>
</body>
</html>
```

6. 修改src文件夹下的配置文件struts.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">
<struts>
	<constant name="struts.devMode" value="true" />
	<package name="basicstruts2" extends="struts-default">
		<action name="index">
			<result>/index.jsp</result>
		</action>
		<action name="hello"
			class="com.allenyip.action.HelloAction" method="execute">
			<result name="success">/HelloWorld.jsp</result>
		</action>
	</package>
</struts>
```

7. 修改WEB-INF文件夹下的web.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
	http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
	<!-- Struts2 -->
	<filter>
        <filter-name>struts2</filter-name>
        <filter-class>
            org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter
        </filter-class>
    </filter>
    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
	<!-- Struts2 ends -->
	<welcome-file-list>
		<welcome-file>index.jsp</welcome-file>
	</welcome-file-list>
</web-app>
```

2，Struts2遵循Servlet标准，通过实现标准的Filter接口进行Http请求的处理，通过在web.xml指定这个实现类StrutsPrepareAndExecuteFilter，就可以将Struts2引入到应用中来。通过Filter生命周期将Struts2划分为初始化和处理HTTP请求两个主线。

1. Struts2初始化：[TODO](http://blog.csdn.net/shan9liang/article/details/9281967)

2. Struts2处理HTTP请求：可分为Http请求预处理阶段和XWork执行业务逻辑阶段。Http请求预处理阶段的主要元素有Dispathcer（核心分发器）、PrepareOperations（HTTP预处理类）和ExecuteOperations（HTTP处理执行类）；XWork执行业务逻辑阶段的主要元素有XWorkd框架的七大元素构成：

> ActionProxy，整个生产线的入口，封装了所有执行细节。
>
ActionInvocation，生产线的调度者，负责调度整个生产线中各个元素的执行次序。

> Interceptor，生产线上的工序，丰富生产线的功能。

> Action，生产线上的核心工序，负责核心业务逻辑的调用或实现。

> ActionContext，提供整个生产线需要的数据环境。

> ValueStack，提供表达式计算的工具类，Xwork数据访问的基础。（后面细说）

> Result，生产线末端设备，输出生产线的生产结果。

3，当浏览器给服务器发送一个URL请求（如http://localhost:8080/hellostruts/hello.action），Struts2的工作流程如下：

1. 容器收到Web服务器请求hello.action，根据从web.xml配置文件中载入的数据，Servlet容器发现所有的请求都被定向到org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter，StrutsPrepareAndExecuteFilter是Struts2框架的入口

2. 框架（中的Servlet Filter Dispatcher）在配置文件中寻找名为hello的action，发现它跟com.allenyip.action.HelloAction关联，（通过Interceptor验证后），实例化HelloAction并且调用它的execute方法

3. execute方法创建MessageStore对象并且返回SUCCESS，框架再次检查配置文件返回SUCCESS应该载入那个文件，于是定向到HelloWorld.jsp

4. 处理HelloWorld.jsp的过程中，<s:property value="messageStore.message" />标签会调用HelloAction的getter得到messageStore，后调用MessageStore的getMessage得到message

5. 将HTML响应发回给客户

4，Strut2常用标签：

使用<%@ taglib prefix="s" uri="/struts-tags" %>语句引入标签库，可将标签分为UI类和非UI类。

1. UI类，可包括表单类和非表单类。

```jsp
<!-- form:  -->
<s:form action="exampleSubmit" method="post" enctype="multipart/form-data">
<s:submit />
<s:reset />
</s:form>
<!-- textfield：-->
<s:textfield
            label="姓名："
            name="name"
            tooltip="Enter your Name here" />
<!-- datepicker：-->
<s:datepicker
            tooltip="Select Your Birthday"
            label="生日"
            name="birthday" />
<!-- textarea：-->
<s:textarea
            tooltip="Enter your remart"
            label="备注"
            name="remart"
            cols="20"
            rows="3"/>
<!-- select: -->
<s:select
            tooltip="Choose user_type"
            label=""
            list="#{'free':'免费','vip':'收费'}" value="#{'free':'免费'}"
           name="bean.user_type"
            emptyOption="true"
            headerKey="None"
            headerValue="None"/>
<s:select
            tooltip="Choose user_type"
            label=""
            list="#{'free':'免费','vip':'收费'}" value="#{'free':'免费'}"
           name="bean.user_type"
            emptyOption="true"
            headerKey="None"
            headerValue="None"/>
<s:select
list="venderList"
listKey="id"
listValue="name"
value="%{profile.companyName}"
name="companyName" cssClass="sel_style_w_180"/>
<!-- checkboxlist： -->
<s:checkboxlist
            tooltip="Choose your Friends"
            label="朋友"
            list="{'Patrick', 'Jason', 'Jay', 'Toby', 'Rene'}"
            name="friends"/>
<!-- checkbox： -->
   <s:checkbox
            tooltip="Confirmed that your are Over 18"
            label="年龄"
            name="legalAge"
            value="18"/>
<!-- file -->
   <s:file
            tooltip="Upload Your Picture"
            label="Picture"
            name="picture" />
<!-- a -->
<s:a href="getP.jsp">超链接提交</s:a>
<!-- date -->
<s:date name="ad_end_time" format="yyyy-MM-dd"/>
```

2. 非UI类

```jsp
<!-- if/elseif/else执行基本的条件流转 -->
<s:set name="age" value="61"/>
<s:if test="${age > 60}">
    老年人
</s:if>
<s:elseif test="${age > 35}">
    中年人
</s:elseif>
<s:else>
    少年
</s:else>
<!-- iterator用于遍历集合（Collection）或枚举值(iterator) -->
<%
    List list = new ArrayList();
    list.add("Max");
    list.add("Joe");
    list.add("Kelvin");
    request.setAttribute("names", list);
%>
<s:iterator value="#request.names" status="stuts">
	<s:if test="#stuts.odd == true">
		<li>White <s:property /></li>
	</s:if>
	<s:else>
		<li style="background-color: gray"><s:property /></li>
	</s:else>
</s:iterator>
```

<h2 id="spri">Spring</h2>

Spring框架为企业级Java应用提供了一个全面的编程和配置模型，是为了解决企业应用程序开发复杂性的轻量级的控制反转和面向切面的容器开源框架。

1，通过以下步骤使用Spring：

1. 从[官网](http://repo.spring.io/release/org/springframework/spring/)下载spring-framework.zip

2. 新建动态Web项目，把spring-framework.zip包中lib文件夹下的文件复制到项目WebContent/WEB-INF/lib文件夹下，至少包含
spring-aop/spring-beans/spring-context/spring-core/cpring-expression这几个包，另外加入commons-logging包

3. 在src目录下新建如下文件，演示了依赖注入的概念，MessagePrinter从MessageService实现中解耦合：

```java
package hello;

public interface MessageService {
    String getMessage();
}

package hello;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class MessagePrinter {
    final private MessageService service;
    @Autowired
    public MessagePrinter(MessageService service) {
        this.service = service;
    }
    public void printMessage() {
        System.out.println(this.service.getMessage());
    }
}

package hello;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.*;

@Configuration
@ComponentScan
public class Application {
    @Bean
    MessageService mockMessageService() {
        return new MessageService() {
            public String getMessage() {
              return "Hello World!";
            }
        };
    }
  public static void main(String[] args) {
      ApplicationContext context =
          new AnnotationConfigApplicationContext(Application.class);
      MessagePrinter printer = context.getBean(MessagePrinter.class);
      printer.printMessage();
  }
}
```

<h2 id="hibe">Hibernate</h2>

Hibernate是一个开源的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。

1，通过以下步骤开始一个Hibernate Demo：

1. 从[官网](http://hibernate.org/)下载hibernate-release.zip

2. 新建动态Web项目，把hibernate-release.zip包中lib/required文件夹下的文件复制到项目WebContent/WEB-INF/lib文件夹下，另外还要添加log4j/slf4j-log4j12/mysql-connector-java等jar包

3. 在数据库中新建一个数据表USER，包含ID/USERNAME/PASSWORD字段

4. 在src文件夹下新建一个POJO类User.java和一个Hibernate配置文件User.hbm.xml：

```java
package com.allenyip.test.pojo;

public class User {
	private Long id;
	private String username;
	private String password;

	public Long getId() {
		return id;
	}

	public void setId(Long id) {
		this.id = id;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC
        '-//Hibernate/Hibernate Mapping DTD 3.0//EN'
        'http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd'>
<hibernate-mapping>
	<class name="com.allenyip.test.pojo.User" table="USER">
		<id name="id" column="ID">
			<generator class="increment"/>
		</id>
		<property name="username" column="USERNAME"></property>
		<property name="password" column="PASSWORD"></property>
	</class>
</hibernate-mapping>
```

5. 修改src文件夹下的配置文件hibernate.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<session-factory>
		<!-- 为true表示将Hibernate发送给数据库的sql显示出来 -->
		<property name="show_sql">true</property>
		<!-- SQL方言，这边设定的是MySQL -->
		<property name="dialect">net.sf.hibernate.dialect.MySQLDialect</property>
		<!-- 一次读的数据库记录数 -->
		<property name="jdbc.fetch_size">50</property>
		<!-- 设定对数据库进行批量删除 -->
		<property name="jdbc.batch_size">30</property>
		<!--驱动程序 -->
		<property name="connection.driver_class">com.mysql.jdbc.Driver</property>
		<!-- JDBC URL -->
		<property name="connection.url">jdbc:mysql:
			//localhost/test?characterEncoding=gb2312</property>
		<!-- 数据库用户名 -->
		<property name="connection.username">root</property>
		<!-- 数据库密码 -->
		<property name="connection.password">root</property>

		<!-- 映射文件 -->
		<mapping resource="com/allenyip/test/pojo/User.hbm.xml" />
	</session-factory>
</hibernate-configuration>
```

6. 新建一个测试类来增加数据

```java
package com.allenyip.test.pojo;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

public class Test {
	public static void main(String[] args) {
		SessionFactory sf = new Configuration().configure()
				.buildSessionFactory();
		Session s = null;
		Transaction t = null;

		try {
			// 准备数据
			User user = new User();
			user.setUsername("test");
			user.setPassword("123");
			s = sf.openSession();
			t = s.beginTransaction();
			s.save(user);
			t.commit();
		} catch (Exception err) {
			t.rollback();
			err.printStackTrace();
		} finally {
			s.close();
		}
	}
}
```
