

# 参考

* [MySql主从复制，从原理到实践！](http://www.macrozheng.com/#/reference/mysql_master_slave)
* [LVS+Keepalived实现mysql的负载均衡](https://www.cnblogs.com/tangyanbo/p/4305589.html)
* [你还在代码里做读写分离么，试试这个中间件吧！](http://www.macrozheng.com/#/reference/gaea)

# 目录

# 第一步  建立MySQL数据库

* [1 在不同虚拟机上的方式建立4个MySQL数据库](https://github.com/stevenli91748/Database/blob/master/MySQL/MySQL%20Linux%E5%AE%89%E8%A3%85/README.md)
* 2 在同一个虚拟机上的方式建立4个MySQL数据库

IP |role|visual machine name |
---|---|---|
192.168.33.180:33305|master|	MySQL-master01	|
192.168.33.180:33307|master|	MySQL-master02	|
192.168.33.180:33308|slave|	MySQL-slave01	|
192.168.33.180:33309|slave|	MySQL-slave02	|

## 建立 MySQL-master01（192.168.33.180:33305） 数据库


## 建立 MySQL-master02（192.168.33.180:33307） 数据库
## 建立 MySQL-slave01（192.168.33.180:33308） 数据库
## 建立 MySQL-slave02（192.168.33.180:33309） 数据库
