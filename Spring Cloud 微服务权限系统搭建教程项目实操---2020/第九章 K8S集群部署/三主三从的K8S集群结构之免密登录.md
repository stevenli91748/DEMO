
# 目录
---

* [1. 查看master虚拟机的SSH端口配置](#1-查看master虚拟机的SSH端口配置)
* [2. 生成集群的公钥文件authorized_keys](#2-生成集群的公钥文件authorized_keys)
* [3. 各台服务器的配置，即是主机又是客户机](#3-各台服务器的配置-即是主机又是客户机)


# 1 查看master虚拟机的SSH端口配置

  **配置master到master2、master3之间免密登录, master,master2,master3都是即是客户端也是服务器，这里以master 为客户机， master2 为服务器为例，集群中的master master2 master3虚拟机都必需分别作为服务器执行该操作**
    
    查看master虚拟机的SSH端口配置
     
       c:\vmcentos8> vagrant ssh-config master
       host master
          HostName 127.0.0.1
          user vagrant
          port  2222
          IdentityFile "c::/vagrant_visual_machine/multi8/.vagrant/machines/master/virtualbox/private_key"

       
      查看master2虚拟机的SSH端口配置
      
       c:\vmcentos8> vagrant ssh-config master2
       host master2
          HostName 127.0.0.1
          user vagrant
          port  2222
          IdentityFile "c::/vagrant_visual_machine/multi8/.vagrant/machines/master2/virtualbox/private_key"
     
      查看master3虚拟机的SSH端口配置
      
       c:\vmcentos8> vagrant ssh-config master3
       host master3
          HostName 127.0.0.1
          user vagrant
          port  2222
          IdentityFile "c::/vagrant_visual_machine/multi8/.vagrant/machines/master3/virtualbox/private_key"
     
     
     在master , master2, master3 虚拟机上 编辑 /etc/hosts
     
      192.168.33.11  master
      192.168.33.10  master2
      192.168.33.9   master3

# 2 生成集群的公钥文件authorized_keys
    
    以master2为主节点，master master3为从节点为例

    2.1 从各个节点生成公钥 ssh-keygen -t rsa 
    
        在master机上生成秘钥
 
         [root@master ]# ssh-keygen -t rsa
         
         在master机上的当前登录用户(root)的目录 ~/.ssh/ 上生成 id_rsa（私钥） id_rsa.pub（公钥） 
         
         [root@master ]# ls -l /root/.ssh
         id_rsa    id_rsa.pub
         
       
       在master2机上生成秘钥
 
         [root@master2 ]# ssh-keygen -t rsa
         
         在master2机上的当前登录用户(root)的目录 ~/.ssh/ 上生成 id_rsa（私钥） id_rsa.pub（公钥） 
         
         [root@master2 ]# ls -l /root/.ssh
         id_rsa    id_rsa.pub


       在master3机上生成秘钥
 
         [root@master3 ]# ssh-keygen -t rsa

         在master3机上的当前登录用户(root)的目录 ~/.ssh/ 上生成 id_rsa（私钥） id_rsa.pub（公钥） 
         
         [root@master3 ]# ls -l /root/.ssh
         id_rsa    id_rsa.pub


    2.2 从各个节点拷贝id-rsa.pub到主节点(master2)：

       然后把master,master3 的公钥文件通过共享目录copy 到master2登录用户root路径上的 /root/.ssh/目录下,并命名为 id_rsa_from_master.pub ,id_rsa_from_master3.pub 
       
       [root@master2 ]# ls -l /root/.ssh
       id_rsa   id_rsa.pub   id_rsa_from_master.pub    id_rsa_from_master3.pub
       
       在master2上若当前登录用户目录’~/.ssh/authorized_keys’不存在,则在当前登录用户目录下建立.ssh文件夹和authorized_keys文件
       
       [root@master2 ]# cd /root/.ssh
       [root@master2 .ssh]# touch authorized_keys
       [root@master2 .ssh]# ls -l
       id_rsa   id_rsa.pub   id_rsa_from_master.pub    id_rsa_from_master3.pub   authorized_keys
       
       把各节点的公钥文件追加到authorized_keys,  被登录的服务器上只会有一个公钥文件，叫authorized_keys。如果被登录的服务器有多个客户端要连上来，就会把每个文本密钥存成一行。
       
       [root@master2 .ssh]# cat id_rsa.pub>>authorized_keys                          //把master2的公钥文件追加到master2机上的authorized_keys文件
       [root@master2 .ssh]# cat id_rsa_from_master.pub>>authorized_keys              //把master的公钥文件追加到master2机上的authorized_keys文件 
       [root@master2 .ssh]# cat id_rsa_from_master3.pub>>authorized_keys             //把master3的公钥文件追加到master2机上的authorized_keys文件
       
       现在就生成了master,master2,master3的共同的公钥文件authorized_keys, 把该公钥文件authorized_keys分发到master，master3机上的当前登录用户目录’~/.ssh/下
       
         [root@master2]# cp /root/.ssh/authorized_keys root@master: /root/.ssh/authorized_keys 
         [root@master2]# cp /root/.ssh/authorized_keys root@master3: /root/.ssh/authorized_keys 
       
         [root@master ]# ls -l /root/.ssh
         id_rsa    id_rsa.pub   authorized_keys


         [root@master3 ]# ls -l /root/.ssh
         id_rsa    id_rsa.pub  authorized_keys


#  3 各台服务器的配置 即是主机又是客户机

 **因为在集群中各台服务器的配置 即是主机又是客户机，所以每一台机同时都需要即要客户机配置又要服务器配置**


   3.1 master配置
      
     //作为客户机配置
     
     3.1.1 在master虚拟机上 编辑 /etc/hosts，添加如下内容
     
            192.168.33.11  master
            192.168.33.10  master2
            192.168.33.9   master3

     3.1.2 配置 ssh 快捷訪问名
     
           可在master机上用 ssh xxx来訪问远程虚拟机
           [root@master]# ssh master3   //訪问远程虚拟机master3 
           
           配置如下：
     
           配置master2 的ssh，修改登录用户路径 ~/.ssh/config 文件：例如 以root登录  /roo/.ssh/config文件

           Host master2
              HostName 192.168.11.10       // 设置远程服务器master2 的IP地址 
              Port 2222                    // 设置远程服务器master2 的 ssh 端口号，在主机上用 vagrant ssh-config master2 命令得到
              User root                    // 设置为远程服务器master2 的root用户    
              IdentityFile /root/.ssh/id_rsa  // 指定在本机master 下root登录用户下生成的私有密钥的地址，并非远程服务器(master2)路径
           Host master3
              HostName 192.168.33.9       // 设置远程服务器master3 的IP地址
              Port 2222                   // 设置远程服务器master3 的 ssh 端口号，在主机上用 vagrant ssh-config master3 命令得到
              User root                   // 设置为远程服务器master3 的root用户    
              IdentityFile /root/.ssh/id_rsa   // 指定在本机master 下root登录用户下生成的私有密钥的地址，并非远程服务器(master3)路径


          这里的 Host 是我们要登录的服务器的别名，为了方便快捷登录，下面是服务器的信息，最后一项是你的私钥路径,完成这个配置后我们就可以在master2上使用 ssh master2 或 ssh master3，进行登录啦

            [root@master]# ssh master2          // 免密登录远程 master2 虚拟机
            [root@master]# ssh master3         // 免密登录远程 master3 虚拟机



     // 作为服务器配置
     
     3.1.3 修改sshd服务的配置文件(/etc/ssh/sshd_config)
   
            在使用之前我们需要对ssh服务进行配置，在大多数linux系统中，ssh服务的配置文件为：/etc/ssh/sshd_config 使用vim进行编辑
            
            [root@master]# vi /etc/ssh/sshd_config
           
            port 2222                    // 设置master的 ssh 端口号，在主机上用 vagrant ssh-config master命令得到,如果不配置会出错： Connection Refused
            AuthorizedKeysFile     .ssh/authorized_keys
            PermitRootLogin yes         //是否允许root账户登录
            PasswordAuthentication no  //是否允许使用密码校验登录,如果使用免密码，用公钥key登录的话，就要设为 No.
            RSAAuthentication yes      // 允许RSA登录
            PubkeyAuthentication yes   //允许使用公钥登录

            如果端口号有冲突，要更改端口号，操作如下
            
            [root@master]# vi /etc/ssh/sshd_config
             
             更改端口号 从2222 到 2323，   因为主机端口 2222 有冲突，ssh 不能使用，启动ssh失败
             
               [root@master]# yum -y install policycoreutils-python-utils
               [root@master]# semanage port -a -t ssh_port_t -p tcp 2323
             
             Restart SSHD service

              [root@master]# systemctl restart sshd.service
             
             ssh 端口号 就从2222 更改到 2323
            
             在某些情况下，该端口号还要穿透防火墙
             
               [root@master]# firewall-cmd --permanent --zone=public --add-port=2323/tcp
             
             Reload firewall

               [root@master]# firewall-cmd --reload

             Check listening

               [root@master]# ss -tnlp|grep ssh
  

     3.1.4 检查公钥文件
     
           [root@master]# ls /root/.ssh
           id_rsa    id_rsa.pub  authorized_keys
           
           如果没有authorized_keys，看 “生成集群的公钥文件authorized_keys” 那一章节
           
           
     3.1.5 配置权限
     
          因为sshd为了安全，对属主的目录和文件权限有所要求。如果权限不对，则ssh的免密码登陆不生效。 注意文件夹一般都会赋予 x 权限，不然连进入文件夹的权限都没有。这也就是文件夹一般会赋
          予 775、755，需要保障other用户不能有w权限，文件会赋予 664、600、644、640 的原因了
          
          .ssh目录权限一般为755或者775。
          rsa_id.pub 及authorized_keys权限一般为644
          rsa_id权限必须为600
     
          但是我只有用 将.ssh目录的权限设为700，将authorized_keys，id_rsa.pub文件的权限为600 才能跟远程虚拟机master2联通
          最好整个集群的虚拟机权限都用统一的权限码，就是在master, master2, master3 的机上的
          
          登录用户的~/.ssh目录设置 700权限
          登录用户的~/.ssh/authorized_keys文件设置600权限
          
           [root@master]# chmod 700 /roo/.ssh
          
          将authorized_keys，id_rsa.pub文件的权限为600

            [root@master]# chmod 600 /roo/.ssh/authorized_keys
            [root@master]# chmod 600 /roo/.ssh/id_rsa.pub
            
          将id_rsa文件的权限为600
          
            [root@master]# chmod 600 /roo/.ssh/id_rsa
         
    3.1.6  启动SSH
            
            //在master虚拟机上启动 ssh service
            
            [root@master]# systemctl restart sshd
          
             配置免登录完成后，在客户机(master3)中输入
          
                 [root@master3]# ssh root@master
          
             或者 
          
                 [root@master3]# ssh root@192.168.33.11  


   3.2 master2配置
      
     //作为客户机配置 
     3.2.1 在master2虚拟机上 编辑 /etc/hosts，添加如下内容
     
            192.168.33.11  master
            192.168.33.10  master2
            192.168.33.9   master3
          
     3.2.2 配置 ssh 快捷訪问名
     
           可在master2机上用 ssh xxx来訪问远程虚拟机
           [root@master2]# ssh master3   //訪问远程虚拟机master3 
           
           配置如下：
     
           配置master2 的ssh，修改登录用户路径 ~/.ssh/config 文件：例如 以root登录  /roo/.ssh/config文件

           Host master
              HostName 192.168.11.11       // 设置远程服务器master的IP地址 
              Port 2222                    // 设置远程服务器master的 ssh 端口号，在主机上用 vagrant ssh-config master命令得到
              User root                    // 设置为远程服务器master的root用户    
              IdentityFile /root/.ssh/id_rsa  // 指定在本机master2 下root登录用户下生成的私有密钥的地址，并非远程服务器(master)路径
           Host master3
              HostName 192.168.33.9       // 设置远程服务器master3 的IP地址
              Port 2222                   // 设置远程服务器master3 的 ssh 端口号，在主机上用 vagrant ssh-config master3 命令得到
              User root                   // 设置为远程服务器master3 的root用户    
              IdentityFile /root/.ssh/id_rsa   // 指定在本机master2 下root登录用户下生成的私有密钥的地址，并非远程服务器(master3)路径


           这里的 Host 是我们要登录的服务器的别名，为了方便快捷登录，下面是服务器的信息，最后一项是你的私钥路径,完成这个配置后我们就可以在master2上使用 ssh master 或 ssh master3，进行登录啦

            [root@master2]# ssh master          // 免密登录远程 master虚拟机
            [root@master2]# ssh master3         // 免密登录远程 master3 虚拟机
     
     
     // 作为服务器配置
     3.2.3 修改sshd服务的配置文件(/etc/ssh/sshd_config)
   
            在使用之前我们需要对ssh服务进行配置，在大多数linux系统中，ssh服务的配置文件为：/etc/ssh/sshd_config 使用vim进行编辑
            
            [root@master2]# vi /etc/ssh/sshd_config
           
            port 2222                    // 设置master2的 ssh 端口号，在主机上用 vagrant ssh-config master2命令得到,如果不配置会出错： Connection Refused
            AuthorizedKeysFile     .ssh/authorized_keys
            PermitRootLogin yes         //是否允许root账户登录
            PasswordAuthentication no  //是否允许使用密码校验登录,如果使用免密码，用公钥key登录的话，就要设为 No.
            RSAAuthentication yes      // 允许RSA登录
            PubkeyAuthentication yes   //允许使用公钥登录

            如果端口号有冲突，要更改端口号，操作如下
            
            [root@master2]# vi /etc/ssh/sshd_config
             
             更改端口号 从2222 到 2323，   因为主机端口 2222 有冲突，ssh 不能使用，启动ssh失败
             
               [root@master2]# yum -y install policycoreutils-python-utils
               [root@master2]# semanage port -a -t ssh_port_t -p tcp 2323
             
             Restart SSHD service

              [root@master2]# systemctl restart sshd.service
             
             ssh 端口号 就从2222 更改到 2323
            
             在某些情况下，该端口号还要穿透防火墙
             
               [root@master2]# firewall-cmd --permanent --zone=public --add-port=2323/tcp
             
             Reload firewall

               [root@master2]# firewall-cmd --reload

             Check listening

               [root@master2]# ss -tnlp|grep ssh


     3.2.4 检查公钥文件
     
           [root@master2]# ls /root/.ssh
           id_rsa    id_rsa.pub  authorized_keys
           
           如果没有authorized_keys，看 “生成集群的公钥文件authorized_keys” 那一章节
           
           
     3.2.5 配置权限
     
          因为sshd为了安全，对属主的目录和文件权限有所要求。如果权限不对，则ssh的免密码登陆不生效。 注意文件夹一般都会赋予 x 权限，不然连进入文件夹的权限都没有。这也就是文件夹一般会赋
          予 775、755，需要保障other用户不能有w权限，文件会赋予 664、600、644、640 的原因了
          
          .ssh目录权限一般为755或者775。
          rsa_id.pub 及authorized_keys权限一般为644
          rsa_id权限必须为600
     
          但是我只有用 将.ssh目录的权限设为700，将authorized_keys，id_rsa.pub文件的权限为600 才能跟远程虚拟机master3联通
          最好整个集群的虚拟机权限都用统一的权限码，就是在master, master2, master3 的机上的
          
          登录用户的~/.ssh目录设置 700权限
          登录用户的~/.ssh/authorized_keys文件设置600权限

          将.ssh目录的权限为700
          
            [root@master2]# chmod 700 /roo/.ssh
          
          将authorized_keys，id_rsa.pub文件的权限为600

            [root@master2]# chmod 600 /roo/.ssh/authorized_keys
            [root@master2]# chmod 600 /roo/.ssh/id_rsa.pub
            
          将id_rsa文件的权限为600
          
            [root@master2]# chmod 600 /roo/.ssh/id_rsa
         
    3.2.6 启动SSH
    
            //在master2 虚拟机上启动 ssh service
            
            [root@master2]# systemctl restart sshd
          
             配置免登录完成后，在客户机(master)中输入
          
                 [root@master]# ssh root@master2
          
             或者 
          
                 [root@master]# ssh root@192.168.33.10  


   3.3 master3 配置

     //作为客户机配置 
     3.3.1 在master3 虚拟机上 编辑 /etc/hosts，添加如下内容
     
            192.168.33.11  master
            192.168.33.10  master2
            192.168.33.9   master3

     3.3.2 配置 ssh 快捷訪问名
     
           可在master3 机上用 ssh xxx来訪问远程虚拟机
           [root@master3]# ssh master   //訪问远程虚拟机master 
           
           配置如下：
     
           配置master3 的ssh，修改登录用户路径 ~/.ssh/config 文件：例如 以root登录  /roo/.ssh/config文件

           Host master
              HostName 192.168.11.11       // 设置远程服务器master的IP地址 
              Port 2222                    // 设置远程服务器master的 ssh 端口号，在主机上用 vagrant ssh-config master命令得到
              User root                    // 设置为远程服务器master的root用户    
              IdentityFile /root/.ssh/id_rsa  // 指定在本机master2 下root登录用户下生成的私有密钥的地址，并非远程服务器(master)路径
           Host master2
              HostName 192.168.33.10       // 设置远程服务器master2 的IP地址
              Port 2222                   // 设置远程服务器master2 的 ssh 端口号，在主机上用 vagrant ssh-config master2 命令得到
              User root                   // 设置为远程服务器master2 的root用户    
              IdentityFile /root/.ssh/id_rsa   // 指定在本机master3 下root登录用户下生成的私有密钥的地址，并非远程服务器(master2)路径


           这里的 Host 是我们要登录的服务器的别名，为了方便快捷登录，下面是服务器的信息，最后一项是你的私钥路径,完成这个配置后我们就可以在master3上使用 ssh master 或 ssh master2，进行登录啦

            [root@master3]# ssh master          // 免密登录远程 master虚拟机
            [root@master3]# ssh master2         // 免密登录远程 master2 虚拟机



     // 作为服务器配置
     
     3.3.3 修改sshd服务的配置文件(/etc/ssh/sshd_config)
   
            在使用之前我们需要对ssh服务进行配置，在大多数linux系统中，ssh服务的配置文件为：/etc/ssh/sshd_config 使用vim进行编辑
            
            [root@master3 ]# vi /etc/ssh/sshd_config
           
            port 2222                    // 设置master3 的 ssh 端口号，在主机上用 vagrant ssh-config master3 命令得到,如果不配置会出错： Connection Refused
            AuthorizedKeysFile     .ssh/authorized_keys
            PermitRootLogin yes         //是否允许root账户登录
            PasswordAuthentication no  //是否允许使用密码校验登录,如果使用免密码，用公钥key登录的话，就要设为 No.
            RSAAuthentication yes      // 允许RSA登录
            PubkeyAuthentication yes   //允许使用公钥登录

            如果主机端口号有冲突，不能启动ssh daemon , 要更改端口号，操作如下
            
            [root@master]# vi /etc/ssh/sshd_config
             
             更改端口号 从2222 到 2323，   因为主机端口 2222 有冲突，ssh 不能使用，启动ssh失败
             
               [root@master]# yum -y install policycoreutils-python-utils
               [root@master]# semanage port -a -t ssh_port_t -p tcp 2323
             
             Restart SSHD service

              [root@master]# systemctl restart sshd.service
             
             ssh 端口号 就从2222 更改到 2323
            
             在某些情况下，该端口号还要穿透防火墙
             
               [root@master]# firewall-cmd --permanent --zone=public --add-port=2323/tcp
             
             Reload firewall

               [root@master]# firewall-cmd --reload

             Check listening

               [root@master]# ss -tnlp|grep ssh





     3.3.4 检查公钥文件
     
           [root@master3]# ls /root/.ssh
           id_rsa    id_rsa.pub  authorized_keys
           
           如果没有authorized_keys，看 “生成集群的公钥文件authorized_keys” 那一章节
           
           
     3.3.5 配置权限
     
          因为sshd为了安全，对属主的目录和文件权限有所要求。如果权限不对，则ssh的免密码登陆不生效。 注意文件夹一般都会赋予 x 权限，不然连进入文件夹的权限都没有。这也就是文件夹一般会赋
          予 775、755，需要保障other用户不能有w权限，文件会赋予 664、600、644、640 的原因了
          
          .ssh目录权限一般为755或者775。
          rsa_id.pub 及authorized_keys权限一般为644
          rsa_id权限必须为600
     
         但是我只有用 将.ssh目录的权限设为700，将authorized_keys，id_rsa.pub文件的权限为600 才能跟远程虚拟机master2联通
         最好整个集群的虚拟机权限都用统一的权限码，就是在master, master2, master3 的机上的
          
         登录用户的~/.ssh目录设置 700权限
         登录用户的~/.ssh/authorized_keys文件设置600权限
          
            [root@master3]# chmod 700 /roo/.ssh
          
          将authorized_keys，id_rsa.pub文件的权限为600

            [root@master3]# chmod 600 /roo/.ssh/authorized_keys
            [root@master3]# chmod 600 /roo/.ssh/id_rsa.pub
            
          将id_rsa文件的权限为600
          
            [root@master3]# chmod 600 /roo/.ssh/id_rsa
     
     
     
         
    3.3.6 启动SSH
    
            //在master3 虚拟机上启动 ssh service
            
            [root@master3 ]# systemctl restart sshd
          
             配置免登录完成后，在客户机(master2)中输入
          
                 [root@master2 ]# ssh root@master3
          
             或者 
          
                 [root@master2 ]# ssh root@192.168.33.9  

