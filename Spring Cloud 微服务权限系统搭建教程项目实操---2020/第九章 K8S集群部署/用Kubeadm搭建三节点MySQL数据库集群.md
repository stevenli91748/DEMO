

# 参考

* [MySql主从复制，从原理到实践！](http://www.macrozheng.com/#/reference/mysql_master_slave)
* [LVS+Keepalived实现mysql的负载均衡](https://www.cnblogs.com/tangyanbo/p/4305589.html)
* [你还在代码里做读写分离么，试试这个中间件吧！](http://www.macrozheng.com/#/reference/gaea)


# [1 在4台虚拟机上建立MySQL数据库](https://github.com/stevenli91748/Database/blob/master/MySQL/MySQL%20Linux%E5%AE%89%E8%A3%85/README.md)

# 2 在同一台虚拟机(192.168.33.180)上建立4个MySQL数据库

IP |role|visual machine name |
---|---|---|
192.168.33.180:33306|master1|	MySQL01	|
192.168.33.180:33307|master2|	MySQL02	|
192.168.33.180:33308|slave1|	MySQL03	|
192.168.33.180:33309|slave2|	MySQL04	|
