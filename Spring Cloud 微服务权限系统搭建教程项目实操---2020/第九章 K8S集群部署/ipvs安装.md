**lvs01和lvs02都执行本操作**

* [1. 安装ipvs](#1-安装ipvs)
  * [lvs01虚拟机安装ipvs](#lvs01虚拟机安装ipvs)
  * [lvs02虚拟机安装ipvs](#lvs01虚拟机安装ipvs)
* [2. 加载ipvsadm模块](#2-加载ipvsadm模块)
  * [lvs01虚拟机加载ipvsadm模块](#lvs01虚拟机加载ipvsadm模块) 
  * [lvs02虚拟机加载ipvsadm模块](#lvs02虚拟机加载ipvsadm模块) 
* [3. LVS负载均衡服务器配置](#LVS负载均衡服务器配置)
  * [lvs01负载均衡服务器配置](#lvs01负载均衡服务器配置)
  * [lvs02负载均衡服务器配置](#lvs02负载均衡服务器配置)  
* [4. 测试验证基于LVS的负载均衡](#测试验证基于LVS的负载均衡)
# 1 安装ipvs

    LVS无需安装，安装的是管理工具，第一种叫ipvsadm，第二种叫keepalive。ipvsadm是通过命令行管理，而keepalive读取配置文件管理。
  
## lvs01虚拟机安装ipvs

    [root@lvs01]# yum -y install ipvsadm
   
## lvs02虚拟机安装ipvs

    [root@lvs02]# yum -y install ipvsadm

# 2 加载ipvsadm模块

    把ipvsadm模块加载进系统

## lvs01虚拟机加载ipvsadm模块

    [root@lvs01]# ipvsadm
    [root@lvs01]# lsmod | grep ip_vs
    
## lvs02虚拟机加载ipvsadm模块
    
    [root@lvs02]# ipvsadm
    [root@lvs02]# lsmod | grep ip_vs

# LVS负载均衡服务器配置

## lvs01负载均衡服务器配置

    [root@lvs01]# ipvsadm -A -t 192.168.33.132:81 -s rr                    // lvs01的IP地址是 192.168.33.132  设置端口为81是为了跟下行连接的NGINX服务器设置的端口对上，
                                                                           // 在本例中下行连接的是NGINX 服务器，该nginx服务器连接的端口是已从80 改为 81
    [root@lvs01]# ipvsadm -a -t 192.168.33.132:81 -r 192.168.33.142 -m     // nginx01服务器的IP地址是 192.168.33.142
    [root@lvs01]# ipvsadm -a -t 192.168.33.132:81 -r 192.168.33.143 -m     // nginx02服务器的IP地址是 192.168.33.143
    [root@lvs01]# ipvsadm -a -t 192.168.33.132:81 -r 192.168.33.144 -m     // nginx03服务器的IP地址是 192.168.33.144
    [root@lvs01]# ipvsadm -S

    -A   //添加虚拟服务
    -a   //添加一个真实的主机到虚拟服务
    -S   //保存
    -s   //选择调度方法
    -rr  //轮训调度
    -m   //网络地址转换NAT

## lvs01负载均衡服务器配置
    
    [root@lvs02]# ipvsadm -A -t 192.168.33.133:81 -s rr                    // lvs02的IP地址是 192.168.33.133  设置端口为81是为了跟下行连接的NGINX服务器设置的端口对上
                                                                           // 在本例中下行连接的是NGINX 服务器，该nginx服务器连接的端口是已从80 改为 81
    [root@lvs02]# ipvsadm -a -t 192.168.33.133:81 -r 192.168.33.142 -m     // nginx01服务器的IP地址是 192.168.33.142
    [root@lvs02]# ipvsadm -a -t 192.168.33.133:81 -r 192.168.33.143 -m     // nginx02服务器的IP地址是 192.168.33.143
    [root@lvs02]# ipvsadm -a -t 192.168.33.133:81 -r 192.168.33.144 -m     // nginx03服务器的IP地址是 192.168.33.144
    [root@lvs02]# ipvsadm -S

    -A   //添加虚拟服务
    -a   //添加一个真实的主机到虚拟服务
    -S   //保存
    -s   //选择调度方法
    -rr  //轮训调度
    -m   //网络地址转换NAT

# 测试验证基于LVS的负载均衡

     此步骤暂不涉及Keepalived，因此是无法故障自动切换的
     
     1. 在lvs01虚拟机上测试：
     
           [root@lvs01]# curl 192.168.33.132:81
           hello nginx03

           [root@lvs01]# curl 192.168.33.132:81
           hello nginx01

           [root@lvs01]# curl 192.168.33.132:81
           hello nginx02

           [root@lvs01]# curl 192.168.33.132:81
           hello nginx01

           [root@lvs01]# curl 192.168.33.132:81
           hello nginx02


     2. 在lvs01虚拟机上测试：
     
           [root@lvs02]# curl 192.168.33.133:81
           hello nginx01

           [root@lvs02]# curl 192.168.33.133:81
           hello nginx03

           [root@lvs02]# curl 192.168.33.133:81
           hello nginx01

           [root@lvs02]# curl 192.168.33.133:81
           hello nginx02

           [root@lvs02]# curl 192.168.33.133:81
           hello nginx01




     
  
