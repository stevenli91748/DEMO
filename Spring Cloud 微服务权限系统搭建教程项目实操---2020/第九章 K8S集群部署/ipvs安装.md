**lvs01和lvs02都执行本操作**

* [1. 安装ipvs](#1-安装ipvs)
  * [lvs01虚拟机安装ipvs](#lvs01虚拟机安装ipvs)
  * [lvs02虚拟机安装ipvs](#lvs01虚拟机安装ipvs)
* [2. 加载ipvsadm模块](#2-加载ipvsadm模块)
  * [lvs01虚拟机加载ipvsadm模块](#lvs01虚拟机加载ipvsadm模块) 
  * [lvs02虚拟机加载ipvsadm模块](#lvs02虚拟机加载ipvsadm模块) 
# 1 安装ipvs

    LVS无需安装，安装的是管理工具，第一种叫ipvsadm，第二种叫keepalive。ipvsadm是通过命令行管理，而keepalive读取配置文件管理。
  
## lvs01虚拟机安装ipvs

   [root@lvs01]# yum -y install ipvsadm
   
## lvs02虚拟机安装ipvs

# 2 加载ipvsadm模块

    把ipvsadm模块加载进系统

## lvs01虚拟机加载ipvsadm模块

    [root@lvs01]# ipvsadm
    [root@lvs01]# lsmod | grep ip_vs
    
## lvs02虚拟机加载ipvsadm模块
    
    [root@lvs02]# ipvsadm
    [root@lvs02]# lsmod | grep ip_vs
  
    
    
