
* [1. Nginx安装](#1-Nginx安装)
  * [Nginx01服务器安装](#Nginx01服务器安装)
  * [Nginx02服务器安装](#Nginx02服务器安装)
  * [Nginx03服务器安装](#Nginx03服务器安装)
* [2. Nginx服务器配置](#2-Nginx服务器配置)

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


## 2 Nginx服务器配置
