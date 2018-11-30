# Maven中的坐标和仓库

#### 坐标

```
<groupId>cn.cx.maven03</groupId>
<artifactId>maven03-model</artifactId>
<version>0.0.1SNAPSHOP</version>
这就是项目的基本坐标
```

在创建包时包名应该与**groupId**和**artifactId**相吻合，这样看起来更清晰、符合逻辑。

#### 仓库

仓库用来管理项目依赖，仓库也分为两种：本地仓库和远程仓库。

远程仓库全球地址可以在***/apache-maven-3.3.3/lib/maven-model-builder-3.3.3.jar/org/apache/maven/model/pom-4.0.0.xml***中的**<url>**标签中看到。

#### 镜像仓库

如何修改镜像仓库地址：

打开***/apache-maven-3.3.3/conf/setting.xml***,找到

```
<mirrors>
    <mirror>
        <id>maven</id>
        <mirrorOf>central</mirrorOf> *
        <name>central mirror in china</name>
        <url>http://maven.net.cn/content/group/public</url>
    </mirror>
<mirrors>
```

#### 更改仓库位置

默认存放在用户个人目录中，即***C:\Users\ASUS\.m2\repository***中。

依旧是修改***setting.xml***来更改仓库位置

找到

```
<localRepository></localRepository>
```

在其中添加目录位置

```
<localRepository>E:/Study/Tools/MavenRespository</localRepository>2
```

保存