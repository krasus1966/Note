# Feign/OpenFeign

### 介绍

Feign 声明式REST客户端，使用Feign从消费端向服务端发送请求。

pom.xml

```xml
<dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

使用需要创建一个Feign客户端，接口形式（类似于不写方法体的Controller）

```java
//交给SpringIOC容器管理
@Component
//注册为Feign客户端
@FeignClient(value = "CLOUD-PAYMENT-SERVICE-EUREKA")
public interface PaymentFeignService {

	//feign会使用GET请求向此地址发送请求
    @GetMapping("/payment/get/{id}")
     CommonResult<Payment> getPaymentById(@PathVariable("id")Long id);

    @GetMapping("/payment/feign/timeout")
    String paymentFeignTimeout();
}
```

改造Controller，获得Feign客户端获取的数据

```java
@RestController
@Slf4j
public class OrderFeignController {

    @Resource
    private PaymentFeignService paymentFeignService;

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id){
        return paymentFeignService.getPaymentById(id);
    }

    @GetMapping("/consumer/payment/feign/timeout")
    public String paymentFeignTimeout(){
        return paymentFeignService.paymentFeignTimeout();
    }
}
```

修改启动类

```java
@SpringBootApplication
//开启Feign
@EnableFeignClients
public class OrderFeignMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderFeignMain80.class,args);
    }
}
```

