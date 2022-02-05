 
 
 
 IP |role|visual machine name |
---|---|---|
192.168.33.180:33305|master|	MySQL-master01	|
192.168.33.180:33307|master|	MySQL-master02	|
192.168.33.180:33308|slave|	MySQL-slave01	|
192.168.33.180:33309|slave|	MySQL-slave02	|

## 建立 MySQL-master01（192.168.33.180:33305） 数据库

             [root]# systemctl start docker

             [root]# docker pull mysql/mysql-server:tag

             [root]# docker run --name=mysql-master01 

                                -p 33305:3306 

                                -v /mydata/mysql-master01/conf:/etc/mysql/          将主机/mydata/mysql-master01/目录下的conf/挂载到容器的/etc/mysql/   

                                -v /mydata/mysql-master01/logs:/var/log/mysql       将主机/mydata/mysql-master01/目录下的log目录挂载到容器的/logs 

                                -v /mydata/mysql-master01/data:/var/lib/mysql       将主机/mydata/mysql-master01/目录下的data目录挂载到容器的/var/lib/mysql 

                                -d mysql/mysql-server:tag

             //The container initialization might take some time. When the server is ready for use, the STATUS of the container in the output of the docker ps command changes 
               from (health: starting) to (healthy).

             [root]# docker ps
             CONTAINER ID   IMAGE                COMMAND                  CREATED             STATUS                              PORTS                NAMES
             a24888f0d6f4   mysql/mysql-server   "/entrypoint.sh my..."   14 seconds ago      Up 13 seconds (health: starting)    3306/tcp, 33060/tcp  mysql1

             [root]# docker logs mysql-master01

             [root]#  docker logs mysql-master01 2>&1 | grep GENERATED
             GENERATED ROOT PASSWORD: Axegh3kAJyDLaRuBemecis&EShOs

             // 进入到MYSQL容器中
             [root]#  docker exec -it mysql-master01 mysql -uroot -p
             the password: 输入以上的原始密码

             //更改在mysql-master01容器中的数据库的root用户密码
             mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'gz@19731108';
             mysql> FLUSH PRIVILEGES

            //  Mysql8开放远程访问
            mysql> create user 'root'@'%' identified by 'Gz@19731108'; //1、先创建权限记录
            mysql> grant all privileges on *.* to 'root'@'%' with grant option; //2、授权
            mysql> exit

## 建立 MySQL-master02（192.168.33.180:33307） 数据库
             [root]# systemctl start docker

             [root]# docker pull mysql/mysql-server:tag

             [root]# docker run --name=mysql-master02 

                                -p 33307:3306 

                                -v /mydata/mysql-master02/conf:/etc/mysql/          将主机/mydata/mysql-master02/目录下的conf/挂载到容器的/etc/mysql/   

                                -v /mydata/mysql-master02/logs:/var/log/mysql       将主机/mydata/mysql-master02/目录下的log目录挂载到容器的/logs 

                                -v /mydata/mysql-master02/data:/var/lib/mysql       将主机/mydata/mysql-master02/目录下的data目录挂载到容器的/var/lib/mysql 

                                -d mysql/mysql-server:tag

             //The container initialization might take some time. When the server is ready for use, the STATUS of the container in the output of the docker ps command changes 
               from (health: starting) to (healthy).

             [root]# docker ps
             CONTAINER ID   IMAGE                COMMAND                  CREATED             STATUS                              PORTS                NAMES
             a24888f0d6f4   mysql/mysql-server   "/entrypoint.sh my..."   14 seconds ago      Up 13 seconds (health: starting)    3306/tcp, 33060/tcp  mysql1

             [root]# docker logs mysql-master02

             [root]#  docker logs mysql-master02 2>&1 | grep GENERATED
             GENERATED ROOT PASSWORD: Axegh3kAJyDLaRuBemecis&EShOs

             // 进入到MYSQL容器中
             [root]#  docker exec -it mysql-master02 mysql -uroot -p
             the password: 输入以上的原始密码

             //更改在mysql-master02容器中的数据库的root用户密码
             mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'gz@19731108';
             mysql> FLUSH PRIVILEGES

            //  Mysql8开放远程访问
            mysql> create user 'root'@'%' identified by 'Gz@19731108'; //1、先创建权限记录
            mysql> grant all privileges on *.* to 'root'@'%' with grant option; //2、授权
             
             mysql> exit


## 建立 MySQL-slave01（192.168.33.180:33308） 数据库

             [root]# systemctl start docker

             [root]# docker pull mysql/mysql-server:tag

             [root]# docker run --name=mysql-slave01 

                                -p 33308:3306 

                                -v /mydata/mysql-slave01/conf:/etc/mysql/          将主机/mydata/mysql-slave01/目录下的conf/挂载到容器的/etc/mysql/   

                                -v /mydata/mysql-slave01/logs:/var/log/mysql       将主机/mydata/mysql-slave01/目录下的log目录挂载到容器的/logs 

                                -v /mydata/mysql-slave01/data:/var/lib/mysql       将主机/mydata/mysql-slave01/目录下的data目录挂载到容器的/var/lib/mysql 

                                -d mysql/mysql-server:tag

             //The container initialization might take some time. When the server is ready for use, the STATUS of the container in the output of the docker ps command changes 
               from (health: starting) to (healthy).

             [root]# docker ps
             CONTAINER ID   IMAGE                COMMAND                  CREATED             STATUS                              PORTS                NAMES
             a24888f0d6f4   mysql/mysql-server   "/entrypoint.sh my..."   14 seconds ago      Up 13 seconds (health: starting)    3306/tcp, 33060/tcp  mysql1

             [root]# docker logs mysql-slave01

             [root]#  docker logs mysql-slave01 2>&1 | grep GENERATED
             GENERATED ROOT PASSWORD: Axegh3kAJyDLaRuBemecis&EShOs

             // 进入到MYSQL容器中
             [root]#  docker exec -it mysql-slave01 mysql -uroot -p
             the password: 输入以上的原始密码

             //更改在mysql-slave01容器中的数据库的root用户密码
             mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'gz@19731108';
             mysql> FLUSH PRIVILEGES

            //  Mysql8开放远程访问
            mysql> create user 'root'@'%' identified by 'Gz@19731108'; //1、先创建权限记录
            mysql> grant all privileges on *.* to 'root'@'%' with grant option; //2、授权
             
             mysql> exit

## 建立 MySQL-slave02（192.168.33.180:33309） 数据库

              [root]# systemctl start docker

             [root]# docker pull mysql/mysql-server:tag

             [root]# docker run --name=mysql-slave02 

                                -p 33309:3306 

                                -v /mydata/mysql-slave02/conf:/etc/mysql/          将主机/mydata/mysql-slave02/目录下的conf/挂载到容器的/etc/mysql/   

                                -v /mydata/mysql-slave02/logs:/var/log/mysql       将主机/mydata/mysql-slave02/目录下的log目录挂载到容器的/logs 

                                -v /mydata/mysql-slave02/data:/var/lib/mysql       将主机/mydata/mysql-slave02/目录下的data目录挂载到容器的/var/lib/mysql 

                                -d mysql/mysql-server:tag

             //The container initialization might take some time. When the server is ready for use, the STATUS of the container in the output of the docker ps command changes 
               from (health: starting) to (healthy).

             [root]# docker ps
             CONTAINER ID   IMAGE                COMMAND                  CREATED             STATUS                              PORTS                NAMES
             a24888f0d6f4   mysql/mysql-server   "/entrypoint.sh my..."   14 seconds ago      Up 13 seconds (health: starting)    3306/tcp, 33060/tcp  mysql1

             [root]# docker logs mysql-slave01

             [root]#  docker logs mysql-slave02 2>&1 | grep GENERATED
             GENERATED ROOT PASSWORD: Axegh3kAJyDLaRuBemecis&EShOs

             // 进入到MYSQL容器中
             [root]#  docker exec -it mysql-slave02 mysql -uroot -p
             the password: 输入以上的原始密码

             //更改在mysql-slave02容器中的数据库的root用户密码
             mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'gz@19731108';
             mysql> FLUSH PRIVILEGES
            
            //  Mysql8开放远程访问
            mysql> create user 'root'@'%' identified by 'Gz@19731108'; //1、先创建权限记录
            mysql> grant all privileges on *.* to 'root'@'%' with grant option; //2、授权
             
             mysql> exit

