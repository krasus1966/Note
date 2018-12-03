# Mybatis笔记

## Mybatis介绍

Mybatis是一款优秀的持久层框架，代替JDBC执行数据库操作，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集，用XML或注解来配置和映射原生信息，将接口和JAVA的普通JAVA对象映射成数据库中的记录。

## Mybatis构成

Mybatis由以下几个部分构成：

Configuration.xml:Mybatis的主配置文件，用来配置JDBC的各项参数（JDBC驱动，连接数据库，数据库用户名及密码）以及配置连接数据库操作的配置文件。

格式：

```XML
<configuration>
  <environments default="development">
    <environment id="development">
       <!--数据库连接方式-->
      <transactionManager type="JDBC">
        <property name="" value=""/>
      </transactionManager>
      <dataSource type="UNPOOLED">
          <!--配置驱动-->
        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
          <!--连接数据库的URl-->
        <property name="url" value="jdbc:mysql://127.0.0.1:3306/micro_message?serverTimezone=GMT%2B8"/>
          <!--数据库用户名-->
        <property name="username" value="root"/>
          <!--密码-->
        <property name="password" value="196610121" />
      </dataSource>
    </environment>
  </environments>

  <mappers>
      <!--连接数据库操作的配置文件-->
    <mapper resource="cn/cx/WeChat/config/Sql/Message.xml"/>
  </mappers>
</configuration>
```

SQL.xml:

名称不固定，可以是对应表格的名字，格式：

```xml
<!--命名空间 namespace必须存在否则报错，每种namespace下的id唯一，不同下的不唯一-->
<mapper namespace="Message">
    <!--type为类名，id为resultMap的名字-->
    <resultMap type="cn.cx.WeChat.Bean.Message" id="MessageResult">
        <!--标签id为主键-->
        <!--column为列名-->
        <!--jdbcType为数据类型-->
        <!--property为属性-->
        <id column="ID" jdbcType="INTEGER" property="id"/>
        <!--result为主键以外的数据-->
        <result column="COMMAND" jdbcType="VARCHAR" property="command"/>
        <result column="DESCRIPTION" jdbcType="VARCHAR" property="description"/>
        <result column="CONTENT" jdbcType="VARCHAR" property="content"/>
    </resultMap>
    
	<!--标签select为查询语句，id为语句的名字，parameterType是数据类型，resultMap代表归属-->
    <select id="queryMessageList" parameterType="cn.cx.WeChat.Bean.Message" resultMap="MessageResult">
        SELECT ID,COMMAND,DESCRIPTION,CONTENT from message
        <where>
            <!--动态拼接SQL语句，用OGNL表达式-->
            <if test="command != null and !&quot;&quot;.equals(command.trim())">
                and COMMAND=#{command}
            </if>
            <if test="description != null and !&quot;&quot;.equals(description.trim())">
                and DESCRIPTION like '%' #{description} '%'
            </if>
        </where>
    </select>
    
	<!--删除语句标签-->
    <delete id="deleteOne"  parameterType="int">
        delete from message where id = #{parameter}
    </delete>
    
    <!--插入语句标签-->
     <insert id=""></insert>
    
    <!--更新语句标签-->
    <update id=""></update>
</mapper>
```

Bean：封装属性

DB:调用配置文件

```java
public class DBAccess {
public SqlSession getSqlSession() throws IOException {
        //通过配置文件获取数据库连接信息
        Reader reader = Resources.getResourceAsReader("cn/cx/WeChat/config/Configuration.xml");
        //通过配置信息构建一个SqlSessionFactory
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
        //通过SqlSessionFactory打开一个数据库会话
        SqlSession sqlSession = sqlSessionFactory.openSession();
        return sqlSession;
    }
}
```



Dao：调用配置文件与数据库进行交互，格式：

```java
DBAccess dbAccess = new DBAccess();//实例化DB
SqlSession sqlSession = null;//另连接为空，在finally中关闭
    try {
        sqlSession = dbAccess.getSqlSession();//获取连接信息
        //通过调用SQLSession中的方法来调用SQL语句
    }catch (IOException e) {
            e.printStackTrace();
    } finally {
      	if (sqlSession != null) {
            sqlSession.close();//关闭连接
         }
    }
```

Servlet：从页面获取数据，将数据传给Service，语句处理完成后跳转页面。

Service：获取Servlet中传递的数据进行处理，处理后将值传给Dao，由Dao执行数据库语句。



Servlet 从页面获取值 调用Service

Service 接收Servlet的值并处理值 调用Dao

Dao 执行SQL语句

*Tips:*导入很多的jar包.

## 用log4j查看信息（控制台输出）

导入log4j.jar(注：不是log4j-core)，写入配置文件log4.properties:

```properties
log4j.rootLogger=DEBUG,Console
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.Console.layout=org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n
log4j.logger.org.apache=INFO
```

然后启动TomCat，在执行调用SQL语句的操作时，就会在控制台看到信息，用于查看错误信息。