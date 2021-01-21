#目录
  
  LVS+Keepalived的配置是跟据架构设计的不同而放在不同的地方，如果架构中有有专门的机器就放在该机器上 [例子1](https://blog.51cto.com/3241766/2094750)，[例子2](https://www.kubernetes.org.cn/6632.html),如果没设计专门的机器就放在
  应用服务器上,[例子3](https://www.kubernetes.org.cn/6632.html)

* [0 安装ipvsadm](#安0-装ipvsadm)
* [1 安装Keepalived](#1-安装Keepalived)
* [2 keepalived配置](#2-keepalived配置)
  *  [2.1 master虚拟机的keepalived配置](#master虚拟机的keepalived配置)
  *  [2.2 master2虚拟机的keepalived配置](#master2虚拟机的keepalived配置)
  *  [2.3 master3虚拟机的keepalived配置](#master3虚拟机的keepalived配置)
* [3 启动keepalived](#3-启动keepalived)
* [4 VIP查看](#4-VIP查看)

# 0 安装ipvsadm

    LVS无需安装，安装的是管理工具，第一种叫ipvsadm，第二种叫keepalive。ipvsadm是通过命令行管理，而keepalive读取配置文件管理
   
    分别在三台control plane节点(master, master2,master3)都执行操作
   
     [root@master]# yum -y install ipvsadm
     [root@master2]# yum -y install ipvsadm
     [root@master3]# yum -y install ipvsadm
     
     把ipvsadm模块加载进系统
     
     [root@master]# ipvsadm
     [root@master2]# ipvsadm
     [root@master3]# ipvsadm

# 1 安装Keepalived

     分别在三台control plane节点(master, master2,master3)都执行操作

     [root@master]# yum -y install keepalived
     [root@master2]# yum -y install keepalived
     [root@master3]# yum -y install keepalived
 
# 2 keepalived配置

## master虚拟机的keepalived配置

   首先，要查看本机的网卡名称，不同Linux系统有不同的名称，我是用 centos8 Linux系统，它的网卡名是 eth1
          [root@master]# ifconfig
              docker0:...................................
              eth0:...................................
              eth1: <BROADCAST , MULTICAST, UP, LOWER_UP>
              link/ether 00:0c:23:45:30:fc brd ff:ff:ff:ff:ff:ff
                 valid——lft forever prefered_lft forever
              inet 192.168.33.11/24 brd 192.168.33.255 scope global noprefixroute eth1   // 本机网卡eth1绑定IP（192.168.33.11）
              inet6 fe80::20c:29ff:fe29:fe45/64
                
              Io:..............................................
           
           

           [root@master]# more /etc/keepalived/keepalived.conf 
           ! Configuration File for keepalived
           
           global_defs {
              router_id master            //master虚拟机的hostname, router_id 机器标识，通常为hostname，但不一定非得是hostname。故障发生时，邮件通知会用到
           }
           vrrp_instance VI_1 {            //vrrp实例定义部分
               state MASTER                // 设置lvs的状态，MASTER和BACKUP两种，必须大写 
               interface eth1              //设置网卡号
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
           
           virtual_server 192.168.33.130 6443      //设置虚拟服务器，需要指定虚拟ip和服务端口
               delay_loop 6                        //健康检查时间间隔
               lb_algo wrr                         //负载均衡调度算法
               lb_kind DR                          //负载均衡转发规则
               persistence_timeout 50              //设置会话保持时间，对动态网页非常有用
               protocol TCP                        //指定转发协议类型，有TCP和UDP两种
               real_server 172.27.9.91 81 {        //配置服务器节点1，需要指定real server的真实IP地址和端口
               weight 1                            //设置权重，数字越大权重越高
              TCP_CHECK {              #realserver的状态监测设置部分单位秒
                 connect_timeout 10       #连接超时为10秒
                 retry 3             #重连次数
                 delay_before_retry 3        #重试间隔
                 connect_port 81         #连接端口为81，要和上面的保持一致
                 }
              }
               real_server 172.27.9.92 81 {    #配置服务器节点1，需要指定real server的真实IP地址和端口
               weight 1                  #设置权重，数字越大权重越高
               TCP_CHECK {               #realserver的状态监测设置部分单位秒
                 connect_timeout 10         #连接超时为10秒
                 retry 3               #重连次数
                 delay_before_retry 3        #重试间隔
                 connect_port 81          #连接端口为81，要和上面的保持一致
                 }
               }
          }
           
           
           
           
           
           
## master2虚拟机的keepalived配置

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

## master3虚拟机的keepalived配置
 
           [root@master3# more /etc/keepalived/keepalived.conf 
           ! Configuration File for keepalived
           
           global_defs {
              router_id master3           //master虚拟机的hostname
           }
           vrrp_instance VI_1 {
               state BACKUP                // 必需大写，表示在keepalived中
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


# 3 启动keepalived

   **master 启动keepalived**
   
   [root@master]# service keepalived start
   
   [root@master]# systemctl enable keepalived
   
   
   **master2 启动keepalived**
   
   [root@master2]# service keepalived start
   
   [root@master2]# systemctl enable keepalived
   
   
   **master3 启动keepalived**

   [root@master3]# service keepalived start
   
   [root@master3]# systemctl enable keepalived


# 4 VIP查看

    在master上查看 VIP被绑定到那一台机上
    
    [root@master]# ip a
    
         1:     docker0:...................................
         2:     eth0:...................................
         3:     eth1: <BROADCAST , MULTICAST, UP, LOWER_UP>
                  link/ether 00:0c:23:45:30:fc brd ff:ff:ff:ff:ff:ff
                      valid——lft forever prefered_lft forever
                  inet 192.168.33.11/24 brd 192.168.33.255 scope global noprefixroute eth1    // master虚拟机网卡eth1绑定物理IP（192.168.33.11）
                  inet 192.168.33.130/24 brd 192.168.33.255 scope global noprefixroute eth1   // master虚拟机网卡eth1绑定虚拟VIP（192.168.33.130），表明在集群(master,  
                                                                                                 master2,master3)中现是由master虚拟机接受负载均衡传递过来的流量
                  inet6 fe80::20c:29ff:fe29:fe45/64
                
         4:     Io:..............................................
         

   假如master虚拟机出故障了，哪VIP会绑定到那一台机上，可手工模拟一下：
   
        先关闭master虚拟机：
                
        [root@master]# init 0
        
        然后，在master2虚拟机上查看
        
        [root@master2]# ip a 
        
         1:     docker0:...................................
         2:     eth0:...................................
         3:     eth1: <BROADCAST , MULTICAST, UP, LOWER_UP>
                  link/ether 00:0c:23:45:30:fc brd ff:ff:ff:ff:ff:ff
                      valid——lft forever prefered_lft forever
                  inet 192.168.33.10/24 brd 192.168.33.255 scope global noprefixroute eth1   // master2虚拟机网卡eth1绑定物理IP（192.168.33.10）
                  inet 192.168.33.130/24 brd 192.168.33.255 scope global noprefixroute eth1   // master2虚拟机网卡eth1绑定虚拟VIP（192.168.33.130），表明在
                                                                                                 集群(master, master2,master3)中现是由master2虚拟机接受负载均
                                                                                                 衡传递过来的流量
                  inet6 fe80::20c:29ff:fe29:fe45/64
                
         4:     Io:..............................................
        
        
        
        
         

