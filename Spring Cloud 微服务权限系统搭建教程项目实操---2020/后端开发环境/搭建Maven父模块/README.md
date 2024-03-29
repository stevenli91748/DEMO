# [搭建Maven父模块](https://www.kancloud.cn/mrbird/spring-cloud/1263687)


# 程序代码

pom.xml 文件

```

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mybird</groupId>
    <artifactId>febs-cloud</artifactId>
    <version>1.0-SNAPSHOT</version>

    <modules>
        <module>../febs-common</module>
        <module>../febs-register-nacos</module>
        <module>../febs-auth</module>
        <module>../febs-gateway-zuul</module>
        <module>../febs-microservice</module>
    </modules>

    <packaging>pom</packaging>

    <name>FEBS-Cloud</name>
    <description>FEBS-Cloud Parent : Spring Cloud Spring Security OAuth2</description>

    <parent>
        <artifactId>spring-boot-starter-parent</artifactId>
        <groupId>org.springframework.boot</groupId>
        <version>2.2.5.RELEASE</version>
        <relativePath/>
    </parent>

    <properties>
        <spring-cloud.version>Hoxton.SR8</spring-cloud.version>
        <spring-cloud-alibaba.version>2.2.0.RELEASE</spring-cloud-alibaba.version>
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

            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring-cloud-alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

        </dependencies>
    </dependencyManagement>



</project>



```
