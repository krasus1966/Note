# Maven项目创建（IDEA）

##### IDE创建Maven Web项目问题

1. 创建web项目，要在main目录中新建Web目录（*WebRoot*），Web目录下新建WEB-INF文件夹，将web.xml放入WEB-INF完成创建Web目录操作，然后修改pom.xml，增加对JDK1.8的支持，代码如下：

```java
<build>
    <plugins>
        <plugin>
            <groupId>cn.cx.maven.helloworld</groupId>
            <artifactId>helloworld</artifactId>
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

2. 创建一个servlet

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

3. 添加maven依赖

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





