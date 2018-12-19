# Spring

docs ：Spring的开发规范和API

libs：Spring的开发的jar和源码

schema：Spring的配置文件的约束

#### 创建Web项目

导入jar包：

**spring-beans**

 **spring-context**  

**spring-core** 

**spring-expression**

**apache.log4j**

**apache.commons.logging**

## Spring框架

- Spring：SE/EE开发的一站式框架。
  - 一站式框架：有EE开发的每一层解决方案。
    - WEB层：SpringMVC
    - Service层：Spring的Bean管理，Spring声明式事务
    - Dao层：Spring的jdbc模板，Spring的ORM模块

### Spring入门

- IOC：控制反转

  - 控制反转：将对象的创建权反转给Spring

- 下载Spring开发包 

  官网：http://spring.io/

#### 创建Web项目

*DataAccess/Integration*：JDBC、ORM、OXM、JMS、Transactions

*Web*：WebSocket、Servlet、Web、Portlet

*AOP*	-	*Aspects*	-	*Instrumentation*	-	*Messaging*

*Core Container*：Beans、Core、Context、SpELl

——————————Test————————

- 创建接口和类

  实现原理：

  ```xml
  <bean id="UserDao" class="xxx.UserDaoImpl"></bean>
  class BeanFactory{
      public static Object getBean(String id){
          //解析XML
          //反射
          Class class = Class.forName(class="xxx.UserDaoImpl");
          return class.newInstance()
      }
  }
  ```

- 将接口和实现类给Spring管理

  在

