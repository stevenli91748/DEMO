
主库   192.168.33.180:33305 ------>  从库  192.168.33.180:33308

# 步骤

* 环境准备
     
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




* 从库配置
* 主从同步测试
* 
