
# 参考
* [MySQL一主一从架构搭建](https://juejin.cn/post/6920477753368117261)
* [细说show slave status参数详解（最全）【转】](https://www.cnblogs.com/paul8339/p/7615310.html)
* [MySQL Binlog 介绍](https://juejin.cn/post/6844903794073960455)
# 目录



# 步骤

* 环境准备

      主库   192.168.33.180:33305 ------>  从库  192.168.33.180:33308     
      操作系统：centos 8 stream
      MYSQL：mysql 8.0
      docker: 2.0.3
      主库地址：192.168.33.180：33305 (mysql-master01容器)
      从库地址：192.168.33.180：33308 （mysql-slave01容器）
      主库数据存储位置: 192.168.33.180中的 /mydata/mysql-master01/
      从库数据存储位置  192.168.33.180中的 /mydata/mysql-slave01/
      
      
* 主库配置
  
  （1）配置 my.cnf
  
      在主库数据存储位置/mydata/mysql-master01/conf目录下，创建my.cnf配置文件
      
      [root]# touch my.cnf
      [root]# vim my.cnf
             [mysqld]
             server-id=33305
         
   (2）重启主库    
   
      [root]# docker restart mysql-master01
      [root]# docker exec -it mysql-master01 mysql -uroot -p         //进入 mysql-master01 容器并登录 MySQL
      password: gz@19731108
      
      mysql> show master status;
 
  File| Position|Binlog_Do_DB|Binlog_Ignore_db|
  ---|---|---|---|
  mysql-bin.000002|156|||

               File：binglog 文件名，每次重启 mysql 服务都会生成一个新的 binlog 文件（序号递增），当文件大小超过限制（默认1G）时也会
                     生产一个新的 binlog 文件。
               Position：binlog 文件偏移量，等于binglog文件大小（字节数）
               Binlog_Do_DB：要同步的数据库。不设置的话默认同步所有的数据库，包括 mysql 默认的数据库。
               Binlog_Ignore_DB：不需要同步的数据库。

   （3）处于数据安全的考虑，添加专门用于主从复制的用户，并仅授予其复制的权限创建用户
        创建用户名为 backup，密码 123456
        
       mysql> CREATE USER 'backup'@'%' IDENTIFIED BY '123456'; 
       mysql> grant replication slave on *.* to 'backup'@'%' with grant option;


   （4） 设置 backup 远程连接的密码验证方式
   
        mysql> ALTER USER 'backup'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
        
* 从库配置

    1）配置 my.cnf 

       在从库数据存储位置/mydata/mysql-slave01/conf目录下，创建my.cnf配置文件
       
       [mysqld]
       sever_id=33307
       
       重启从库 
       [root]# docker restart mysql-slave01
       [root]# docker exec -it mysql-slave01 mysql -uroot -p
       
    2)  指定主库
        
        mysql> change master to master_host='192.168.33.180', \
                                master_user='backup',         \
                                master_port=33305,            \
                                master_password='123456',     \
                                master_log_file='mysql-bin.000002',           \      master_log_file： 对应主库中 show master status命令输出
                                master_log_pos=156;              \                   master_log_pos：  对应主库中 show master status命令输出
 
     3)  开启从库

         mysql> start slave;
         
         mysql> show slave status\G;

<a href="https://ibb.co/2NmMVs4"><img src="https://i.ibb.co/vYgdyQn/SLAVE.jpg" alt="SLAVE" border="0"></a>
                               
**只要 Slave_IO_Running 和 Slave_SQL_Running 都是 yes,就表明 主从配置成功了，这两个进程的状态需全部为 YES，只要有一个为 NO，则复制就会停止**                              
   

* 主从同步测试
* 
