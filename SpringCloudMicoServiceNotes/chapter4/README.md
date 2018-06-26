Spring Cloud Ribbon 是一个基于HTTP和TCP的客户端负载均衡工具，基于Netflix Ribbon 实现。通过Spring Cloud的封装，可以让我们轻松的将面向服务的REST模板请求自动转换成客户端负载均衡的服务调用。

##客户端负载均衡

通常所说的负载均衡都指的是服务端负载均衡，其中分为硬件负载均衡（如F5）和软件负载均衡（如Nginx）。  
服务端与客户端的不同点在于服务清单所存储的位置。

微服务架构中使用客户端负载均衡的步骤：
+ 服务提供者启动多个实例并注册到一个注册中心或是多个相关联的服务注册中心
+ 服务消费者直接通过调用被`@LoadBalanced`注解修饰过的`RestTemplate`来实现面向服务的接口调用

##RestTemplate详解

###GET请求

在`RestTemplate`中，对GET请求可以通过如下两个方法进行调用实现：
+ `getForEntity`函数
+ `getForObject`函数

###POST请求

在`RestTemplate`中，对POST请求时可以通过如下三个方法进行调用实现：
+ `postForEntity`函数
+ `postForObject`函数
+ `postForLocation`函数

###PUT请求

在`RestTemplate`中，对PUT请求可以通过`put`方法进行调用实现

###DELETE请求

在`RestTemplate`中，对DELETE请求可以通过`delete`方法进行调用实现

##源码分析

##负载均衡器

##负载均衡策略







