# Struts2笔记

Struts2本质是JAVA WEB的拦截器，拦截所有的action，然后经过处理，跳转到对应页面。

##### Struts.xml:用来配置Action的文件

```xml
<struts>
    <!-- include 节点是struts2中组件化的方式，可以将每个功能模块独立到一个xml配置文件中，然后用include节点引用-->
    
    <include file="struts-default.xml"></include>
    
    <!-- package 提供了将多个Action 组织为一个模块的方式
            pakcage的名字必须是唯一的，pakcage可以扩展，当一个package扩展自另一个pakcage时该package会在本身配置上加入扩展的pakcage的配置
            父pakcage必须在子pakcage前配置
                name：pakcage 名称
                extends：继承的父pakcage名称
                abstract：设置pakcage的属性为抽象的 抽象的pakcage不能定义Action 值true：false
                namespace：定义pakcage命名空间 该命名空间影响到URL的地址，例如此命名空间为/test那么访问地址为http://localhost:8080/Struts2Project/test/XX.action
                -->
    <package name="default" namespace="/" extends="struts-default">

    <!--
    <interceptors>
             定义拦截器
                 name：拦截器名称
                 class：拦截器类路径

        <interceptor name="timer" class="cn.cx.action.timer"></interceptor>
         定义拦截器栈
        <interceptor-stack name="mystack">
            <interceptor-ref name="timer"></interceptor-ref>
        </interceptor-stack>
    </interceptors>
    -->
        <!--
            定义默认的拦截器 每个Action都会自动引用
            如果Action中引用了其他的拦截器 默认的拦截器将无效
        -->

        <!--
            全局result配置
        -->
        <global-results>
            <result>helloworld.action</result>
        </global-results>

        <!--
            Action配置 一个Action可以被多次映射(只要Action配置中的name不同)
            name：action名称
            class：对应的类的路径
            method：调用Action中的方法名
        -->
        <action name="helloworld" class="cn.cx.action.HelloWorld">

            <!--
                引用拦截器
                name：拦截器名称或拦截器栈名称
                <interceptor-ref name="timer"></interceptor-ref>
            -->

            <!--
                节点配置
                name：result名称 和Action中返回值相同
                type：result类型 不写则选用superpackage的type struts-default.xml中的默认为dis
            -->
            <result>/result.jsp</result>
            <!--
              参数设置
              name：对应Action中的get/set方法
              <param name="url'>http://www.sina.com</param>
            -->
        </action>
    </package>
</struts>
```

##### Struts.properties:Struts的参数文件

```properties
#指定默认编码集，对于请求参数带有中文的情况应该设置GBK或GB2312，默认UTF-8
struts.i18n.encoding=GB2312
#是否每次HTTP请求到达时，都重新加载国际化资源文件，默认值false
struts.i18n.reload=true
#当struts.xml改动后，是否重新加载该文件，在开发阶段建议将此属性设置为true，提高开发效率，默认值false
struts.configuration.xml.reload=true
#是否使用Struts2的开发模式，可以获得更多报错信息，便于调试，在开发阶段设置为true，默认值false
struts.devMode = true
#设置浏览器是否缓存静态页面，开发阶段设置为false，以获得服务器的最新响应，默认值true
struts.serve.static.browserCache=true
#指定后缀为.action形式的请求可悲Struts2处理，可配置多个请求后缀，比如.do、.struts2等，配置时多个后缀名用逗号隔开
struts.action.extension=action,do,struts2,
#配置服务器运行时的端口号，一般情况下该属性不修改，如果端口号占用则重新分配端口号，默认值80
struts.url.http.port = 8080
```

##### web.xml:要在这个文件中添加Struts2的过滤器

```xml
<filter>
        <filter-name>struts2</filter-name>
        <filter-class>org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

##### HelloAction.java:

```java
import com.opensymphony.xwork2.ActionSupport;
public class HelloWorld extends ActionSupport{

    @Override
    public String execute() throws Exception{
        System.out.println("执行Action");
        return SUCCESS;
    }
}
```



### 通配符方式

通配符方式调用Action中的方法时，课程中代码为：

```xml
<action name="HelloAction_*" method="{1}" class="cn.cx.Users.Action.HelloAction">
   <result>index.jsp</result>
   <result name="Login">/{1}.jsp</result>
</action>
```

struts-core 2.3以下的版本不会有问题，可以使用，但是2.3以上的版本，使用通配符时，内部会验证是否允许访问此方法，因此要添加一行代码，即：

```xml
<action name="HelloAction_*" method="{1}" class="cn.cx.Users.Action.HelloAction">
   <result>index.jsp</result>
   <result name="Login">/{1}.jsp</result>
   <allowed-methods>[方法名1],[方法名2]</allowed-methods>
</action>
```

### Struts2的参数传递

页面form表单的action中写调用的action

1. 使用action属性接收

```java
LoginAction.java:
private String username;
private String password;
    public String Login() {
        System.out.println(username);
        return SUCCESS;
    }
//省略username 和 password 的get和set方法
```

2. 使用DomainModel接收参数

```java
private User user;
    public String Login() {
        System.out.println(user.getUsername());
        return SUCCESS;
    }
//省略User的get和set方法
页面：
<input name="user.username">
```

3. 使用ModelDriven接收参数

```java
public class LoginAction extends ActionSupport implements ModelDriven<User> {
    private User user=new User();
    public String Login() {
        System.out.println(user.getUsername());
        return SUCCESS;
    }
    @Override
    public String execute() throws Exception{
        System.out.println("执行Action");
        return  SUCCESS;
    }
    @Override
    public User getModel() {
        return user;
    }
}
```

### 配置文件问题

配置文件中，<result name="success">即对应return SUCCESS，即Struts2中默认的SUCCESS、INPUT、ERROR，在配置文件中的name均为小写，否则报错。

使用Maven整合Struts2和Mybatis，所有的配置文件都要放在/resources下，其为根目录，可以创建文件夹进行分类。

### 标签库问题

Struts2有自己的标签库

```xml
<%@taglib prefix="s" uri="/struts-tags"%>
    <s:fielderror> 字段错误信息
    <s:actionerror> action错误信息
```

其他的参考：

>https://www.cnblogs.com/yw-ah/p/5760540.html

### 数据校验

参考：

> http://www.cnblogs.com/cenyu/p/6235794.html

### 整合Mybatis问题

由于Struts2中可以用其他方式传递参数，因此可以省略Servlet和Service（当然也可以用），其他（Dao，DB，Bean）则相同，直接用Action进行传参。