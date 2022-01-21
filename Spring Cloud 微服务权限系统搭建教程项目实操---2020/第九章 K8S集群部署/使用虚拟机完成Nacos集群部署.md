# 目录
* [1 配置JAVA_HOME环境变量, 下载安装Nacos](#1-配置JAVA_HOME环境变量)
* [2 在同一虚拟机上使用不同的端口搭建Nacos集群](#2-在同一虚拟机上使用不同的端口搭建Nacos集群)
* [3 在不同虚拟机上使用相同的端口搭建Nacos集群](#3-在不同虚拟机上使用相同的端口搭建Nacos集群)

# 1 配置JAVA_HOME环境变量
   
    每台虚拟机都要配置JAVA_HOME环境变量，不配置会导致无法运行Nacos
  
    假如没有设置JAVA_HOME环境变量， 就启动 Nacos , 会出现如下错误：
   
    [root@register1 bin]# bash startup.sh -m standalone

     which: no javac in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
     readlink:  miss operand 
     Try 'readlink --help' for more information.
     dirname:  miss operand 
     Try 'dirname --help' for more information.
     ERROR: Please set the JAVA_HOME variable in your environment, We need java(x64)! jdk8 or later is better! !!
     
     
     [root@register1 bin]# yum search jdk  //查找在yum中的JDK version
     ......
     java-1.8.0-Openjdk.x86_64      // 这是OpenJDK 8 Runtime Environment
     .....
     java-1.8.0-Openjdk-devel.x86_64  // 这是 jdk的开发環境（OpenJDK 8 Development Environment）(必需选择jdk的开发環境 OpenJDK 8 Development Environment）
     .....
   
     [root@register1 bin]# yum install  java-1.8.0-Openjdk-devel.x86_64
     
     查找linux中 JDK的安装目录，以便设置JAVA_HOME的環境变量
     
      [root@register1 ~]# java -version
       java 1.8.0
      
      
      [root@register1 ~]# which java
       /usr/bin/java
      
      [root@register1 ~]# ll /usr/bin/java 
       lrwxrwxrwx. 1 root root 22 2 month   24 12:26 /usr/bin/java -> /etc/alternatives/java
       
      [root@register1 ~]# ll /etc/alternatives/java
      lrwxrwxrwx. 1 root root 73 2 month   24 12:26 /etc/alternatives/java -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java 
      
      得到JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64  (注意： 不要带 /)
      
      [root@register1]# vi /etc/profile
      
      export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64         
      export JRE_HOME=$JAVA_HOME/jre
      export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
      export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
      
      [root@register1]# source /etc/profile
      
      [root@register1]# reboot

      通过https://github.com/alibaba/nacos/releases链接可以下载Nacos的最新发行版，解压到指定目录
 
# 2 在同一虚拟机上使用不同的端口搭建Nacos集群

    在当前例子中我用了两台虚拟机
    
IP|端口|名字|
--|---|--|
192.168.33.100 |8848， 8847， 8846|nacos register1|
192.168.33.180	|3306|MySQL01|


    通常如果我们只是为了体验的话直接在本地起动3个实例就可以了，没必要真的去搞三台服务器，下面我们就以同一虚拟机上使用不同的端口搭建Nacos集群  

    在192.168.33.100 （nacos register1）将Nacos的解压包复制分成3份，建立三个目录分别是:

 * [nacos-server1目录](#1-配置nacos-server1)
 * [nacos-server2目录](#2-配置nacos-server2)
 * [nacos-server3目录](#3-配置nacos-server3)
    
 ## 1 配置nacos server1目录
 
 **Nacos server1数据库支持**
 
   **derby 切换 mysql 数据库配置**   
   
      在 192.168.33.180（MySQL01）虚拟机上设置MySQL 数据库
      
      执行nacos-mysql.sql脚本：
      
               在主机上 使用Navicat连接192.168.33.180（MySQL01）虚拟机，并创建名为 nacos_config 的数据库，在主机上上进入 nacos 安装目录 conf 文件下，找到 nacos-mysql.sql 脚本
               并用Navicat导入到 nacos_config数据库中。
          
      修改application.properties，添加mysql支持：
      
              在192.168.33.100（nacos register1）虚拟机进入nacos-server1目录的子conf目录，编辑application.properties文件，增加数据库配置

              # 指定数据源为Mysql
                spring.datasource.platform=mysql

              # 数据库实例数量
                db.num=1
                db.url.0=jdbc:mysql://192.168.33.180:3306/nacos?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
                db.user=root                    //mysql root用户名
                db.password=Gz@19731108         //mysql 数据库密码

 
 **Nacos server1集群部署搭建**
 
 
 ## 2 配置nacos server2目录
 
 ## 3 配置nacos server3目录



# 3 在不同虚拟机上使用相同的端口搭建Nacos集群

