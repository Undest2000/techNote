SpringBoot使用XML配置文件的方法：

```java
//@EnableDubbo
//引入配置信息
@ImportResource(locations="classpath:provider.xml")
@SpringBootApplication
public class UserServiceProviderApplication {
//略
}
```

```xml
<dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
            <version>0.2.0</version>
</dependency>
```

如果要引入Zookeeper作为注册中心：

```xml
<dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.4.6</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

| Versions | Java | Spring Boot | Dubbo  |
| -------- | ---- | ----------- | ------ |
| 0.2.0    | 1.8+ | 2.0.x       | 2.6.2+ |
| 0.1.1    | 1.7+ | 1.5.x       | 2.6.2+ |

服务提供方

```java
import com.alibaba.dubbo.config.annotation.Service;
import com.zang.gmall.bean.UserAddress;
import com.zang.gmall.service.UserService;
import org.springframework.stereotype.Component;

import java.util.Arrays;
import java.util.List;

@Service   //属于Dubbo的@Service注解，非Spring  作用：暴露服务
@Component
public class UserServiceImpl implements UserService {

    @Override
    public List<UserAddress> getUserAddressList(String userId) {
    //省略
}}
```

```properties
#当前服务/应用的名字
dubbo.application.name=user-service-provider
#注册中心的协议和地址
dubbo.registry.protocol=zookeeper
dubbo.registry.address=127.0.0.1:2181
#通信规则（通信协议和接口）
dubbo.protocol.name=dubbo
dubbo.protocol.port=20880
#连接监控中心
dubbo.monitor.protocol=registry
#开启包扫描，可替代 @EnableDubbo 注解
##dubbo.scan.base-packages=com.zang.gmall
```

如果采用XML方式配置（示例）：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
    <!-- 提供方应用信息 -->
    <dubbo:application name="provider"  />
    <!-- 注册中心服务地址 -->
    <dubbo:registry  protocol="zookeeper" address="127.0.0.1"  check="false" />
    <!-- 用dubbo协议-->
    <dubbo:protocol name="dubbo" port="-1" dispather="all" check="false" />
    <dubbo:provider timeout="10000"  threads="10" threadpool="fixed"   loadbalance="roundrobin"/>
    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="com.jaycekon.dubbo.service" ref="userService"/> </beans>
```

```java
import com.alibaba.dubbo.config.spring.context.annotation.EnableDubbo;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

// 开启基于注解的dubbo功能（主要是包扫描@DubboComponentScan）
// 也可以在配置文件中使用dubbo.scan.base-package来替代   @EnableDubbo
@EnableDubbo

@SpringBootApplication
public class UserServiceProviderApplication {

    public static void main(String[] args) {
        SpringApplication.run(UserServiceProviderApplication.class, args);
    }
}
```

服务消费者：

```java
import com.alibaba.dubbo.config.annotation.Reference;
import com.zang.gmall.bean.UserAddress;
import com.zang.gmall.service.OrderService;
import com.zang.gmall.service.UserService;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class OrderServiceImpl implements OrderService {
    //Dubbo替换的Autowired注解，用来引入服务提供者的服务
    @Reference
    UserService userService;

    @Override
    public List<UserAddress> initOrder(String userId) {
　　//代码省略
   }}
```

```properties
#避免和监控中心端口冲突，设为8081端口访问
server.port=8081  

dubbo.application.name=order-service-consumer
dubbo.registry.address=zookeeper://127.0.0.1:2181
dubbo.monitor.protocol=registry
```

如果采用XML配置方式(示例):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
    <!-- 提供方应用信息 -->
    <dubbo:application name="provider"  />
    <!-- 注册中心服务地址 -->
    <dubbo:registry  protocol="zookeeper" address="${dubbo.registry.address}"  check="false" />
    <!-- 用dubbo协议-->
    <dubbo:protocol name="dubbo" port="-1" dispather="all" check="false" />
    <dubbo:provider timeout="10000"  threads="10" threadpool="fixed"   loadbalance="roundrobin"/>
    <!-- 声明需要暴露的服务接口 -->
    <dubbo:service interface="com.jaycekon.dubbo.service" ref="userService"/>
</beans>
```

启动类同样加上@EnableDubbo注解

为查看调用是否成功，新增控制层用于访问

```java
import com.zang.gmall.bean.UserAddress;
import com.zang.gmall.service.OrderService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import java.util.List;

@Controller
public class OrderController {

    @Autowired
    OrderService orderService;

    @ResponseBody   //以json格式返回
    @RequestMapping("/initOrder")
    public List<UserAddress> initOrder(@RequestParam("uid") String userId){

       return orderService.initOrder(userId);
    }

}
```

DUBBO入门：https://segmentfault.com/a/1190000019896723

官方文档:[Apache Dubbo](https://dubbo.apache.org/zh/)
