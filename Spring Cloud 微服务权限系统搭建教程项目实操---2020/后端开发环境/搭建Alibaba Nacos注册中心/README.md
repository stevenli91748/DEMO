* [使用Alibaba Nacos注册中心](https://www.kancloud.cn/mrbird/spring-cloud/1271133)
* [Spring Cloud Alibaba Nacos注册中心入门实例](https://mrbird.cc/Spring-Cloud-Alibaba-Nacos%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83.html)

# Nacos安装

在以下三台虚拟机上安装Nacos server

ip| name|
192.168.33.100|	spring cloud alibaba nacos register1	|
192.168.33.101|	spring cloud alibaba nacos register2  |
192.168.33.102|  spring cloud alibaba nacos register3  |


因为Spring Cloud Alibaba 2.2.0.RELEASE内置的Nacos client版本为1.1.4，所以我们使用这个版本的Nacos。Nacos下载地址：https://github.com/alibaba/nacos/releases，选择nacos-server-1.1.4.zip 下载并解压：

解压后，打开conf目录下的配置文件，在末尾添加数据源配置：

spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://localhost:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=123456
