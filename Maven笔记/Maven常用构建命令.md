# Maven常用构建命令

**compile** 编译
**test** 测试
**-v** 查看maven版本信息
**mvn clean** 删除maven生成的文件
**install**  首先进入项目根目录，输入mvn install 可以将项目发布到本地仓库中

---

# Maven目录结构

```
    src
        -main
            -java
                -pakcage
        -test
            -java
                -pakcage
        resources
```
***
# 第一个Maven项目
先创建Maven目录结构和文件
```
code/maven01/src/main/java/cn/cx/maven01/model/HelloWorld.java
code/maven01/src/test/java/cn/cx/maven01/model/HelloWorld.txt

```
*HelloWorld.java*:
```java
    package cn.cx.maven01.model;

    public class HelloWorld{
        public String sayHello(){
            return "Hello World!";
        }
    }
```
*HelloWorldTest.java*:
```java
    package cn.cx.maven01.model;

    import org.junit.*;
    import org.junit.Assert.*;
    
    public class HelloWorldTest{
        @Test
        public void testHello(){
            Assert.assertEquals("Hello World!",new HelloWorld().sayHello());
        }
    }
```
打开终端，进入项目目录，
输入**mvn compile**开始构建（第一次构建会下载jar包）。
构建完成出现信息 **Build SUCCESS** 即编译成功。
接着输入mvn test执行test用例，出现**TESTS**和**BUILD SUCCESS**即成功运行。
编译成功后，项目根目录生成**target**文件夹，**classes**文件夹下为编译生成的字节码，**surefire-reports**文件夹下为测试报告。
接着在终端输入**mvn package**,会将项目打包成jar文件。

---

# Maven自动建立目录骨架

#### archetype插件

1. 进入项目目录，输入**archetype:generate**，回车。

- 如果出现

> **[INFO] Generating project in Interactive mode**

并卡住，要在命令后面加上

> **-DarchetypeCatalog=internal**  

让它不要从远程服务器上取catalog。

- 出现 

  >  Generating project in Batch mode

  这个问题是在命令后面加了

  > -DinteractiveMode=false

  如果不想一直卡住，同样在命令里添加

  > -DarchetypeCatalog=internal

```
出现选择数字：7
groupId：cn.cx.maven03(创建的项目为maven03)
artifactId：maven-service
version：1.0.0SNAPSHOT
package：cn.cx.maven.service
```

然后回到目录下就会看到目录结构已经创建好了。

2. 另一种创建目录方式，命令后面添加命令

```
mvn archetype:generate -DgroupId=cn.cx.maven04 -DartifactId=maven04-demo -Dversion=1.0.0SNAPSHOT -Dpackage=cn.cx.maven04.demo
```

如果依旧卡住，方法同上。









