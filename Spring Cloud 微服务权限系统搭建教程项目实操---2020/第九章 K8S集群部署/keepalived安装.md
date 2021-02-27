
**lvs01和lvs02都执行本操作, 此步骤使用LVS+Keepalived即可做到故障的自动检测及切换**


# 目录
* [1. keepalived安装](#1-keepalived安装)
  * [lvs01虚拟机keepalived安装](#lvs01虚拟机keepalived安装)
  * [lvs02虚拟机keepalived安装](#lvs02虚拟机keepalived安装)
* [2. keepalived配置](#2-keepalived配置)
  * [lvs01虚拟机keepalived配置](#lvs01虚拟机keepalived配置)
  * [lvs02虚拟机keepalived配置](#lvs02虚拟机keepalived配置)
* [3. master上去掉vip](#3-master上去掉vip)
* [4. 启动keepalived](#4-启动keepalived)
  * [lvs01虚拟机启动keepalived](#lvs01虚拟机启动keepalived)
  * [lvs02虚拟机启动keepalived](#lvs02虚拟机启动keepalived)
* [5. vip查看](#5-vip查看)


# 1 keepalived安装

## lvs01虚拟机keepalived安装
 
     [root@lvs01]# yum -y install keepalived
     
 
## lvs02虚拟机keepalived安装

    [root@lvs02]# yum -y install keepalived


# 2 keepalived配置

## lvs01虚拟机keepalived配置

     
     [root@lvs01]# vi 

     [root@lvs01]# more /etc/keepalived/keepalived.conf

     ! Configuration File for keepalived
     global_defs {
        
        router_id lvs01              #router_id 机器标识，通常为hostname，但不一定非得是hostname。故障发生时，邮件通知会用到。
     }
     
     #这个是我们在上一小结讲到的nginx检查脚本，我们保存在这个文件中（注意文件权限）
     vrrp_script chknginx {
     script "/usr/keepalived-1.2.17/bin/checknginx.sh"
     #每10秒钟，检查一次
     interval 10
     }
     
     #keepalived实例设置，是最重要的设置信息
     vrrp_instance VI_1 {            #vrrp实例定义部分
         state MASTER                #设置lvs的状态，MASTER和BACKUP两种，必须大写 
         interface eth1              #设置对外服务的接口
         virtual_router_id 100       #设置虚拟路由标示，这个标示是一个数字，同一个vrrp实例使用唯一标示 
         priority 100                #定义优先级，数字越大优先级越高，在一个vrrp——instance下，master的优先级必须大于backup
         advert_int 1                #设定master与backup负载均衡器之间同步检查的时间间隔，单位是秒
         
         #实际的eth1上的固定ip地址
         #mcast_src_ip=192.168.33.132               //当前机器lvs01的IP， 可加也可不加
         
         #验证信息，只有验证信息相同，才能被加入到一个组中
         authentication {            #设置验证类型和密码
             auth_type PASS          #主要有PASS和AH两种
             auth_pass 1111          #验证密码，同一个vrrp_instance下MASTER和BACKUP密码必须相同
         }
         virtual_ipaddress {         #设置虚拟ip地址，可以设置多个，每行一个，dev 是指定浮动IP要绑定的网卡设备号 ， 当前网卡设备号是eth1
             192.168.33.130    or  192.168.33.130 dev eth1

         }
         
         
         #设置的检查脚本
         #关联上方的“vrrp_script chknginx”
         track_script {
                    chknginx
         }
         
     }            //vrrp_instance vi_1 end
     
     
     virtual_server 192.168.33.130 81 {   #设置虚拟服务器，需要指定虚拟ip和服务端口 , 在本例中下行连接的是NGINX 服务器，该nginx服务器连接的端口是已从80 改为 81
         delay_loop 6                     #健康检查时间间隔
         lb_algo wrr                      #负载均衡调度算法
         lb_kind DR                       #负载均衡转发规则
         #persistence_timeout 50          #设置会话保持时间，对动态网页非常有用
         protocol TCP                     #指定转发协议类型，有TCP和UDP两种
         
         real_server 192.168.33.142 81 {    #配置服务器节点1，需要指定real server的真实IP地址和端口,在本例中该real server指向 nginx01虚拟机 ，可下行指向任何类型server，在本例中下行连
                                            #接的是NGINX 服务器，该nginx服务器连接的端口是已从80 改为 81
         weight 10                          #设置权重，数字越大权重越高
         TCP_CHECK {                        #realserver的状态监测设置部分单位秒
            connect_timeout 10              #连接超时为10秒
            retry 3                         #重连次数
            delay_before_retry 3            #重试间隔
            connect_port 81                 #连接端口为81，要和上面的保持一致
            }
         }
         
         real_server 192.168.33.143 81 {    #配置服务器节点1，需要指定real server的真实IP地址和端口, 在本例中该real server指向 nginx02虚拟机 ， 可下行指向任何类型server
         weight 10                          #设置权重，数字越大权重越高
         TCP_CHECK {                        #realserver的状态监测设置部分单位秒
            connect_timeout 10              #连接超时为10秒
            retry 3                         #重连次数
            delay_before_retry 3            #重试间隔
            connect_port 81                 #连接端口为81，要和上面的保持一致
            }
         }
         
         real_server 192.168.33.144 81 {  #配置服务器节点1，需要指定real server的真实IP地址和端口, 在本例中该real server指向 nginx03虚拟机 ， 可下行指向任何类型server
         weight 10                        #设置权重，数字越大权重越高
         TCP_CHECK {                      #realserver的状态监测设置部分单位秒
            connect_timeout 10            #连接超时为10秒
            retry 3                       #重连次数
            delay_before_retry 3          #重试间隔
            connect_port 81               #连接端口为81，要和上面的保持一致
            }
         }
     }

## lvs02虚拟机keepalived配置


     [root@lvs02]# more /etc/keepalived/keepalived.conf

     ! Configuration File for keepalived
     global_defs {
        router_id lvs02              #router_id 机器标识，通常为hostname，但不一定非得是hostname。故障发生时，邮件通知会用到。
     }
     vrrp_instance VI_1 {            #vrrp实例定义部分
         state BACKUP               #设置lvs的状态，MASTER和BACKUP两种，必须大写 
         interface eth1              #设置对外服务的接口
         virtual_router_id 100       #设置虚拟路由标示，这个标示是一个数字，同一个vrrp实例使用唯一标示 
         priority 90                #定义优先级，数字越大优先级越高，在一个vrrp——instance下，master的优先级必须大于backup
         advert_int 1                #设定master与backup负载均衡器之间同步检查的时间间隔，单位是秒
         authentication {            #设置验证类型和密码
             auth_type PASS          #主要有PASS和AH两种
             auth_pass 1111          #验证密码，同一个vrrp_instance下MASTER和BACKUP密码必须相同
         }
         virtual_ipaddress {         #设置虚拟ip地址，可以设置多个，每行一个
             192.168.33.130
         }
     }
     virtual_server 192.168.33.130 81 {   #设置虚拟服务器，需要指定虚拟ip和服务端口，在本例中下行连接的是NGINX 服务器，该nginx服务器连接的端口是已从80 改为 81
         delay_loop 6                     #健康检查时间间隔
         lb_algo wrr                      #负载均衡调度算法
         lb_kind DR                       #负载均衡转发规则
         #persistence_timeout 50          #设置会话保持时间，对动态网页非常有用
         protocol TCP                     #指定转发协议类型，有TCP和UDP两种
         
         real_server 192.168.33.142 81 {    #配置服务器节点1，需要指定real server的真实IP地址和端口,该real server指向 nginx01虚拟机 
         weight 10                          #设置权重，数字越大权重越高
         TCP_CHECK {                        #realserver的状态监测设置部分单位秒
            connect_timeout 10              #连接超时为10秒
            retry 3                         #重连次数
            delay_before_retry 3            #重试间隔
            connect_port 81                 #连接端口为81，要和上面的保持一致
            }
         }
         real_server 192.168.33.143 81 {    #配置服务器节点1，需要指定real server的真实IP地址和端口, 该real server指向 nginx02虚拟机 
         weight 10                          #设置权重，数字越大权重越高
         TCP_CHECK {                        #realserver的状态监测设置部分单位秒
            connect_timeout 10              #连接超时为10秒
            retry 3                         #重连次数
            delay_before_retry 3            #重试间隔
            connect_port 81                 #连接端口为81，要和上面的保持一致
            }
         }
         real_server 192.168.33.144 81 {  #配置服务器节点1，需要指定real server的真实IP地址和端口, 该real server指向 nginx03虚拟机 
         weight 10                        #设置权重，数字越大权重越高
         TCP_CHECK {                      #realserver的状态监测设置部分单位秒
            connect_timeout 10            #连接超时为10秒
            retry 3                       #重连次数
            delay_before_retry 3          #重试间隔
            connect_port 81               #连接端口为81，要和上面的保持一致
            }
         }
     }

# 3 master上去掉vip

  [root@master]# ifconfig eth1:2 192.168.33.130 netmask 255.255.255.0 down
  
  master上去掉初始化使用的ip 192.168.33.130



# 4 启动keepalived

 lvs01和lvs02都启动keepalived并设置为开机启动
 
## lvs01虚拟机启动keepalived

    [root@lvs01]# service keepalived start
    [root@lvs01]# systemctl enable keepalived

## lvs02虚拟机启动keepalived

    [root@lvs02]# service keepalived start
    [root@lvs02]# systemctl enable keepalived


# 5 vip查看

    [root@lvs01]# ip a
    
 <a href="https://sm.ms/image/YaIUHmMOXc3egBD" target="_blank"><img src="https://i.loli.net/2020/03/09/YaIUHmMOXc3egBD.png" alt="image-20200309164434474.png"></a>
