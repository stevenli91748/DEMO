# 目录
* [1 配置JAVA_HOME环境变量, 下载安装Nacos](#1-配置JAVA_HOME环境变量)
* [2 在同一虚拟机上使用不同的端口搭建Nacos集群](#2-在同一虚拟机上使用不同的端口搭建Nacos集群)
* [3 在不同虚拟机上使用相同的端口搭建Nacos集群](#3-在不同虚拟机上使用相同的端口搭建Nacos集群)

# 1 配置JAVA_HOME环境变量
   
    每台虚拟机都要配置JAVA_HOME环境变量，不配置会导致无法运行Nacos
  
    假如没有设置JAVA_HOME环境变量， 就启动 Nacos , 会出现如下错误：
   
    [root@localhost bin]# bash startup.sh -m standalone

     which: no javac in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
     readlink:  miss operand 
     Try 'readlink --help' for more information.
     dirname:  miss operand 
     Try 'dirname --help' for more information.
     ERROR: Please set the JAVA_HOME variable in your environment, We need java(x64)! jdk8 or later is better! !!
     
     
     [root@localhost bin]# yum search jdk  //查找在yum中的JDK version
     ......
     java-1.8.0-Openjdk.x86_64      // 这是OpenJDK 8 Runtime Environment
     .....
     java-1.8.0-Openjdk-devel.x86_64  // 这是 jdk的开发環境（OpenJDK 8 Development Environment）(必需选择jdk的开发環境 OpenJDK 8 Development Environment）
     .....
   
     [root@localhost bin]# yum install  java-1.8.0-Openjdk-devel.x86_64
     
     查找linux中 JDK的安装目录，以便设置JAVA_HOME的環境变量
     
      [root@localhost ~]# java -version
       java 1.8.0
      
      
      [root@localhost ~]# which java
       /usr/bin/java
      
      [root@localhost ~]# ll /usr/bin/java 
       lrwxrwxrwx. 1 root root 22 2 month   24 12:26 /usr/bin/java -> /etc/alternatives/java
       
      [root@localhost ~]# ll /etc/alternatives/java
      lrwxrwxrwx. 1 root root 73 2 month   24 12:26 /etc/alternatives/java -> /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64/jre/bin/java 
      
      得到JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64  (注意： 不要带 /)
      
      [root@centos7-2 nacos]# vi /etc/profile
      
      export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk-1.8.0.242.b08-0.el7_7.x86_64         
      export JRE_HOME=$JAVA_HOME/jre
      export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH
      export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
      
      [root@localhost ~]# source /etc/profile
      
      [root@localhost ~]# reboot

      通过https://github.com/alibaba/nacos/releases链接可以下载Nacos的最新发行版，解压到指定目录
 
# 2 在同一虚拟机上使用不同的端口搭建Nacos集群

    通常如果我们只是为了体验的话直接在本地起动3个实例就可以了，没必要真的去搞三台服务器，下面我们就以同一虚拟机上使用不同的端口搭建Nacos集群  

    将Nacos的解压包复制分成3份，建立三个目录分别是:

    [nacos-server1](#1-配置nacos-server1)
    [nacos-server2](#2-配置nacos-server2)
    [nacos-server3](#3-配置nacos-server3)
    
 ## 1 配置nacos server1
   
 ## 2 配置nacos server2
 
 ## 3 配置nacos server3



# 3 在不同虚拟机上使用相同的端口搭建Nacos集群

