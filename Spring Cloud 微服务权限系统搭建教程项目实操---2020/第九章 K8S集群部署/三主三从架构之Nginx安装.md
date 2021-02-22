
* [1. Nginx安装](#1-Nginx安装)
  * [Nginx01服务器安装](#Nginx01服务器安装)
  * [Nginx02服务器安装](#Nginx02服务器安装)
  * [Nginx03服务器安装](#Nginx03服务器安装)
* [2. Nginx服务器配置](#2-Nginx服务器配置)
  * [防火墙配置](#防火墙配置)
  * [Nginx01服务器配置](#Nginx01服务器配置)
  * [Nginx02服务器配置](#Nginx02服务器配置)
  * [Nginx03服务器配置](#Nginx03服务器配置)
* [3. 打开Nginx所在服务器的“路由”功能、关闭“ARP查询”功能并设置回环ip](#3-打开Nginx所在服务器的路由功能并关闭ARP查询功能并设置回环IP) 
* [4. Nginx服务器管理](#4-Nginx服务器管理) 

# 1 Nginx安装

## Nginx01服务器安装

    [root@nginx01]# yum install -y nginx
    [root@nginx01]# systemctl enable nginx
    [root@nginx01]# systemctl start nginx
    
    使用status命令确保正确启动了NGINX
    
    [root@nginx01]# systemctl status nginx
    
<a href="https://ibb.co/XD91sJJ"><img src="https://i.ibb.co/Sc2bnvv/Virtual-Box-multi8-master-1610860178204-33726-19-02-2021-12-12-42.png" alt="Virtual-Box-multi8-master-1610860178204-33726-19-02-2021-12-12-42" border="0"></a>

    打开浏览器  http://192.168.33.142

<a href="https://ibb.co/F0F0CHP"><img src="https://i.ibb.co/mX3XgNx/19111819528462.png" alt="19111819528462" border="0"></a>

    您已在CentOS 8上成功安装了NGINX。

    但是，您必须正确配置它，以便公众可以访问您的网站。
  
## Nginx02服务器安装

    [root@nginx02]# yum install -y nginx
    [root@nginx02]# systemctl enable nginx
    [root@nginx02]# systemctl start nginx

    使用status命令确保正确启动了NGINX
    
    [root@nginx01]# systemctl status nginx
    
<a href="https://ibb.co/XD91sJJ"><img src="https://i.ibb.co/Sc2bnvv/Virtual-Box-multi8-master-1610860178204-33726-19-02-2021-12-12-42.png" alt="Virtual-Box-multi8-master-1610860178204-33726-19-02-2021-12-12-42" border="0"></a>

    打开浏览器  http://192.168.33.143

<a href="https://ibb.co/F0F0CHP"><img src="https://i.ibb.co/mX3XgNx/19111819528462.png" alt="19111819528462" border="0"></a>

    您已在CentOS 8上成功安装了NGINX。

    但是，您必须正确配置它，以便公众可以访问您的网站。


## Nginx03服务器安装

    [root@nginx03]# yum install -y nginx
    [root@nginx03]# systemctl enable nginx
    [root@nginx03]# systemctl start nginx

    使用status命令确保正确启动了NGINX
    
    [root@nginx01]# systemctl status nginx
    
<a href="https://ibb.co/XD91sJJ"><img src="https://i.ibb.co/Sc2bnvv/Virtual-Box-multi8-master-1610860178204-33726-19-02-2021-12-12-42.png" alt="Virtual-Box-multi8-master-1610860178204-33726-19-02-2021-12-12-42" border="0"></a>

    打开浏览器  http://192.168.33.144

<a href="https://ibb.co/F0F0CHP"><img src="https://i.ibb.co/mX3XgNx/19111819528462.png" alt="19111819528462" border="0"></a>

    您已在CentOS 8上成功安装了NGINX。

    但是，您必须正确配置它，以便公众可以访问您的网站。


# 2 Nginx服务器配置


## 防火墙配置

   Nginx01虚拟机防火墙配置：
   
    [root@nginx01]# firewall-cmd --permanent --zone=public --add-service=http
    [root@nginx01]# firewall-cmd --permanent --zone=public --add-service=https
    [root@nginx01]# firewall-cmd --reload
    
   Nginx02虚拟机防火墙配置：

    [root@nginx02]# firewall-cmd --permanent --zone=public --add-service=http
    [root@nginx02]# firewall-cmd --permanent --zone=public --add-service=https
    [root@nginx02]# firewall-cmd --reload
    
   Nginx03虚拟机防火墙配置：

    [root@nginx03]# firewall-cmd --permanent --zone=public --add-service=http
    [root@nginx03]# firewall-cmd --permanent --zone=public --add-service=https
    [root@nginx03]# firewall-cmd --reload

## Nginx01服务器配置
  
    

## Nginx02服务器配置

## Nginx03服务器配置

# 3 打开Nginx所在服务器的路由功能并关闭ARP查询功能并设置回环IP

# 4 Nginx服务器管理

    为了管理您的NGINX服务器，您有多种选择。

    要检查NGINX的状态，您必须运行以下命令

        $ sudo systemctl status nginx

    要停止您的NGINX服务器，请运行

        $ sudo systemctl stop nginx

    如果要重新启动，则必须运行

        $ sudo systemctl start nginx

    如果您对NGINX服务器进行了一些修改，则可以重新加载它而不必停止并重新启动它。

    要重新加载NGINX，您只需运行

        $ sudo systemctl reload nginx

    如果您不想在引导时启动NGINX服务器，则必须通过运行来禁用它

        $ sudo systemctl disable nginx

    静态HTML文件：
    
    默认情况下，您的静态HTML文件位于“/usr/share/nginx/html”，因此，如果要导航到此路径，则将找到使用Web浏览器浏览时显示的文件的HTML。
    
    
