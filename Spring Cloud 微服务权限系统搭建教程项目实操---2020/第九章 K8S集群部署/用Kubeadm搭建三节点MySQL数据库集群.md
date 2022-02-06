


IP |role|visual machine name |
---|---|---|
192.168.33.180:33305|master|	MySQL-master01	|
192.168.33.180:33307|master|	MySQL-master02	|
192.168.33.180:33308|slave|	MySQL-slave01	|
192.168.33.180:33309|slave|	MySQL-slave02	|

* 主mysql-master01 33305 ---> 从mysql-slave01 33308
* 主mysql-master02 33307 ---> 从mysql-slave02 33309
* mysql-master01 33305 <---> mysql-master02 33307 互为主从
* 2个写节点，每个写节点下又是2个读节点

# 参考

* [MySql主从复制，从原理到实践！](http://www.macrozheng.com/#/reference/mysql_master_slave)
* [LVS+Keepalived实现mysql的负载均衡](https://www.cnblogs.com/tangyanbo/p/4305589.html)
* [你还在代码里做读写分离么，试试这个中间件吧！](http://www.macrozheng.com/#/reference/gaea)

# 目录

## 1 建立MySQL数据库

* [1 在不同虚拟机上建立4个MySQL数据库的方式](https://github.com/stevenli91748/Database/blob/master/MySQL/MySQL%20Linux%E5%AE%89%E8%A3%85/README.md)
* [2 在同一个虚拟机上建立4个 Docker MySQL 数据库的方式](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E5%9C%A8%E5%90%8C%E4%B8%80%E4%B8%AA%E8%99%9A%E6%8B%9F%E6%9C%BA%E4%B8%8A%E7%9A%84%E6%96%B9%E5%BC%8F%E5%BB%BA%E7%AB%8B4%E4%B8%AAMySQL%E6%95%B0%E6%8D%AE%E5%BA%93.md)

## 2 MySQL主从复制
   * [主从复制](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/MySQL%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6/%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6.md)
     * MySQL一主多从架构搭建
     * MySQL多主一从架构搭建
     * MySQL多主多从架构搭建  
   * [主从切换](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/MySQL%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6/%E4%B8%BB%E4%BB%8E%E5%88%87%E6%8D%A2.md)
   * [主主复制](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/MySQL%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6/%E4%B8%BB%E4%B8%BB%E5%A4%8D%E5%88%B6.md)
## 3 MySQL分库分表
## 4 Mysql读写分离
