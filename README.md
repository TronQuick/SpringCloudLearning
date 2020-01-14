## 微服务架构介绍

**微服务架构（MicroServices）架构风格：**

- 一系列微小的服务共同组成

- 跑在自己的进程里
- 每个服务为独立的业务开发
- 独立部署
- 分布式的管理



**分布式定义：**

旨在支持应用程序和服务的开发，可以利用物理架构由**多个自治的处理元素，不共享主内存**，但通过网络发送**消息**合作



![微服务架构图](docs\img\微服务架构图.png)

**微服务架构基础框架/组件**

- **服务注册发现**
- **服务网关（Service Gateway）**
- **后端通用服务（aka中间层服务）（Middle Tier Service）**
- **前端服务（aka边缘服务）（Edge Service）**





## Spring Cloud介绍

- Spring Cloud 是一个开发工具集，含了多个子项目
  - 利用 Spring Boot 的开发便利
  - 主要是基于对 Netflix 开源组件的进一步封装
- Spring Cloud **简化了分布式开发**
- **掌握如何使用，更要理解分布式、架构的特点**





## Spring Cloud Eureka

- 基于 Netflix Eureka 做了二次封装
- 两个组件组成：
  - Eureka Server **注册中心**
  - Eureka Client  **服务注册**



### Eureka Server 注册中心

- 分布式系统中，服务注册中心是最重要的基础部分



- Application启动类中加注解：`@EnableEurekaServer`

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaApplication {

    public static void main(String[] args) {
        SpringApplication.run(EurekaApplication.class, args);
    }

}
```

- application.yml:

  关闭自我保护模式:只能在dev下关闭，生产环境不要关闭

  eureka.server.enable-self-preservation: false 

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/ #注册地址   
    register-with-eureka: false
  server:
    enable-self-preservation: false  #关闭自我保护模式
spring:
  application:
    name: eureka #应用名
server:
  port: 8761
```



### Eureka Client  服务注册

- Application启动类中加注解:`@EnableDiscoveryClient`

```java
@SpringBootApplication
@EnableDiscoveryClient
public class ClientApplication {

    public static void main(String[] args) {
        SpringApplication.run(ClientApplication.class, args);
    }

}
```

- application.yml

```yaml
eureka:
  client:
    service-url:
            defaultZone: http://localhost:8761/eureka/,http://localhost:8762/eureka/  #注册地址。两个eureka都注册
  instance:
    hostname: clientName #自定义host名 
spring:
  application:
    name: client #应用名

```



### Eureka 高可用

- 生产上建议至少两台以上eureka

![EurekaServer高可用](docs\img\EurekaServer高可用.png)

![EurekaServer高可用](docs\img\EurekaServer高可用2.png)

（eureka互相注册后会交换信息，client可以在多个eureka中注册，就算其中一个eureka挂掉，其他也可以用）

