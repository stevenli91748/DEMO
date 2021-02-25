# 负载均衡层设计方案例子
 * [架构设计：负载均衡层设计方案（1）——负载场景和解决方式](https://blog.csdn.net/yinwenjie/article/details/46605451)
 * [架构设计：负载均衡层设计方案（2）——Nginx安装](https://blog.csdn.net/yinwenjie/article/details/46620711)
 * [架构设计：负载均衡层设计方案（3）——Nginx进阶](https://blog.csdn.net/yinwenjie/article/details/46742661)
 * [架构设计：负载均衡层设计方案（4）——LVS原理](https://blog.csdn.net/yinwenjie/article/details/46845997)
 * [架构设计：负载均衡层设计方案（5）——LVS单节点安装](https://blog.csdn.net/yinwenjie/article/details/47010569)
 * [架构设计：负载均衡层设计方案（6）——Nginx + Keepalived构建高可用的负载层](https://yinwj.blog.csdn.net/article/details/47130609)
 * [架构设计：负载均衡层设计方案（7）——LVS + Keepalived + Nginx安装及配置](https://yinwj.blog.csdn.net/article/details/47211551)
 * [架构设计：负载均衡层设计方案（8）——负载均衡层总结上篇](https://yinwj.blog.csdn.net/article/details/47211641)
 * [架构设计：负载均衡层设计方案（9）——负载均衡层总结下篇](https://yinwj.blog.csdn.net/article/details/48101869)


# 目录

* [1. Nginx安装](#1-Nginx安装)
  * [用源码安装nginx](#用源码安装nginx)
    * [Nginx01服务器源码安装](#Nginx01服务器源码安装)
    * [Nginx02服务器源码安装](#Nginx02服务器源码安装)
    * [Nginx03服务器源码安装](#Nginx03服务器源码安装)
  * [用包安装nginx](#用包安装nginx)
    * [Nginx01服务器包安装](#Nginx01服务器包安装)
    * [Nginx02服务器包安装](#Nginx02服务器包安装)
    * [Nginx03服务器包安装](#Nginx03服务器包安装)
* [2. Nginx服务器配置](#2-Nginx服务器配置)
  * [防火墙配置](#防火墙配置)
  * [Nginx01服务器配置文件](#Nginx01服务器配置文件)
  * [Nginx02服务器配置文件](#Nginx02服务器配置文件)
  * [Nginx03服务器配置文件](#Nginx03服务器配置文件)
* [3. 打开Nginx所在服务器的“路由”功能、关闭“ARP查询”功能并设置回环ip](#3-打开Nginx所在服务器的路由功能并关闭ARP查询功能并设置回环IP) 
* [4. Nginx服务器管理](#4-Nginx服务器管理) 

# 1 Nginx安装

## 用源码安装nginx

### Nginx01服务器源码安装

    1. Install dependent packages
    
       [root@vip]# yum -y install make zlib zlib-devel gcc gcc-c++ libtool openssl openssl-devel
    
    2.  install PCRE
    
       [root@vip]# yum -y install wget
       [root@vip]# wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz
       [root@vip]# tar -zxvf pcre-8.37.tar.gz
       [root@vip]# cd pcre-8.37
       [root@vip]# ./configure
       [root@vip]# make && make install
       [root@vip]# pcre-config --version
       
       If the version number can be displayed correctly, it means success!
       
    3. download the nginx source package

       [root@vip]# yum -y install wget
       [root@vip]# wget http://nginx.org/download/nginx-1.12.2.tar.gz
       [root@vip]# tar -zxvf nginx-1.12.2.tar.gz
       [root@vip]# cd /nginx-1.12.2
       [root@vip nginx-1.12.2]# ./configure
       [root@vip nginx-1.12.2]# make && make install

<a href="https://ibb.co/3SBxTxL"><img src="https://i.ibb.co/1R8C0Cw/19111819528462.png" alt="19111819528462" border="0"></a>

       错误1：
           cause of the problem: Don’t know
           Solution: Go to the nginx-1.6.3 directory (the unzipped directory) 
           
           [root@vip nginx-1.12.2]# cd /objs
           [root@vip nginx-1.12.2]# vi Makefile
           
              CFLAGS =  -pipe  -O -W -Wall -Wpointer-arith -Wno-unused-parameter -Werror -g　
           
           删除 -Werror 就OK
       
       错误2：
           
           Reason for error: Don’t know
           Solution: Edit this file
           
           [root@vip nginx-1.12.2]# vi /nginx-1.2.2/src/os/unix/ngx_user.c
           
           Comment out this line (around line 35)
           
           注消 35行的这句程序
           
            /* cd.current_salt[0]=~salt[0];*/
           
           然后重新编译
           
           [root@vip nginx-1.12.2]# make
           [root@vip nginx-1.12.2]# make install 
           
           启动nginx server
           
           [root@vip nginx-1.12.2]# cd /usr/local/nginx/sbin
           [root@vip sbin]# ./nginx
           
           Access service ip, its default port is port 80
           
           http://nginx server ip 
           
<a href="https://ibb.co/9bC3kPg"><img src="https://i.ibb.co/zX0HTdx/19111819528462.png" alt="19111819528462" border="0"></a>           
           
**从源码安装Nginx 就成功完成了，但是，您必须正确配置它，以便公众可以访问您的网站。**       


### Nginx02服务器源码安装
    
    参考Nginx01

### Nginx03服务器源码安装

    参考Nginx01

## 用包安装nginx
### Nginx01服务器包安装

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
  
### Nginx02服务器包安装

     参考Nginx01

### Nginx03服务器包安装

     参考Nginx01

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

## Nginx01服务器配置文件
  
    [root@nginx01]# vi /etc/nginx/nginx.conf


      #指定运行nginx的用户和用户组，默认情况下该选项关闭（关闭的情况就是nobody）
      # 指定可以运行nginx服务的用户和用户组，只能在全局块配置
      # user [user] [group]
      # 将user指令注释掉，或者配置成nobody的话所有用户都可以运行
      # user nobody nobody;
      # user指令在Windows上不生效，如果你制定具体用户和用户组会报小面警告
      # nginx: [warn] "user" is not supported, ignored in D:\software\nginx-1.18.0/conf/nginx.conf:2
      #user www www;   or  user nobody nobody;

[worker_processes解析](#worker_processes)

      #运行nginx的进程数量，后文详细讲解，这个指令只能在全局块配置
      worker_processes  1 or auto; 
      
      #nginx运行错误的日志存放位置。当然您还可以指定错误级别,此指令可以在全局块、http块、server块以及location块中配置。
      error_log  /usr/local/nginx/logs/error.log info;
      error_log  logs/error.log notice;
      error_log  logs/error.log ;

      ##指定主进程id文件的存放位置，虽然worker_processes != 1的情况下，会有很多进程，管理进程只有一个，这个指令只能在全局块配置
      pid /usr/local/nginx/logs/nginx.pid;
            
      worker_rlimit_nofile 65535;       
       
      events
      {
        #参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型
        #是Linux 2.6以上版本内核中的高性能网络I/O模型，linux建议epoll，如果跑在FreeBSD上面，就用kqueue模型
        use epoll;
        
        #每一个进程可同时建立的连接数量，后问详细讲解

[worker_connections解析](#worker_connections)        

        worker_connections 65535;
        
        keepalive_timeout 60;
        client_header_buffer_size 4k;
        open_file_cache max=65535 inactive=60s;
        open_file_cache_valid 80s;
        open_file_cache_min_uses 1;
        open_file_cache_errors on;
      }
#============================================================================ 以上是全局配置项

      #设定http服务器，利用它的反向代理功能提供负载均衡支持
      http
      {
#================================================================================================以下是 http 协议主配置

          #安装nginx后，在conf目录下除了nginx.conf主配置文件以外，有很多模板配置文件，这里就是导入这些模板文件
          include mime.types;

          #HTTP核心模块指令，这里设定默认类型为二进制流，也就是当文件类型未定义时使用这种方式
          default_type application/octet-stream;

         #日志格式
         #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '   
                            '$status $body_bytes_sent "$http_referer" '
                           '"$http_user_agent" "$http_x_forwarded_for"';
         
         #日志文件存放的位置
         #access_log  logs/access.log  main;    


         #默认编码
         charset utf-8;
          
          server_names_hash_bucket_size 128;

          client_header_buffer_size 32k;

          large_client_header_buffers 4 64k;

          #设定通过nginx上传文件的大小
          client_max_body_size 8m;

          #sendfile 规则开启
          sendfile on;

          #开启目录列表访问，合适下载服务器，默认关闭。
          autoindex on;

          #此选项允许或禁止使用socke的TCP_CORK的选项，此选项仅在使用sendfile的时候使用
          tcp_nopush on;
     
          tcp_nodelay on;

          #指定一个连接的等待时间（单位秒），如果超过等待时间，连接就会断掉。注意一定要设置，否则高并发情况下会产生性能问题。
          keepalive_timeout  65;                      
          
       
          #FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。下面参数看字面意思都能理解。
          fastcgi_connect_timeout 300;
          fastcgi_send_timeout 300;
          fastcgi_read_timeout 300;
          fastcgi_buffer_size 64k;
          fastcgi_buffers 4 64k;
          fastcgi_busy_buffers_size 128k;
          fastcgi_temp_file_write_size 128k;
 
          #gzip模块设置
          gzip on; #开启gzip压缩输出，开启数据压缩，
          
          #进行压缩的原始文件的最小大小值，也就是说如果原始文件小于5K，那么就不会进行压缩了
          gzip_min_length 5k;    #最小压缩文件大小
          
          #gzip压缩是要申请临时内存空间的，假设前提是压缩后大小是小于等于压缩前的。例如，如果原始文件大小为10K，那么它超过了8K，所以分配的内存是8 * 2 = 16K;再例如，原始文件大小为18K，
          很明显16K也是不够的，那么按照 8 * 2 * 2 = 32K的大小申请内存。如果没有设置，默认值是申请跟原始数据相同大小的内存空间去存储gzip压缩结果。
          gzip_buffers 2 8k;    #压缩缓冲区
          
          
          #gzip压缩基于的http协议版本，默认就是HTTP 1.1
          gzip_http_version 1.1;    #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
          
          # gzip压缩级别1-9，级别越高压缩率越大，压缩时间也就越长CPU越高
          gzip_comp_level 2;    #压缩等级
          
          #需要进行gzip压缩的Content-Type的Header的类型。建议js、text、css、xml、json都要进行压缩；图片就没必要了，gif、jpge文件已经压缩得很好了，就算再压，效果也不好，而且还耗费cpu
          gzip_types text/plain application/x-javascript text/css application/xml;    
          gzip_vary on;

          #开启限制IP连接数的时候需要使用
          #limit_zone crawler $binary_remote_addr 10m;
#=========================================================================================== 以上是 http 协议主配置         
#===========================================================================================以下是Nginx后端服务配置项          
          #负载均衡配置
          upstream jh.w3cschool.cn {
            #nginx向后端服务器分配请求任务的方式，默认为轮询；如果指定了ip_hash，就是hash算法（上文介绍的算法内容）
            #ip_hash    
     
            #upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大，加权轮询方式。
            server 192.168.80.121:80 weight=3;
            server 192.168.80.122:80 weight=2;
            server 192.168.80.123:80 weight=3;
            
            #backup表示，这个是一个备份节点，只有当所有节点失效后，nginx才会往这个节点分配请求任务
            #server 192.168.220.133:8080 backup;   
            
        #nginx的upstream目前支持4种方式的分配
        #1、轮询（默认）
        #每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
        #2、weight
        #指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。
        #例如：
        #upstream bakend {
        #    server 192.168.0.14 weight=10;
        #    server 192.168.0.15 weight=10;
        #}
        #2、ip_hash
        #每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
        #例如：
        #upstream bakend {
        #    ip_hash;
        #    server 192.168.0.14:88;
        #    server 192.168.0.15:80;
        #}
        #3、fair（第三方）
        #按后端服务器的响应时间来分配请求，响应时间短的优先分配。
        #upstream backend {
        #    server server1;
        #    server server2;
        #    fair;
        #}
        #4、url_hash（第三方）
        #按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。
        #例：在upstream中加入hash语句，server语句中不能写入weight等其他的参数，hash_method是使用的hash算法
        #upstream backend {
        #    server squid1:3128;
        #    server squid2:3128;
        #    hash $request_uri;
        #    hash_method crc32;
        #}

        #tips:
        #upstream bakend{#定义负载均衡设备的Ip及设备状态}{
        #    ip_hash;
        #    server 127.0.0.1:9090 down;
        #    server 127.0.0.1:8080 weight=2;
        #    server 127.0.0.1:6060;
        #    server 127.0.0.1:7070 backup;
        #}
        #在需要使用负载均衡的server中增加 proxy_pass http://bakend/;

        #每个设备的状态设置为:
        #1.down表示单前的server暂时不参与负载
        #2.weight为weight越大，负载的权重就越大。
        #3.max_fails：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream模块定义的错误
        #4.fail_timeout:max_fails次失败后，暂停的时间。
        #5.backup： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。

        #nginx支持同时设置多组的负载均衡，用来给不用的server来使用。
        #client_body_in_file_only设置为On 可以讲client post过来的数据记录到文件中用来做debug
        #client_body_temp_path设置记录文件的目录 可以设置最多3层目录
        #location对URL进行匹配.可以进行重定向或者进行新的代理 负载均衡
          
          
      }    // http end

 #========================================================================以上是Nginx后端服务配置项
 #========================================================================以下是一个服务实例的配置
      #虚拟主机的配置
      server           //可以有多个server块
      {
          #这个代理实例的监听端口
          listen 80; 
         
          #域名可以有多个，用空格隔开
          server_name www.w3cschool.cn w3cschool.cn;
          index index.html index.htm index.php;
          root /data/www/w3cschool;
          
          #文字格式
          charset utf-8; 

          #定义本虚拟主机的访问日志
           #日志格式设定
           #$remote_addr与$http_x_forwarded_for用以记录客户端的ip地址；
           #$remote_user：用来记录客户端用户名称；
           #$time_local： 用来记录访问时间与时区；
           #$request： 用来记录请求的url与http协议；
           #$status： 用来记录请求状态；成功是200，
           #$body_bytes_sent ：记录发送给客户端文件主体内容大小；
           #$http_referer：用来记录从那个页面链接访问过来的；
           #$http_user_agent：记录客户浏览器的相关信息；
           #通常web服务器放在反向代理的后面，这样就不能获取到客户的IP地址了，通过$remote_add拿到的IP地址是反向代理服务器的iP地址。反向代理服务器在转发请求的http头信息中，可以增加x_forwarded_for信息，用以记录原有客户端的IP地址和原来客户端的请求的服务器地址。
           log_format access '$remote_addr - $remote_user [$time_local] "$request" '
           '$status $body_bytes_sent "$http_referer" '
           '"$http_user_agent" $http_x_forwarded_for';

          access_log  /usr/local/nginx/logs/host.access.log  main;
          access_log  /usr/local/nginx/logs/host.access.404.log  log404;

#===========================================================================以下是 location部份

          #location将按照规则分流满足条件的URL。"location /"您可以理解为“默认分流位置”。
          location / {
             #root目录，这个html表示nginx主安装目录下的“html”目录。
             root   html;   
             #目录中的默认展示页面
             index  index.html index.htm;        
          } 

          #location支持正则表达式，“~” 表示匹配正则表达式。
          location ~ ^/business/ {   
              #方向代理。后文详细讲解。
              proxy_pass http://backendserver1;   
           }

          #对******进行负载均衡
          location ~ .*.(php|php5)?$
          {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            include fastcgi.conf;
          }
         
           #图片缓存时间设置
           location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$
           {
               expires 10d;
           }
         
           #JS和CSS缓存时间设置
           location ~ .*.(js|css)?$
           {
               expires 1h;
           }
         
        #对 "/" 启用反向代理，
        location / {
            proxy_pass http://127.0.0.1:88;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
             
            #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
             
            #以下是一些反向代理的配置，可选。
            proxy_set_header Host $host;

            #允许客户端请求的最大单文件字节数
            client_max_body_size 10m;

            #缓冲区代理缓冲用户端请求的最大字节数，
            #如果把它设置为比较大的数值，例如256k，那么，无论使用firefox还是IE浏览器，来提交任意小于256k的图片，都很正常。如果注释该指令，使用默认的client_body_buffer_size设置，也就是操作系统页面大小的两倍，8k或者16k，问题就出现了。
            #无论使用firefox4.0还是IE8.0，提交一个比较大，200k左右的图片，都返回500 Internal Server Error错误
            client_body_buffer_size 128k;

            #表示使nginx阻止HTTP应答代码为400或者更高的应答。
            proxy_intercept_errors on;

            #后端服务器连接的超时时间_发起握手等候响应超时时间
            #nginx跟后端服务器连接超时时间(代理连接超时)
            proxy_connect_timeout 90;

            #后端服务器数据回传时间(代理发送超时)
            #后端服务器数据回传时间_就是在规定时间之内后端服务器必须传完所有的数据
            proxy_send_timeout 90;

            #连接成功后，后端服务器响应时间(代理接收超时)
            #连接成功后_等候后端服务器响应时间_其实已经进入后端的排队之中等候处理（也可以说是后端服务器处理请求的时间）
            proxy_read_timeout 90;

            #设置代理服务器（nginx）保存用户头信息的缓冲区大小
            #设置从被代理服务器读取的第一部分应答的缓冲区大小，通常情况下这部分应答中包含一个小的应答头，默认情况下这个值的大小为指令proxy_buffers中指定的一个缓冲区的大小，不过可以将其设置为更小
            proxy_buffer_size 4k;

            #proxy_buffers缓冲区，网页平均在32k以下的设置
            #设置用于读取应答（来自被代理服务器）的缓冲区数目和大小，默认情况也为分页大小，根据操作系统的不同可能是4k或者8k
            proxy_buffers 4 32k;

            #高负荷下缓冲大小（proxy_buffers*2）
            proxy_busy_buffers_size 64k;

            #设置在写入proxy_temp_path时数据的大小，预防一个工作进程在传递文件时阻塞太长
            #设定缓存文件夹大小，大于这个值，将从upstream服务器传
            proxy_temp_file_write_size 64k;
            
            
             #redirect server error pages to the static page /50x.html
             #error_page  404              /404.html;
             error_page   500 502 503 504  /50x.html;
             location = /50x.html {
                 root   html;
             }
      }     //server end
#========================================================================以上是一个服务实例的配置      
      #设定查看Nginx状态的地址
      location /NginxStatus 
      {
         stub_status on;
         access_log on;
         auth_basic "NginxStatus";
         auth_basic_user_file confpasswd;
         #htpasswd文件的内容可以用apache提供的htpasswd工具来产生。
       }
      
      
      #本地动静分离反向代理配置
      #所有jsp的页面均交由tomcat或resin处理
      location ~ .(jsp|jspx|do)?$ {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://127.0.0.1:8080;
        }
      
      #所有静态文件由nginx直接读取不经过tomcat或resin
      location ~ .*.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|
      pdf|xls|mp3|wma)$
      {
            expires 15d; 
      }
         
      location ~ .*.(js|css)?$
      {
            expires 1h;
      }
 } 
 
 ### worker_processes
 
        worker_processes：操作系统启动多少个工作进程运行Nginx。注意是工作进程，不是有多少个nginx工程。在Nginx运行的时候，会启动两种进程，一种是主进程master process；一种是工作
        进程worker process。例如我在配置文件中将worker_processes设置为4，启动Nginx后，使用进程查看命令观察名字叫做nginx的进程信息，我会看到如下结果：

<a href="https://ibb.co/3SBxTxL"><img src="https://i.ibb.co/xG4XPgQ/19111819528462.png  " alt="19111819528462" border="0"></a>

        图中可以看到1个nginx主进程，master process；还有四个工作进程，worker process。主进程负责监控端口，协调工作进程的工作状态，分配工作任务，工作进程负责进行任务处理。一般这个参
        数要和操作系统的CPU内核数成倍数
 
 ### worker_connections
   
   * [1. 更改操作系统级别的进程最大可打开文件数的设置](#更改操作系统级别的进程最大可打开文件数的设置)
   * [2. 更改Nginx软件级别的进程最大可打开文件数的设置](#更改Nginx软件级别的进程最大可打开文件数的设置)
   * [3. 验证Nginx的进程最大可打开文件数是否起作用](#验证Nginx的进程最大可打开文件数是否起作用)


       这个属性是指单个工作进程可以允许同时建立外部连接的数量。无论这个连接是外部主动建立的，还是内部建立的。这里需要注意的是，一个工作进程建立一个连接后，进程将打开一个文件副本。所以这个
       数量还受操作系统设定的，进程最大可打开的文件数有关。网上50%的文章告诉了您这个事实，并要求您修改worker_connections属性的时候，一定要使用ulimit -n 修改操作系统对进程最大文件数的限
       制，但是这样更改只能在当次用户的当次shell回话中起作用，并不是永久了。接着您继续Google/百度，发现30%的文章还告诉您，要想使“进程最大可打开的文件数”永久有效，还需要修 
       改/etc/security/limits.conf这个主配置文件，但是您应该如何正确检查“进程的最大可打开文件”的方式，却没有说。

       下面本文告诉您全面的、正确的设置方式:
      

      
      
 ####  更改操作系统级别的进程最大可打开文件数的设置：
      
       首先您需要操作系统的root权限：叫您的操作系统管理员给您。

       修改limits.conf主配置文件

         vim /etc/security/limits.conf

      在主配置文件最后加入下面两句：

      * soft nofile 65535
      * hard nofile 65535

      加到文件里面的。这两句话的含义是soft（应用软件）级别限制的最大可打开文件数的限制，hard表示操作系统级别限制的最大可打开文件数的限制，“”表示所有用户都生效。保存这个文件
      （只有root用户能够有权限）。

      保存这个文件后，配置是不会马上生效的，为了保证本次shell会话中的配置马上有效，我们需要通过ulimit命令更改本次的shell会话设置（当然您如果要重启系统，我也不能说什么）。

      [root@master]# ulimit -n 65535

      执行命令后，配置马上生效。您可以用ulimit -a 查看目前会话中的所有核心配置：
 
      [root@master]ulimit -a
      core file size (blocks, -c) 0
      data seg size (kbytes, -d) unlimited
      scheduling priority (-e) 0
      file size (blocks, -f) unlimited
      pending signals (-i) 7746
      max locked memory (kbytes, -l) 64
      max memory size (kbytes, -m) unlimited
      open files (-n) 65535                        //请注意open files这一项
      pipe size (512 bytes, -p) 8
      POSIX message queues (bytes, -q) 819200
      real-time priority (-r) 0
      stack size (kbytes, -s) 10240
      cpu time (seconds, -t) unlimited
      max user processes (-u) 7746
      virtual memory (kbytes, -v) unlimited
      file locks (-x) unlimited
 
 #### 更改Nginx软件级别的进程最大可打开文件数的设置：
 
     刚才更改的只是操作系统级别的“进程最大可打开文件”的限制，作为Nginx来说，我们还要对这个软件进行更改。打开nginx.conf主陪文件。您需要配合worker_rlimit_nofile属性。如下
     
     user root root;
     worker_processes 4;
     worker_rlimit_nofile 65535;

     #error_log logs/error.log;
     #error_log logs/error.log notice;
     #error_log logs/error.log info;

     #pid logs/nginx.pid;
     events {
     use epoll;
     worker_connections 65535;
     }

    这里只粘贴了部分代码，其他的配置代码和主题无关，也就不需要粘贴了。请注意代码行中加粗的两个配置项，请一定两个属性全部配置。配置完成后，请通过nginx -s reload命令重新启动Nginx
    
#### 验证Nginx的进程最大可打开文件数是否起作用    
 
    那么我们如何来验证配置是否起作用了呢？在linux系统中，所有的进程都会有一个临时的核心配置文件描述，存放路径在/pro/进程号/limit。首先我们来看一下，没有进行参数优化前的进程配置信息：

     ps -elf | grep nginx
     1 S root 1594 1 0 80 0 - 6070 rt_sig 05:06 ? 00:00:00 nginx: master process /usr/nginx-1.8.0/sbin/nginx
     5 S root 1596 1594 0 80 0 - 6176 ep_pol 05:06 ? 00:00:00 nginx: worker process
     5 S root 1597 1594 0 80 0 - 6176 ep_pol 05:06 ? 00:00:00 nginx: worker process
     5 S root 1598 1594 0 80 0 - 6176 ep_pol 05:06 ? 00:00:00 nginx: worker process
     5 S root 1599 1594 0 80 0 - 6176 ep_pol 05:06 ? 00:00:00 nginx: worker process

     可以看到，nginx工作进程的进程号是：1596 1597 1598 1599。我们选择一个进程，查看其核心配置信息：
     
     cat /proc/1598/limits
     
<a href="https://ibb.co/tYMxQZT"><img src="https://i.ibb.co/dk0Ljbw/19111819528462.png" alt="19111819528462" border="0"></a>

     请注意其中的Max open files ，分别是1024和4096。那么更改配置信息，并重启Nginx后，配置信息就是下图所示了
     
<a href="https://ibb.co/XFHHQ8d"><img src="https://i.ibb.co/VM661mh/19111819528462.png" alt="19111819528462" border="0"></a>     
      
## Nginx02服务器配置文件

    参考Nginx01

## Nginx03服务器配置文件

    参考Nginx01
    
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
    
    
