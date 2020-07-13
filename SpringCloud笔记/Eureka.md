# Eureka

### 介绍

Eureka是分布式服务发现注册框架，需要自己提供服务端与客户端，使用@EnableEurekaServer（服务端）、@EnableEurekaClient（客户端）来开启服务发现。

pom:

```xml
<!-- 服务端依赖 -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
<!-- 客户端依赖 -->
<dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

#### 服务端配置文件：

##### cloud-eureka-server-7001 ：application.yml

```yaml
server:
  port: 7001

eureka:
  instance:
    hostname: eureka7001.com #服务端的实例名称
  client:
    # false 表示不向注册中心注册自己
    register-with-eureka: false
    # false 表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
      # 设置与 Eureka Server 交互的地址查询服务和注册服务都需要依赖这个地址
      # defaultZone: http://eureka7002.com:7002/eureka/
      defaultZone: http://eureka7001.com:7001/eureka/
#  server:
#    # 关闭自我保护模式 保证不可用服务及时剔除
#    enable-self-preservation: false
#    eviction-interval-timer-in-ms: 2000
```

#### 客户端配置文件：

##### cloud-provider-payment-8001 ：application.yml

```yaml
server:
  port: 8001

spring:
  application:
    name: cloud-payment-service-eureka
  # zipkin 链路追踪 默认端口9411
  # zipkin:
    # base-url: http://localhost:9411
  sleuth:
    sampler:
      probability: 1
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/start_cloud?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 196610121



mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: top.krasus1966.springcloud.entities

eureka:
  client:
    # 表示是否将自己注册进 EurekaServer 默认为 true
    register-with-eureka: true
    # 是否从 EurekaServer 抓取已有的注册信息，默认为 true，单节点无所谓，集群必须设置为 true 才能配合 ribbon 使用负载均衡
    fetch-registry: true
    service-url:
#	   单机配置
#      defaultZone: http://localhost:7001/eureka
#      集群配置
       defaultZone: http://localhost:7001/eureka,http://localhost:7002/eureka
  instance:
    instance-id: payment8001
    # Eureka服务端显示的实例名称
    prefer-ip-address: true
    # Eureka客户端向服务器发送心跳的时间间隔，单位为秒，默认为30秒
    lease-renewal-interval-in-seconds: 1
    # Eureka服务端在受到最后一次心跳后等待时间上线，单位为秒，默认是90秒，超时将删除服务
    lease-expiration-duration-in-seconds: 2
```

更多配置信息建议百度。

