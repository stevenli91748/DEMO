<a href="https://ibb.co/FmNcJGx"><img src="https://i.ibb.co/GnBGkgx/lvs-keepalived-nginx.png" alt="lvs-keepalived-nginx" border="0"></a>

<a href="https://ibb.co/Sy9TPFL"><img src="https://i.ibb.co/qjPGdTZ/20200721155351865.png" alt="20200721155351865" border="0"></a>


集群节点部署情况


服务器IP	|部署服务| 端口|	备注|
---|---|---|---|
192.168.33.180|MySQL|3306|测试，使用单机 MySQL，[高可用参考：MySQL 5.7.28 主从复制实现](https://blog.csdn.net/lzb348110175/article/details/103081631)|
192.168.33.142 |Nginx01|8087 |测试，使用单机 Nginx，Nginx集群搭建请自行了解(Nginx默认端口为80，此处负载均衡使用8087端口)|
192.168.33.100 |nacos| 8848| 集群节点01：nacos register1|
192.168.33.101	|nacos| 8848| 集群节点02：nacos register2|
192.168.33.102 |nacos| 8848| 集群节点03：nacos register3|



# 参考
* [docker swarm 搭建 nacos 集群](https://www.jianshu.com/p/23d2c145423e)
* [ Nacos集群搭建可以看看这篇实例教程](https://os.51cto.com/article/649179.html)
* [Spring Cloud Alibaba Nacos注册中心](https://mrbird.cc/Spring-Cloud-Alibaba-Nacos%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83.html)
* [Nacos集群搭建过程详解](https://juejin.cn/post/6844903907706011662) 
* [使用Docker完成Nacos集群部署](https://juejin.cn/post/6861996608247201806)
* [微服务注册中心nacos集群搭建指南](https://www.kancloud.cn/cehgnxuyuan_123/springcloud)
* [Nacos 集群搭建和持久化配置(Linux)](https://blog.csdn.net/m0_37989980/article/details/108580168)

# 目录
* Nacos集群安装
  * [使用虚拟机完成Nacos集群部署 ](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E4%BD%BF%E7%94%A8%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%AE%8C%E6%88%90Nacos%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2.md)
  * [使用Docker完成Nacos集群部署 ](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E4%BD%BF%E7%94%A8Docker%E5%AE%8C%E6%88%90Nacos%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2%20.md)
