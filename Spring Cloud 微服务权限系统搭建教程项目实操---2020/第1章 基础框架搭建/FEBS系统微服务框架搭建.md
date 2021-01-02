
# 目录
---

* [1. Maven父模块搭建](#1-Maven父模块搭建)
* [2. 通用模块搭建](#2-通用模块搭建)
* [3. 搭建微服务注册中心(febs-register)](#3-搭建微服务注册中心)
  * [3A. 使用Security保护微服务注册中心](#3A-使用Security保护微服务注册中心)
* [6. lombok plugin安装](#6-lombok-plugin安装)
---

# 1 Maven父模块搭建

首先我们使用IDEA创建一个名称为FEBS-Cloud的 Maven模块，该模块为整个工程的服务模块，用于聚合各个微服务子系统。 在D盘根目录创建一个名称为febs的文件夹，然后打开IDEA，点击Create New Project
新建一个Maven项目，Project SDK选择JDK 1.8

<a href="https://ibb.co/ZW1m5dX"><img src="https://i.ibb.co/m6F8kJT/idea1.png" alt="idea1" border="0"></a>

点击Next，如下图所示填写GroupId和ArtifactId：

<a href="https://ibb.co/bWfb5Tt"><img src="https://i.ibb.co/C1rPKgL/idea2.png" alt="idea2" border="0"></a>

点击Next，按照下图所示填写相关内容，路径选择D盘根目录下的febs：


<a href="https://ibb.co/v4qTnf2"><img src="https://i.ibb.co/RTbswtL/idea3.png" alt="idea3" border="0"></a>

点击Finish完成创建。创建好后，项目如下所示:

<a href="https://ibb.co/1vNB39C"><img src="https://i.ibb.co/Fx1c9BQ/idea4.png" alt="idea4" border="0"></a>

因为febs-cloud模块是项目的父模块，仅用于聚合子模块，所以我们可以把src目录下的内容全部删了，只保留pom.xml（febs-cloud.iml为IDEA文件，不能删哦），然后修改pom.xml，引入Spring Boot
和Spring Cloud：

        <?xml version="1.0" encoding="UTF-8"?>
        <project xmlns="http://maven.apache.org/POM/4.0.0"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
            <modelVersion>4.0.0</modelVersion>

            <groupId>cc.mrbird</groupId>
            <artifactId>febs-cloud</artifactId>
            <version>1.0-SNAPSHOT</version>
            <packaging>pom</packaging>

            <name>FEBS-Cloud</name>
            <description>FEBS-Cloud：Spring Cloud，Spring Security OAuth2 微服务权限管理系统</description>

            <parent>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-parent</artifactId>
                <version>2.1.6.RELEASE</version>
                <relativePath/> <!-- lookup parent from repository -->
            </parent>

            <properties>
                <spring-cloud.version>Greenwich.SR1</spring-cloud.version>
            </properties>

            <dependencyManagement>
                <dependencies>
                    <dependency>
                        <groupId>org.springframework.cloud</groupId>
                        <artifactId>spring-cloud-dependencies</artifactId>
                        <version>${spring-cloud.version}</version>
                        <type>pom</type>
                        <scope>import</scope>
                    </dependency>
                </dependencies>
            </dependencyManagement>
        </project>

上面的pom配置中，我们指定了packaging为pom，表示这是一个纯聚合模块，无需打包为jar或者war；name指定为FEBS-Cloud；引入了Spring Boot 2.1.6.RELEASE和Spring Cloud Greenwich.SR1。

至此，父模块搭建完毕，接下来开始搭建通用模块。

---

# 2 通用模块搭建

通用模块主要用于定义一些各个微服务通用的实体类，工具类或者第三方依赖等。

点击File -> New -> Module...，新建一个Maven模块，Module SDK选择JDK 1.8：


<a href="https://ibb.co/4mvVQS5"><img src="https://i.ibb.co/ngdLW15/idea6.png" alt="idea6" border="0"></a>

点击Next：

<a href="https://ibb.co/2ycywcF"><img src="https://i.ibb.co/80P0JPm/idea7.png" alt="idea7" border="0"></a>

父模块选择我们上面创建好的febs-cloud，ArtifactId填febs-common，然后点击Next：

<a href="https://ibb.co/pJYpyt2"><img src="https://i.ibb.co/MMFL6Yn/idea8.png" alt="idea8" border="0"></a>

填写内容如上图所示（注意febs-common和febs-cloud都位于febs目录下，它们在目录结构上是平级的关系），点击Finish完成创建。创建好后，项目结构如下所示：

<a href="https://ibb.co/XsPBvxf"><img src="https://i.ibb.co/wWqHxhZ/idea9.png" alt="idea9" border="0"></a>

这时候我们查看febs-cloud的pom文件，会发现它新增了如下内容：

              <modules>
                 <module>../febs-common</module>
              </modules>
              
因为我们刚刚在创建febs-common模块的时候选择febs-cloud作为父模块。  我们往febs-common模块的pom里添加一些后续要用到的依赖：              


        <?xml version="1.0" encoding="UTF-8"?>
        <project xmlns="http://maven.apache.org/POM/4.0.0"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
            <parent>
                <artifactId>febs-cloud</artifactId>
                <groupId>cc.mrbird</groupId>
                <version>1.0-SNAPSHOT</version>
                <relativePath>../febs-cloud/pom.xml</relativePath>
            </parent>
            <modelVersion>4.0.0</modelVersion>

            <artifactId>febs-common</artifactId>
            <name>FEBS-Common</name>
            <description>FEBS-Common通用模块</description>

            <dependencies>
                <dependency>
                    <groupId>org.projectlombok</groupId>
                    <artifactId>lombok</artifactId>
                </dependency>
                <dependency>
                    <groupId>com.alibaba</groupId>
                    <artifactId>fastjson</artifactId>
                    <version>1.2.51</version>
                </dependency>
                <dependency>
                    <groupId>org.apache.commons</groupId>
                    <artifactId>commons-lang3</artifactId>
                </dependency>
                <dependency>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-web</artifactId>
                </dependency>
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-starter-oauth2</artifactId>
                </dependency>
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-starter-security</artifactId>
                </dependency>
            </dependencies>
        </project>

至此，通用模块也搭建完毕了，接下来开始搭建微服务注册中心。

---

# 3 搭建微服务注册中心

  * [主流微服务注册中心浅析和对比](https://my.oschina.net/yunqi/blog/3040280)
  * [Nacos---主流]()
  * [Eureka]()
  * [Zookeeper]()
  * [Consul]()

微服务注册中心的作用就是用于统一管理微服务实例，微服务间的调用只需要知道对方的服务名，而无需关注具体的IP和端口，便于微服务架构的拓展和维护。在这一节中，我们先使用Eureka构建微服务注册中心（Eureka服务端），因为Eureka较为简单，无须启动第三方服务，只需要引入相关依赖即可。后续会介绍如何使用Nacos构建微服务注册中心。

在IDEA的菜单栏中点击File -> New -> Modules...，新建一个Spring Boot项目，模板选择Spring Initializr，Module SDK选择JDK 1.8：

<a href="https://ibb.co/ChpKGZY"><img src="https://i.ibb.co/vZC4nGT/idea10.png" alt="idea10" border="0"></a>

点击Next，然后按照下图所示填写相关内容

<a href="https://ibb.co/0JcHZFB"><img src="https://i.ibb.co/MNsFpG8/idea11.png" alt="idea11" border="0"></a>

继续点击Next，在依赖列表中，搜索Eureka Server，然后添加进去：

<a href="https://ibb.co/Ky3cCpn"><img src="https://i.ibb.co/RHVGLk8/idea3.png" alt="idea3" border="0"></a>

继续点击Next，填写模块名称和路径：

<a href="https://ibb.co/smR6K7k"><img src="https://i.ibb.co/f1FnkJB/idea4.png" alt="idea4" border="0"></a>

点击Finish，完成构建，至此项目结构如下所示：

<a href="https://ibb.co/C93601X"><img src="https://i.ibb.co/PWvMCmb/idea5.png" alt="idea5" border="0"></a>

使用Spring Initializr构建的是一个Spring Boot应用，其父模块是spring-boot-starter-parent，而我们项目的父模块是上一节中构建的febs-cloud，所以我们调整一下febs-register模块的pom文件，调整后如下所示：


        <?xml version="1.0" encoding="UTF-8"?>
        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <parent>
                <artifactId>febs-cloud</artifactId>
                <groupId>cc.mrbird</groupId>
                <version>1.0-SNAPSHOT</version>
                <relativePath>../febs-cloud/pom.xml</relativePath>
            </parent>

            <artifactId>febs-register</artifactId>
            <name>FEBS-Register</name>
            <description>FEBS-Cloud服务注册中心</description>

            <dependencies>
                <dependency>
                    <groupId>org.springframework.cloud</groupId>
                    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
                </dependency>
            </dependencies>

            <build>
                <plugins>
                    <plugin>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-maven-plugin</artifactId>
                    </plugin>
                </plugins>
            </build>

        </project>


着修改febs-cloud模块的pom，在modules标签里添加上刚刚创建的febs-register模块：

        <modules>
            <module>../febs-common</module>
            <module>../febs-register</module>
        </modules>

打开febs-register的入口类FebsRegisterApplication，在类上使用@EnableEurekaServer标注，用以开启Eureka服务端功能：


        EnableEurekaServer
        @SpringBootApplication
        public class FebsRegisterApplication {

            public static void main(String[] args) {
                SpringApplication.run(FebsRegisterApplication.class, args);
            }
        }



然后开始编写项目配置文件，由于我个人比较习惯yml格式的配置，所以我将resources目录下的application.properties重命名为application.yml（后面章节搭建的boot项目，配置文件都采用yml格式，请知悉），在该配置文件中编写如下内容：

        server:
          port: 8001
          servlet:
            context-path: /register

        spring:
          application:
            name: FEBS-Register

        eureka:
          instance:
            hostname: localhost
          client:
            register-with-eureka: false
            fetch-registry: false
            instance-info-replication-interval-seconds: 30
            serviceUrl:
              defaultZone: http://${eureka.instance.hostname}:${server.port}${server.servlet.context-path}/eureka/

项目的端口号为8001（在上一节中我们已经约定过），context-path为/register，剩下的配置含义如下：

spring.application.name，定义服务名称为FEBS-Register;

eureka.instance.hostname，指定了Eureka服务端的地址，因为我们是在本地搭建的，所以填写为localhost即可；

eureka.client.register-with-eureka，表示是否将服务注册到Eureka服务端，由于我们这里是单节点的Eureka服务端，所以这里指定false；

eureka.client.fetch-registry，表示是否从Eureka服务端获取服务信息，因为这里是单节点的Eureka服务端，并不需要从别的Eureka服务端同步服务信息，所以这里设置为false；

eureka.client.instance-info-replication-interval-seconds，微服务更新实例信息的变化到Eureka服务端的间隔时间，单位为秒，这里指定为30秒（这就是微服务启动后，要过一会才能注册到Eureka服务端的原因）。

eureka.client.serviceUrl.defaultZone，指定Eureka服务端的地址，这里为当前项目地址，即 http://localhost:8001/register/eureka/

由于要搭建的微服务模块较多，所以为了在项目启动的时候更直观的区分开当前启动的是哪个微服务模块，我们可以自定义一个启动banner。在resources目录下新建一个banner.txt文件，文件内容如下所示：


|------------------------------|
|    ____  ____  ___   __      |
|   | |_  | |_  | |_) ( (`     |
|   |_|   |_|__ |_|_) _)_)     |
|                              |
|   ${spring.application.name}              |
|   Spring-Boot: ${spring-boot.version} |
|------------------------------|

至此，一个简单的微服务注册中心搭建好了，我们运行入口类FebsRegisterApplication的main方法启动项目，启动后访问 http://localhost:8001/register/ 出现Eureka页面说明微服务注册中心搭建成功，目前还没有微服务实例注册进来，所以列表是空的。


# 3A 使用Security保护微服务注册中心

目前Eureka服务端是“裸奔着”的，只要知道了Eureka服务端的地址后便可以将微服务注册进来，我们可以引入spring-cloud-starter-security来保护Eureka服务端。在febs-register模块的pom文件中添加如下依赖

       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-security</artifactId>
       </dependency>


在cc.mrbird.febs.register路径下新建configure包，然后在configure包下新建FebsRegisterWebSecurityConfigure配置类：

        @EnableWebSecurity
        public class FebsRegisterWebSecurityConfigure extends WebSecurityConfigurerAdapter {
            @Override
            protected void configure(HttpSecurity http) throws Exception {
                http.csrf().ignoringAntMatchers("/eureka/**");
                super.configure(http);
            }
        }

该配置类用于开启Eureka服务端端点保护。在application.yml中配置访问Eureka服务的受保护资源所需的用户名和密码：

        spring:
          security:
            user:
              name: febs
              password: 123456

由于现在Eureka服务端是受保护的，需要正确的用户名和密码才能访问，所以上面我们配置的eureka.client.serviceUrl.defaultZone路径也要配置上账号密码，否则将抛出com.netflix.discovery.shared.transport.TransportException: Cannot execute request on any known server异常。将eureka.client.serviceUrl.defaultZone的配置改为如下所示:


         eureka:
           client:
             serviceUrl:
               defaultZone: http://${spring.security.user.name}:${spring.security.user.password}@${eureka.instance.hostname}:${server.port}${server.servlet.context-path}/eureka/
      
      
配置的格式为：eureka.client.serviceUrl.defaultZone=http://${userName}:${password}@${hosetname}:${port}${server.servlet.context-path}/eureka/      


重启项目，再次访问 http://localhost:8001/register/:

<a href="https://ibb.co/WVRsPwb"><img src="https://i.ibb.co/vXMDQG7/idea15.png" alt="idea15" border="0"></a>

这次需要登录才能访问，输入我们在配置文件中定义的用户名和密码（febs，123456）后便可成功访问注册列表页面。

到这里微服务注册中心febs-register已经搭建完毕，下一节开始着手搭建微服务认证中心febs-auth。




---


# 6 lombok plugin安装

lombok的使用需要安装相关插件（lombok可以通过注解自动生成get，set等方法，不懂的同学可以自行百度lombok），双击Shift，然后输入plugins：

<a href="https://ibb.co/r4HL92S"><img src="https://i.ibb.co/YphKgBM/plguin1.png" alt="plguin1" border="0"></a>

选择第一个，然后按Enter键，输入lombok，安装列表中的第一个，然后重启IDEA即可：

<a href="https://ibb.co/RQyw4w3"><img src="https://i.ibb.co/frvwGwk/plguin2.png" alt="plguin2" border="0"></a>
