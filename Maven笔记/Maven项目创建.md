# Maven项目创建（IDEA）

##### IDE创建Maven Web项目问题

1. 创建web项目，要在main目录中新建Web目录（*WebRoot*），Web目录下新建WEB-INF文件夹，将web.xml放入WEB-INF完成创建Web目录操作，然后修改pom.xml，增加对JDK1.8的支持，代码如下：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <configuration>
            	<!--源码版本-->
                <source>1.8</source>
                <!-- 编译目标版本-->
                <target>1.8</target>
                <!-- 指定编码-->
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
```

2. 创建一个Servlet并配置Servlet：

```java
package cn.cx.maven.servlet;

public class ServletTest extends HttpServlet{
    @Override
    protected void doGet(HttpServletRequest request,HttpServletresponse response)throws ServletException, IOException {
        this,doPost(request,response);
    }

    @Override
    protected void doPost(HttpServletRequest request,HttpServletresponse response)throws ServletException,IOException{
        request.getRequestDispatcher("/jsp/test.jsp").forward(request,response);
    }
}
```

3. 添加maven依赖：

```xml
<dependencies>
  <dependency>
     <groupId>javax.servlet</groupId>
     <artifactId>servlet-api</artifactId>
     <version>3.0.1</version>
     <scope>provided</scope>
  </dependency>

  <dependency>
     <groupId>javax.servlet.jsp</groupId>
     <artifactId>jsp-api</artifactId>
     <version>2.0</version>
     <scope>provided</scope>
  </dependency>
</dependencies>
```

4. 添加maven插件

```xml
<build>
        <plugins>
        	<!--上面还有一个添加JDK1.8的支持插件-->
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                	<!--部署端口-->
                    <port>9090</port>
                    <path>/helloworld</path>
                    <uriEncoding>utf-8</uriEncoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

5. Edit Configuration，添加Maven，在Command line输入启动指令**tomcat7:run**

   注：如果用**tomcat:run**启动，会调用默认TOMCAT6启动，不支持JDK1.8，报错

   ```java
   org.apache.jasper.JasperException: Unable to compile class for JSP:
   An error occurred at line: 1 in the generated java file
   The type java.io.ObjectInputStream cannot be resolved. It is indirectly referenced from required .class files
   ```

6. 启动成功后，在浏览器地址栏输入*localhost:9090/helloworld/*即可打开部署后的Web项目。

*TIPS：*IDEA右侧的*Maven Project*栏，可以看到插件信息，依赖信息，在*Lifecycle*中可以执行*Maven*的各个命令。

IDEA快速添加依赖的快捷键：*Alt+insert*

# Maven整合Struts2

1. 创建maven项目，选择archetype-webapp，构建maven目录。
2. 在resources目录上点右键 **New->XML Configuration File->Struts Config**创建Struts2配置文件struts.xml：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
        "http://struts.apache.org/dtds/struts-2.5.dtd">
<struts>
    <package name="default" extends="struts-default">
        <action name="HelloAction" class="cn.cx.web.action.HelloAction">
            <result>index.jsp</result>
        </action>
    </package>
</struts>
```

3. 在pom.xml添加struts2依赖

```xml
<dependency>
	<groupId>junit</groupId>
	<artifactId>junit</artifactId>
	<version>4.11</version>
		<scope>test</scope>
</dependency>

<dependency>
	<groupId>org.apache.struts</groupId>
	<artifactId>struts2-core</artifactId>
	<version>2.5.18</version>
</dependency>
```

4. 添加struts2的action目录，创建HelloAction类

```java
package cn.cx.web.action;
import com.opensymphony.xwork2.ActionSupport;
public class HelloAction extends ActionSupport {
    @Override
    public String execute()throws Exception{
        System.out.println("进入默认的HelloAction...返回默认的jsp页面");
        return SUCCESS;
    }
}
```

5. 在web.xml中添加struts2过滤器

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

