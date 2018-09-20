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

传统路由配置方式并不友好，需要运维人员花费大量时间维护各个路由`path`与`url`的关系。面向服务的路由，则是通过`zuul`与`eureka`的无缝整合，可以让路由的`path`不是具体的`url`，而是让它们映射到某个具体的服务，而具体的`url`则交给`eureka`的服务发现机制去自动维护。

+ 增加eureka的依赖


    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
    </dependency>
    
+ `application.properties`相关配置


    spring.application.name=api-gateway
    server.port=5555
    
    zuul.routes.api-a.path=/api-a/**
    zuul.routes.api-a.service-id=hello-service
    
    zuul.routes.api-b.path=/api-b/**
    zuul.routes.api-b.service-id=feign-consumer
    
    eureka.client.service-url.defaultZone=http://localhost:19999/eureka
    

##请求过滤

+ 定义一个简单的`Zuul`过滤器


    public class AccessFilter extends ZuulFilter {
        @Override
        public String filterType() {
            return "pre";
        }
    
        @Override
        public int filterOrder() {
            return 0;
        }
    
        @Override
        public boolean shouldFilter() {
            return true;
        }
    
        @Override
        public Object run() throws ZuulException {
            RequestContext requestContext = RequestContext.getCurrentContext();
            HttpServletRequest httpServletRequest = requestContext.getRequest();
    
            Object accessToken = httpServletRequest.getParameter("accessToken");
            if (accessToken == null) {
                requestContext.setSendZuulResponse(false);
                requestContext.setResponseStatusCode(401);
                return null;
            }
    
            return null;
        }
    }
    

+ `filterType`过滤器的类型，决定过滤器的请求在哪个生命周期中执行
+ `filterOrder`过滤器的执行顺序，当请求在一个阶段中存在多个过滤器时，需要根据该方法返回的值依次执行
+ `shouldFilter`判断过滤器是否需要被执行
+ `run`过滤器的具体逻辑
+ 需要创建具体的Bean，过滤器才能生效


    @EnableZuulProxy
    @SpringBootApplication
    public class GatewayApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(GatewayApplication.class, args);
        }
    
        @Bean
        public AccessFilter accessFilter() {
            return new AccessFilter();
        }
    }
    
##路由详解




