
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
              eth1: <BROADCAST , MULTICAST, UP, LOWER_UP>
              link/ether 00:0c:23:45:30:fc brd ff:ff:ff:ff:ff:ff
                 valid——lft forever prefered_lft forever
              inet 192.168.33.11/24 brd 192.168.33.255 scope global noprefixroute eth1   // 本机网卡eth1绑定IP（192.168.33.11）
              inet6 fe80::20c:29ff:fe29:fe45/64
                
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

   **master 启动keepalived**
   
   [root@master]# service keepalived start
   [root@master]# systemctl enable keepalived
   
   
   **master2 启动keepalived **
   
   [root@master2]# service keepalived start
   [root@master2]# systemctl enable keepalived
   
   
   **master3 启动keepalived **

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
                  inet 192.168.33.11/24 brd 192.168.33.255 scope global noprefixroute eth1   // 本机网卡eth1绑定IP（192.168.33.11）
                  inet 192.168.33.130/24 brd 192.168.33.255 scope global noprefixroute eth1   // 本机网卡eth1绑定IP（192.168.33.11）
                  inet6 fe80::20c:29ff:fe29:fe45/64
                
         4:     Io:..............................................

