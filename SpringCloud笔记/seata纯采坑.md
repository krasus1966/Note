## 采坑记录

![image-20200710232945206](E:\Study\笔记\Note\SpringCloud笔记\picture\image-20200710232945206.png)

图中可以看到，Spring Cloud Alibaba版本是2.2.1 RELEASE或2.1.2 RELEASE或2.0.2 RELEASE才支持使用最新的Seata，并且配置文件和老版本Seata差别很大。

在此之前我使用的Spring Boot版本为2.2.2 RELEASE（目前修改为2.3.1 RELEASE，最新），Spring Cloud 版本为Hoxton SR1（目前修改为SR3，最新版SR6），Spring Cloud Alibaba 版本为 2.1.0 RELEASE（目前修改为2.2.1 RELEASE，最新）。

最新版本下，不用写Seata的file.conf和register.conf，直接在配置文件中配置相关信息，可以正常运行（除了报no available service 'null' found, please make sure registry config correct的错误）。

配置中心使用Nacos，建议Windows下使用官方示例中的Python文件导入配置信息，需要修改Nacos-config.txt中相应的内容，如数据库配置信息。



#### no available service 'null' found错误解决办法

如果错误信息是null，先看配置文件（1.2.0不用写conf文件），使用nacos作为配置中心，在项目的配置文件中不必出现 service 及相关内容，全部使用nacos的配置中心的内容。

![image-20200711180718014](E:\Study\笔记\Note\SpringCloud笔记\picture\image-20200711180718014.png)

![image-20200711180845533](E:\Study\笔记\Note\SpringCloud笔记\picture\image-20200711180845533.png)

配置一致的情况下，可能会出现另一个问题 

#### no available service 'default' found, please make sure registry config correct.

出现这个问题，检查一下seata/conf/registry.conf文件中的registry内的nacos，application = “serverAddr”。如果 application等于其他值，就会出现此问题。

猜测由于 NacosRegistryServiceImpl 类中定死了这个属性（不懂这个源码）。或许还有其他解决办法但我没找到。

### 新版的配置文件问题

在1.2.0版本中，官网github的配置文件需要进行修改才能推送到Nacos使用，所有的 横杠- 和 下划线_ 后面的第一个字母都要改成大写（小写驼峰）。

服务端和客户端都使用Nacos的配置中心的话，file.conf是完全不需要的，只要修改config.txt中对应的内容即可；服务端必须有registry.conf；客户端中两个conf文件都可以用配置文件（application.yml）中的配置替换，但目前好像只有1.2.0可以。





## 以上内容写得特别不好，别喷，有任何写错的地方可以私信我