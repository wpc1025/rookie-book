#背景

+ 需要一套机制来有效降低维护路由规则与服务实例列表的难度
+ 需要一套机制能够很好的解决微服务架构中，对于微服务接口访问时各前置校验的冗余问题

#功能

+ 网关用于所有外部客户端访问的调度和过滤，实现了请求路由、负载均衡、校验过滤以及与服务治理框架的结合、请求转发时的熔断机制、服务的聚合等一系列功能

#快速入门

##构建网关

+ 创建一个SpringBoot工程，并引入依赖如下：


    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
    </dependency>
    
+ 创建应用主类，使用`@EnableZuulProxy`注解开启`Zuul`的API网关服务功能


    @EnableZuulProxy
    @SpringBootApplication
    public class GatewayApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(GatewayApplication.class, args);
        }
    }
    

+ 在`application.properties`中配置基础信息，如应用名、端口等


    spring.application.name=api-gateway
    server.port=5555
    
##请求路由

###传统路由方式

    spring.application.name=api-gateway
    server.port=5555
    
    zuul.routes.api-a-url.path=/api-a-url/**
    zuul.routes.api-a-url.url=http://localhost:8080/
    
当访问`http://localhost:5555/api-a-url/hello`时，API网关将会将请求路由到`http://localhost:8080/hello`提供的服务接口上

###面向服务的路由
