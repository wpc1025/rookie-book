Spring Cloud Eureka是基于Netflix Eureka做的二次封装的产物，主要负责完成微服务架构中的服务治理功能。

##服务治理

服务治理主要用来实现各个微服务实例的自动注册与发现。

+ **服务注册：** 在服务治理框架中，通常都会构建一个注册中心，每个服务单元向注册中心登记自己提供的服务，将主机和端口号、版本号、通信协议等一些附加信息告知注册中心，注册中心按服务名分类组织服务清单。服务注册中心还需要以心跳的方式去检测清单中的服务是否可用，若不可用，需要从服务清单中剔除。
+ **服务发现：** 服务调用方在调用服务提供方接口的时候，并不知道具体得服务实例位置。因此，调用方需要向服务注册中心咨询服务，并获取所有服务的实例清单。

##Netflix Eureka

Spring Cloud Eureka，使用Netflix Eureka来实现服务注册和发现，既包含了服务端组件，也包含了客户端组件，且两端均采用Java编写。

Eureka服务端，也称为服务注册中心。支持高可用配置。依托于强一致性提供良好的服务实例可用性。

Eureka客户端，主要处理服务的注册和发现。在应用程序运行时，Eureka客户端向服务注册中心注册自己的服务并周期性的发送心跳更新它的服务租约。同时，也能从服务端查询当前注册的服务信息并把他们缓存到本地并周期性的刷新服务状态。

##搭建服务注册中心

1. 引入依赖

        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.0.3.RELEASE</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
        	<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>

2. 增加注解

`@EnableEurekaServer`注解启动一个服务注册中心提供给其他应用进行对话。

3. 修改配置

默认情况下，该服务注册中心也会将自己作为客户端来尝试注册自己，所以我们需要禁用它的客户端注册行为。在`application.properties`增加以下配置：

    server.port=1111
    eureka.instance.hostname=localhost
    eureka.client.register-with-eureka=false
    eureka.client.fetch-registry=false
    eureka.client.service-url.defaultZone=http://${eureka.instance.hostname}:${server.port}/eureka
    
4. Web访问

启动该应用，访问http://localhost:1111，查看服务中心情况。

##注册服务提供者
