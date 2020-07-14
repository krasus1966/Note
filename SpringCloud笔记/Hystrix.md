# Hystrix

hystrix用来避免微服务调用过程中出现的超时、错误、服务不可用等问题，通过熔断降级机制避免在调用过程中出现不可用的情况。

### Hystrix dashboard

用来查看接入Hystrix的服务

使用需要单独写一个客户端

```xml
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
</dependency>
```

```java
@SpringBootApplication
@EnableHystrixDashboard //开启dashboard
public class HystrixDashboardMain9001 {
    public static void main(String[] args) {
        SpringApplication.run(HystrixDashboardMain9001.class,args);
    }
}
```

```yaml
application.yml:
server:
  port: 9001
```

这就建成了一个dashboard客户端。

需要熔断降级的服务要加以下内容

```xml
<--! 后续使用很多其他依赖时其实会自带hystrix的依赖，比如openfeign -->
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
</dependency>
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

```java
@SpringBootApplication
@EnableHystrix //开启Hystrix支持
public class OrderHystrixMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderHystrixMain80.class,args);
    }
}
```

```java
@RestController
@Slf4j
@DefaultProperties(defaultFallback = "payment_Global_FallbackMethod")//定义全局fallback为哪个方法
public class OrderHystrixController {

    @Resource
    private PaymentHystrixService paymentHystrixService;

    @GetMapping("/consumer/payment/hystrix/ok/{id}")
    public String paymentInfoOk(@PathVariable("id") Integer id) {
        return paymentHystrixService.paymentInfoOk(id);
    }

    //ribbon 默认超时时间为1秒，如果超过一秒则直接进入兜底方法
    @GetMapping("/consumer/payment/hystrix/timeout/{id}")
    //定义熔断条件和fallback方法
//    @HystrixCommand(fallbackMethod = 	"paymentInfoTimeoutFallbackMethod",commandProperties = {
//            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "1000")
//    })
    @HystrixCommand
    public String paymentInfoTimeout(@PathVariable("id") Integer id) {
        return paymentHystrixService.paymentInfoTimeout(id);
    }

    public String paymentInfoTimeoutFallbackMethod(@PathVariable("id")Integer id){
        return "消费者80，对方系统繁忙，请稍后再试。";
    }

    // 全局Fallback方法
    public String payment_Global_FallbackMethod(){
        return "Global，请稍后再试";
    }
}
```

