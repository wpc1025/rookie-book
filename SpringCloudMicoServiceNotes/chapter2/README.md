##框架简介

Spring Boot的宗旨并非要重写Spring或是替代Spring，而是希望通过设计大量的自动化配置等方式来简化Spring原有样板化的配置，使得开发者可以快速构建应用。

Spring Boot还通过一系列Starter POMs的定义，让我们整合各项功能的时候，不需要维护错综复杂的依赖关系。

Spring Boot内置tomcat、jetty等容器，只需把Spring Boot应用打成jar包，并通过java -jar就能直接运行一个标准化的web应用。

##快速入门

##项目构建和解析

##实现RESTful API

##配置详解

##配置文件

##自定义参数

##参数引用

##使用随机数

##命令行参数

##多环境配置

##加载顺序

Spring Boot属性加载顺序：
1. 命令行传入的参数
2. SPRING_APPLICATION_JSON中的属性。SPRING_APPLICATION_JSON是以JSON格式配置在系统环境变量中的内容
3. java:comp/env中的JNDI属性
4. java的系统属性，可以通过System.getProperties()获得的内容
5.  操作系统的环境变量
6. 通过random.*配置的随机属性
7. 位于当前应用jar包之外，针对不同{profile}环境的配置文件内容，例如application-{profile}.properties或是YAML定义的配置文件
8. 位于当前应用jar包之内，针对不同{profile}环境的配置文件内容，例如application-{profile}.properties或是YAML定义的配置文件
9. 位于当前应用jar包之外的application.properties和YAML配置内容
10. 位于当前应用jar包之内的application.properties和YAML配置内容
11. 在@Configuration注解修改的类中，通过@PropertySource注解定义的属性
12. 应用默认属性，使用SpringApplication.setDefaultProperties定义的内容

##监控和管理


