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

       -A –add-service 在内核的虚拟服务器表中添加一条新的虚拟服务器记录。也就是增加一台新的虚拟服务器。
       -E –edit-service 编辑内核虚拟服务器表中的一条虚拟服务器记录。
       -D –delete-service 删除内核虚拟服务器表中的一条虚拟服务器记录。
       -C –clear 清除内核虚拟服务器表中的所有记录。
       -R –restore 恢复虚拟服务器规则
       -S –save 保存虚拟服务器规则，输出为-R 选项可读的格式
       -a –add-server 在内核虚拟服务器表的一条记录里添加一条新的真实服务器记录。也就是在一个虚拟服务器中增加一台新的真实服务器
       -e –edit-server 编辑一条虚拟服务器记录中的某条真实服务器记录
       -d –delete-server 删除一条虚拟服务器记录中的某条真实服务器记录
       -L –list 显示内核虚拟服务器表
       -Z –zero 虚拟服务表计数器清零（清空当前的连接数量等）
       –set tcp tcpfin udp 设置连接超时值
       –start-daemon 启动同步守护进程。他后面可以是master 或backup，用来说明LVS Router 是master 或是backup。在这个功能上也可以采用keepalived 的VRRP 功能。
       –stop-daemon 停止同步守护进程
       -t –tcp-service service-address 说明虚拟服务器提供的是tcp 的服务[vip:port] or [real-server-ip:port]
       -u –udp-service service-address 说明虚拟服务器提供的是udp 的服务[vip:port] or [real-server-ip:port]
       -f –fwmark-service fwmark 说明是经过iptables 标记过的服务类型。
       -s –scheduler scheduler 使用的调度算法，选项：rr|wrr|lc|wlc|lblc|lblcr|dh|sh|sed|nq, 默认的调度算法是： wlc.
       -p –persistent [timeout] 持久稳固的服务。这个选项的意思是来自同一个客户的多次请求，将被同一台真实的服务器处理。timeout 的默认值为300 秒。
       -M –netmask netmask persistent granularity mask
       -r –real-server server-address 真实的服务器[Real-Server:port]
       -g –gatewaying 指定LVS 的工作模式为直接路由模式DR模式（也是LVS默认的模式）
       -i –ipip 指定LVS 的工作模式为隧道模式
       -m –masquerading 指定LVS 的工作模式为NAT 模式
       -w –weight weight 真实服务器的权值
       –mcast-interface interface 指定组播的同步接口
       –connection 显示LVS 目前的连接 如：ipvsadm -L -c
       –timeout 显示tcp tcpfin udp 的timeout 值 如：ipvsadm -L –timeout
       –daemon 显示同步守护进程状态
       –stats 显示统计信息
       –rate 显示速率信息
       –sort 对虚拟服务器和真实服务器排序输出
       –numeric -n 输出IP 地址和端口的数字形式


## lvs01负载均衡服务器配置
    
    [root@lvs02]# ipvsadm -A -t 192.168.33.133:81 -s rr                    // lvs02的IP地址是 192.168.33.133  设置端口为81是为了跟下行连接的NGINX服务器设置的端口对上
                                                                           // 在本例中下行连接的是NGINX 服务器，该nginx服务器连接的端口是已从80 改为 81
    [root@lvs02]# ipvsadm -a -t 192.168.33.133:81 -r 192.168.33.142 -m     // nginx01服务器的IP地址是 192.168.33.142
    [root@lvs02]# ipvsadm -a -t 192.168.33.133:81 -r 192.168.33.143 -m     // nginx02服务器的IP地址是 192.168.33.143
    [root@lvs02]# ipvsadm -a -t 192.168.33.133:81 -r 192.168.33.144 -m     // nginx03服务器的IP地址是 192.168.33.144
    [root@lvs02]# ipvsadm -S

      -A –add-service 在内核的虚拟服务器表中添加一条新的虚拟服务器记录。也就是增加一台新的虚拟服务器。
      -E –edit-service 编辑内核虚拟服务器表中的一条虚拟服务器记录。
      -D –delete-service 删除内核虚拟服务器表中的一条虚拟服务器记录。
      -C –clear 清除内核虚拟服务器表中的所有记录。
      -R –restore 恢复虚拟服务器规则
      -S –save 保存虚拟服务器规则，输出为-R 选项可读的格式
      -a –add-server 在内核虚拟服务器表的一条记录里添加一条新的真实服务器记录。也就是在一个虚拟服务器中增加一台新的真实服务器
      -e –edit-server 编辑一条虚拟服务器记录中的某条真实服务器记录
      -d –delete-server 删除一条虚拟服务器记录中的某条真实服务器记录
      -L –list 显示内核虚拟服务器表
      -Z –zero 虚拟服务表计数器清零（清空当前的连接数量等）
      –set tcp tcpfin udp 设置连接超时值
      –start-daemon 启动同步守护进程。他后面可以是master 或backup，用来说明LVS Router 是master 或是backup。在这个功能上也可以采用keepalived 的VRRP 功能。
      –stop-daemon 停止同步守护进程
      -t –tcp-service service-address 说明虚拟服务器提供的是tcp 的服务[vip:port] or [real-server-ip:port]
      -u –udp-service service-address 说明虚拟服务器提供的是udp 的服务[vip:port] or [real-server-ip:port]
      -f –fwmark-service fwmark 说明是经过iptables 标记过的服务类型。
      -s –scheduler scheduler 使用的调度算法，选项：rr|wrr|lc|wlc|lblc|lblcr|dh|sh|sed|nq, 默认的调度算法是： wlc.
      -p –persistent [timeout] 持久稳固的服务。这个选项的意思是来自同一个客户的多次请求，将被同一台真实的服务器处理。timeout 的默认值为300 秒。
      -M –netmask netmask persistent granularity mask
      -r –real-server server-address 真实的服务器[Real-Server:port]
      -g –gatewaying 指定LVS 的工作模式为直接路由模式DR模式（也是LVS默认的模式）
      -i –ipip 指定LVS 的工作模式为隧道模式
      -m –masquerading 指定LVS 的工作模式为NAT 模式
      -w –weight weight 真实服务器的权值
      –mcast-interface interface 指定组播的同步接口
      –connection 显示LVS 目前的连接 如：ipvsadm -L -c
      –timeout 显示tcp tcpfin udp 的timeout 值 如：ipvsadm -L –timeout
      –daemon 显示同步守护进程状态
      –stats 显示统计信息
      –rate 显示速率信息
      –sort 对虚拟服务器和真实服务器排序输出
      –numeric -n 输出IP 地址和端口的数字形式


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




     
  
