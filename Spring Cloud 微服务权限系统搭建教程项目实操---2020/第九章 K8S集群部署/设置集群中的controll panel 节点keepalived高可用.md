
* [1 安装Keepalived](#1-安装Keepalived)
* [2 keepalived配置](#2-keepalived配置)
  *  [2.1 master虚拟机的keepalived配置](#master虚拟机的keepalived配置)
  *  [2.2 master2虚拟机的keepalived配置](#master2虚拟机的keepalived配置)
  *  [2.3 master3虚拟机的keepalived配置](#master3虚拟机的keepalived配置)
* [3 启动keepalived](#3-启动keepalived)
* [4 VIP查看](#4-VIP查看)

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
              eth1:
                
              Io:..............................................
           
           

           [root@master]# more /etc/keepalived/keepalived.conf 
           ! Configuration File for keepalived
           
           global_defs {
              router_id master            //master虚拟机的hostname
           }
           vrrp_instance VI_1 {
               state MASTER                // 必需大写，表示在keepalived中
               interface ens160
               virtual_router_id 50
               priority 100
               advert_int 1
               authentication {
                   auth_type PASS
                   auth_pass 1111
               }
               virtual_ipaddress {
                   172.27.34.130
               }
           }
## master2虚拟机的keepalived配置

           [root@master2]# more /etc/keepalived/keepalived.conf 
           ! Configuration File for keepalived
           
           global_defs {
              router_id master2            //master虚拟机的hostname
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
                   172.27.34.130
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
                   172.27.34.130
               }
           }


# 3 启动keepalived

# 4 VIP查看
