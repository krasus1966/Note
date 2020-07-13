# Ribbon

#### 介绍

Ribbon 负载均衡 使用前提，Eureka或其他使用集群配置。

使用RestTemplate类来像服务端发送请求，首先配置RestTemplate的Bean，并加载负载均衡

```java
@Configuration
public class ApplicationContextConfig {

    @Bean
    @LoadBalanced //@LoadBalanced 注解赋予了RestTemplate负载均衡的能力
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

若想使用自定义负载均衡（不使用默认的轮询）

```java
//需要注意，这个类不能和启动类在同一个包及子包下
@Configuration
public class MySelfRule {
    @Bean
    public IRule myRule(){
        return new RandomRule();
    }
}
```

最后在启动类上添加Ribbon注解，传入自定义负载均衡类

```java
@SpringBootApplication
@EnableEurekaClient
// 使用自定义负载均衡
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE-EUREKA",configuration = MySelfRule.class)
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class,args);
    }
}
```

