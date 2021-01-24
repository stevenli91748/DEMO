# 目录
  
  LVS+Keepalived的配置是跟据架构设计的不同而放在不同的地方，如果架构中有有专门的机器就放在该机器上 [例子1](https://blog.51cto.com/3241766/2094750)，[例子2](https://www.kubernetes.org.cn/6632.html),如果没设计专门的机器就放在
  应用服务器上,[例子3](https://www.kubernetes.org.cn/6632.html)

* [0 安装ipvsadm](#安0-装ipvsadm)
* [1 安装Keepalived](#1-安装Keepalived)
* [2 keepalived配置](#2-keepalived配置)
  *  [2.1 lvs01虚拟机的keepalived配置](#lvs01虚拟机的keepalived配置)
  *  [2.2 lvs02虚拟机的keepalived配置](#lvs022虚拟机的keepalived配置)
* [3 启动keepalived](#3-启动keepalived)
* [4 VIP查看](#4-VIP查看)
* [5. 安装Nginx](#5-安装Nginx)
  * [第一种方法---编译安装nginx来定制自己的模块](#第一种方法---编译安装nginx来定制自己的模块)
  * [第二种方法---yum安装nginx](#第二种方法---yum安装nginx)
    
# 0 安装ipvsadm

   关闭防火墙:
   关闭两台lvs(lvs01,lvs02) 服务器和三台nginx(nginx01,nginx02,nginx03)服务器防火墙
     
     关闭lvs01防火墙
     [root@lvs01]# firewall-cmd-state
     [root@lvs01]# systemctl stop firewalld.service
     [root@lvs01]# systemctl disable firewalld.service     
     
     关闭lvs02防火墙
     [root@lvs02]# firewall-cmd-state
     [root@lvs02]# systemctl stop firewalld.service
     [root@lvs02]# systemctl disable firewalld.service     
     
     关闭nginx01防火墙
     [root@nginx01]# firewall-cmd-state
     [root@nginx01]# systemctl stop firewalld.service
     [root@nginx01]# systemctl disable firewalld.service     
     
     关闭nginx02防火墙
     [root@nginx02]# firewall-cmd-state
     [root@nginx02]# systemctl stop firewalld.service
     [root@nginx02]# systemctl disable firewalld.service     

     关闭nginx03防火墙
     [root@nginx03]# firewall-cmd-state
     [root@nginx03]# systemctl stop firewalld.service
     [root@nginx03]# systemctl disable firewalld.service     

   关闭防selinux:
      
   关闭lvs(lvs01,lvs02) 和 nginx(nginx01,nginx02,nginx03) 服务器的selinux，修改每一台的/etc/selinux/config，将SELINUX由enforcing设置为disabled，重启服务器。
    
    [root@lvs01]# sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
    [root@lvs02]# sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
    [root@nginx01]# sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
    [root@nginx02]# sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
    [root@nginx03]# sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
       

   LVS无需安装，安装的是管理工具，第一种叫ipvsadm，第二种叫keepalive。ipvsadm是通过命令行管理，而keepalive读取配置文件管理
   
   安装ipvsadm
   
   分别在二台LVS节点(lvs01,lvs02)都执行操作
   
     [root@lvs01]# yum -y install ipvsadm
     [root@lvs02]# yum -y install ipvsadm

     
     把ipvsadm模块加载进系统
     
     [root@lvs01]# ipvsadm
     [root@lvs02]# ipvsadm

     [root@lvs01]#lsmod | grep ip_vs
     [root@lvs02]#lsmod | grep ip_vs

# 1 安装Keepalived

     分别在二台LVS节点(lvs01,lvs02)都执行操作

     [root@lvs01]# yum -y install keepalived
     [root@lvs02]# yum -y install keepalived

 
# 2 keepalived配置

## lvs01虚拟机的keepalived配置

   首先，要查看本机的网卡名称，不同Linux系统有不同的名称，我是用 centos8 Linux系统，它的网卡名是 eth1
   

   
              [root@lvs01]# ifconfig
              docker0:...................................
              eth0:...................................
              eth1: <BROADCAST , MULTICAST, UP, LOWER_UP>
              link/ether 00:0c:23:45:30:fc brd ff:ff:ff:ff:ff:ff
                 valid——lft forever prefered_lft forever
              inet 192.168.33.132/24 brd 192.168.33.255 scope global noprefixroute eth1   // lvs01虚拟机网卡eth1绑定物理IP（192.168.33.132）
              inet6 fe80::20c:29ff:fe29:fe45/64
                
              Io:..............................................
           
           

           [root@lvs01]# more /etc/keepalived/keepalived.conf 
           ! Configuration File for keepalived
           
           global_defs {
              router_id lvs01             //lvs01虚拟机的hostname, router_id 机器标识，通常为hostname，但不一定非得是hostname。故障发生时，邮件通知会用到
           }
           vrrp_instance VI_1 {            //vrrp实例定义部分
               state MASTER                // 设置lvs的状态，MASTER和BACKUP两种，必须大写 
               interface eth1              //设置lvs01网卡名
               virtual_router_id 50        //设置虚拟路由标示，这个标示是一个数字，同一个vrrp实例使用唯一标示 
               priority 100                //定义优先级，数字越大优先级越高，在一个vrrp——instance下，master的优先级必须大于backup
               advert_int 1                //设定master与backup负载均衡器之间同步检查的时间间隔，单位是秒
               authentication {            //设置验证类型和密码
                   auth_type PASS          //主要有PASS和AH两种
                   auth_pass 1111          //验证密码，同一个vrrp_instance下MASTER和BACKUP密码必须相同
               }
               virtual_ipaddress {         //设置虚拟ip地址，可以设置多个，每行一个
                  192.168.33.130
               }
           }
           
           virtual_server 192.168.33.130 6443{      //设置虚拟服务器，需要指定虚拟ip和服务端口
               delay_loop 6                        //健康检查时间间隔
               lb_algo wrr                         //负载均衡调度算法
               lb_kind DR                          //负载均衡转发规则
               persistence_timeout 50              //设置会话保持时间，对动态网页非常有用
               protocol TCP                        //指定转发协议类型，有TCP和UDP两种
               
               //配置下行服务器 master虚拟机
               real_server 192.168.33.11 6443 {    //配置服务器节点1(master虚拟机)，需要指定real server(master虚拟机)的真实IP地址和端口
               weight 1                            //设置权重，数字越大权重越高
               TCP_CHECK {                          //realserver的状态监测设置部分单位秒
                 connect_timeout 10                //连接超时为10秒
                 retry 3                           //重连次数
                 delay_before_retry 3              //重试间隔
                 connect_port 6443                 //连接端口为6443，要和上面的保持一致
                 }
               }
              
               
               //配置下行服务器 master2虚拟机
               real_server 192.168.33.10 6443 {    //配置服务器节点2(master2虚拟机)，需要指定real server(master2虚拟机)的真实IP地址和端口
               weight 1                            //设置权重，数字越大权重越高
               TCP_CHECK {                         //realserver的状态监测设置部分单位秒
                 connect_timeout 10                //连接超时为10秒
                 retry 3                           //重连次数
                 delay_before_retry 3              //重试间隔
                 connect_port 6443                 //连接端口为81，要和上面的保持一致
                 }
               }
               
               //配置下行服务器 master3虚拟机
               real_server 192.168.33.9 6443 {    //配置服务器节点1(master3虚拟机)，需要指定real server(master3虚拟机)的真实IP地址和端口
               weight 1                            //设置权重，数字越大权重越高
               TCP_CHECK {                          //realserver的状态监测设置部分单位秒
                 connect_timeout 10                //连接超时为10秒
                 retry 3                           //重连次数
                 delay_before_retry 3              //重试间隔
                 connect_port 6443                 //连接端口为6443，要和上面的保持一致
                 }
               }

               
          }                                        // virtual_server 192.168.33.130 6443{
           
           
           
           
           
           
## lvs02虚拟机的keepalived配置

           [root@master2]# more /etc/keepalived/keepalived.conf 
           ! Configuration File for keepalived
           
           global_defs {
              router_id master2            //master虚拟机的hostname，router_id 机器标识，通常为hostname，但不一定非得是hostname。故障发生时，邮件通知会用到
          }
           vrrp_instance VI_1 {
               state BACKUP                // 设置lvs的状态，MASTER和BACKUP两种，必须大写 
               interface eth1
               virtual_router_id 50
               priority 90
               advert_int 1
               authentication {
                   auth_type PASS
                   auth_pass 1111
               }
               virtual_ipaddress {
                  192.168.33.130
               }
           }

           virtual_server 192.168.33.130 6443{      //设置虚拟服务器，需要指定虚拟ip和服务端口
               delay_loop 6                        //健康检查时间间隔
               lb_algo wrr                         //负载均衡调度算法
               lb_kind DR                          //负载均衡转发规则
               persistence_timeout 50              //设置会话保持时间，对动态网页非常有用
               protocol TCP                        //指定转发协议类型，有TCP和UDP两种
               
               //配置下行服务器 master虚拟机
               real_server 192.168.33.11 6443 {    //配置服务器节点1(master虚拟机)，需要指定real server(master虚拟机)的真实IP地址和端口
               weight 1                            //设置权重，数字越大权重越高
               TCP_CHECK {                          //realserver的状态监测设置部分单位秒
                 connect_timeout 10                //连接超时为10秒
                 retry 3                           //重连次数
                 delay_before_retry 3              //重试间隔
                 connect_port 6443                 //连接端口为6443，要和上面的保持一致
                 }
               }
              
               
               //配置下行服务器 master2虚拟机
               real_server 192.168.33.10 6443 {    //配置服务器节点2(master2虚拟机)，需要指定real server(master2虚拟机)的真实IP地址和端口
               weight 1                            //设置权重，数字越大权重越高
               TCP_CHECK {                         //realserver的状态监测设置部分单位秒
                 connect_timeout 10                //连接超时为10秒
                 retry 3                           //重连次数
                 delay_before_retry 3              //重试间隔
                 connect_port 6443                 //连接端口为81，要和上面的保持一致
                 }
               }
               
               //配置下行服务器 master3虚拟机
               real_server 192.168.33.9 6443 {    //配置服务器节点1(master3虚拟机)，需要指定real server(master3虚拟机)的真实IP地址和端口
               weight 1                            //设置权重，数字越大权重越高
               TCP_CHECK {                          //realserver的状态监测设置部分单位秒
                 connect_timeout 10                //连接超时为10秒
                 retry 3                           //重连次数
                 delay_before_retry 3              //重试间隔
                 connect_port 6443                 //连接端口为6443，要和上面的保持一致
                 }
               }
              
          }                                        // virtual_server 192.168.33.130 6443{



# 3 启动keepalived

   **lvs01启动keepalived**
   
   [root@lvs01]# service keepalived start
   
   [root@lvs01]# systemctl enable keepalived
   
   
   **lvs02 启动keepalived**
   
   [root@lvs02]# service keepalived start
   
   [root@lvs02]# systemctl enable keepalived
   
   


# 4 VIP查看

    在lvs01上查看 VIP被绑定到那一台机上
    
    [root@lvs01]# ip a
    
         1:     docker0:...................................
         2:     eth0:...................................
         3:     eth1: <BROADCAST , MULTICAST, UP, LOWER_UP>
                  link/ether 00:0c:23:45:30:fc brd ff:ff:ff:ff:ff:ff
                      valid——lft forever prefered_lft forever
                  inet 192.168.33.132/24 brd 192.168.33.255 scope global noprefixroute eth1    // lvs01虚拟机网卡eth1绑定物理IP（192.168.33.132）
                  inet 192.168.33.130/24 brd 192.168.33.255 scope global noprefixroute eth1   //  lvs01虚拟机网卡eth1绑定虚拟VIP（192.168.33.130），表明在集群(lvs01,  
                                                                                                 lvs02)中现是由lvs01虚拟机负载均衡传递过来的流量
                  inet6 fe80::20c:29ff:fe29:fe45/64
                
         4:     Io:..............................................
         

   假如master虚拟机出故障了，哪VIP会绑定到那一台机上，可手工模拟一下：
   
        先关闭lvs01虚拟机：
                
        [root@lvs01]# init 0
        
        然后，在lvs02虚拟机上查看
        
        [root@lvs02]# ip a 
        
         1:     docker0:...................................
         2:     eth0:...................................
         3:     eth1: <BROADCAST , MULTICAST, UP, LOWER_UP>
                  link/ether 00:0c:23:45:30:fc brd ff:ff:ff:ff:ff:ff
                      valid——lft forever prefered_lft forever
                  inet 192.168.33.133/24 brd 192.168.33.255 scope global noprefixroute eth1   // lvs02虚拟机网卡eth1绑定物理IP（192.168.33.133）
                  inet 192.168.33.130/24 brd 192.168.33.255 scope global noprefixroute eth1   // lvs02虚拟机网卡eth1绑定虚拟VIP（192.168.33.130），表明在
                                                                                                 集群(lvs01, lvs02)中现是由lvs02虚拟机负载均衡传递过来的流量                                                                                               
                  inet6 fe80::20c:29ff:fe29:fe45/64
                
         4:     Io:..............................................
        
        
# 5 安装Nginx     
   
   * [Centos7.3安装nginx](https://blog.51cto.com/3241766/2094315)
   * [如何在 CentOS 8 上安装 Nginx](https://www.jianshu.com/p/9b2dd37a5af9)
   * [nginx服务器安装及配置文件详解](https://www.jianshu.com/p/57eacdaf7392)
 
    
    使用编译安装nginx可自定制自己的模块，yum安装rpm包会比编译安装简单很多，默认会安装许多模块，但缺点是如果你想以后安装第三方模块那就没办法了    
        
### 第一种方法   编译安装nginx来定制自己的模块

   在nginx集群中的nginx01,nginx02,nginx03配置
    
   gcc安装:
   
    [root@nginx01]# yum -y install gcc-c++
    [root@nginx02]# yum -y install gcc-c++
    [root@nginx03]# yum -y install gcc-c++
    
   pcre安装

    [root@nginx01]# yum -y install pcre pcre-devel
    [root@nginx02]# yum -y install pcre pcre-devel
    [root@nginx03]# yum -y install pcre pcre-devel

   zlib安装

    [root@nginx01]# yum -y install zlib zlib-devel
    [root@nginx02]# yum -y install zlib zlib-devel
    [root@nginx03]# yum -y install zlib zlib-devel


   OpenSSL安装
    
    [root@nginx01]# yum -y install openssl openssl-devel
    [root@nginx02]# yum -y install openssl openssl-devel
    [root@nginx03]# yum -y install openssl openssl-devel




### 第二种方法   yum安装nginx 
        
        
   在nginx集群中的nginx01,nginx02,nginx03配置
   
   从CentOS 8开始，Nginx软件包在默认的CentOS存储库中可用
   
    [root@nginx01]# yum -y install nginx
    [root@nginx02]# yum -y install nginx
    [root@nginx03]# yum -y install nginx
    
    [root@nginx01]# systemctl enable nginx
    [root@nginx02]# systemctl enable nginx
    [root@nginx03]# systemctl enable nginx
    
    [root@nginx01]# systemctl start nginx
    [root@nginx02]# systemctl start nginx
    [root@nginx03]# systemctl start nginx
    
    [root@nginx01]# systemctl status nginx
    [root@nginx02]# systemctl status nginx
    [root@nginx03]# systemctl status nginx
    
    显示如下：
    
      nginx.service - The nginx HTTP and reverse proxy server
      Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
      Active: active (running) since Sun 2019-10-06 18:35:55 UTC; 17min ago
      ....
    
    
    
      
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
  
