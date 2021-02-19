
目录
* [1. 初始化Master配置文件](#1-初始化Master配置文件)
* [2. master初始化](#2-master初始化)
* [3. 加载环境变量](#3-加载环境变量)
* [4. 安装网络]()
  * [4.1 安装flannel网络](#4A-安装flannel网络)
  * [4.2 安装Calico网络](#4B-安装Calico网络)

# 1 初始化Master配置文件

      //添加VIP的域名到 集群中的每一台机(master,master2,master3,node1,node2,node3,client)上的/etc/hosts
         
      [root@master]# vi /etc/hosts
      [root@master2]# vi /etc/hosts
      [root@master3]# vi /etc/hosts
      [root@node1]# vi /etc/hosts
      [root@node2]# vi /etc/hosts
      [root@node3]# vi /etc/hosts
      [root@client]# vi /etc/hosts   
      
      192.168.33.130  cluster.kube.com           // 配置VIP 的域名


      // 在master上起虚拟ip：192.168.33.130
      [root@master]# ifconfig eth1:2 192.168.33.130 netmask 255.255.255.0 up
      
      /如果要取消该虚拟ip：192.168.33.130
      [root@master]# ifconfig eth1:2 192.168.33.130 netmask 255.255.255.0 down
      


#  2 master初始化

      [root@master]# swapoff -a                 //清除缓冲区
      [root@master]# kubeadm reset              //清除kubernetes以前的配置
      [root@master]# rm -rf $HOME/.kube/config  //清除kubernetes以前的配置文件
      [root@master]# kubeadm init --control-plane-endpoint "192.168.33.130:6443"       //指定虚拟IP的地址和端口号
                                  --upload-certs                                       //生成证书
                                  --pod-network-cidr 10.244.0.0/16
                                  --apiserver-advertise-address=192.168.33.11         // 当前master虚拟机的IP
                                  --kubernetes-version=v1.19.3
                                  --service-cidr=10.1.0.0/16
      
<a href="https://ibb.co/HGDvnkc"><img src="https://i.ibb.co/P1Zpwf2/Virtual-Box-multi8-master-1610860178204-33726-19-02-2021-12-12-42.png" alt="Virtual-Box-multi8-master-1610860178204-33726-19-02-2021-12-12-42" border="0"></a>


  如果初始化失败，可执行kubeadm reset后重新初始化
  
    [root@master]# kubeadm reset
    [root@master]# rm -rf $HOME/.kube/config       // $HOME 就是 /root 目录，因为是以root用户登录的

    //kubeadm reset后，之前flannel创建的bridge device cni0和网口设备flannel.1依然健在。为了保证环境彻底恢复到初始状态，我们可以通过下面命令删除这两个设备：

    //[root@master]# ifconfig  cni0 down
    //[root@master]# brctl delbr cni0
    //[root@master]# ip link delete flannel.1           //flannel1.1 可能每个人都有不同的flannel版本,这里是V1.1
    
    然后,重新开始初始化master
    
    [root@master]# swapoff -a
    [root@master]# cd /febs
    [root@master]# kubeadm init --config=kubeadm-config.yaml   //重新生成一个新的令牌
    
# 3 加载环境变量    

    以root用户登录
 
    [root@master]# echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> /root/.bash_profile
    [root@master]# source /root/.bash_profile
    
    非root用户
    
    mkdir -p $HOME/.kube                                                // $HOME就是登录用户目录
    cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    chown $(id -u):$(id -g) $HOME/.kube/config

# 安装网络
## 4A 安装flannel网络

     //对于网络插件，可以有许多选择，请参考https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-network的说明。这里我选择的flannel
     //注意： 为了使Flannel正常工作，执行kubeadm init命令时需要增加--pod-network-cidr=10.244.0.0/16参数。Flannel适用于amd64，arm，arm64和ppc64le上工作，但使用除amd64平台得其他平台，
     //你必须手动下载并替换amd64。
     
        [root@master]# kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml  //一般会出问题 可先下载，
      
        ////在master虚拟机上下载flannel配置文件到当前目录下：
        [root@master]# wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml 
       
        //修改kube-flannel.yml：
        [root@master]# vi kube-flannel.yml
        
        containers:
             - name: kube-flannel
               image: quay.io/coreos/flannel:v0.13.1-rc1
               command:
               - /opt/bin/flanneld
               args:
               - --ip-masq
               - --kube-subnet-mgr
               - --iface=eth1 # 新增部分

<a href="https://ibb.co/5v50VyM"><img src="https://i.ibb.co/vmJF8Gw/eth1.png" alt="eth1" border="0"></a>

         //Vagrant 在多主机模式下有多个网卡，eth0 网卡用于nat转发访问公网，而eth1网卡才是主机真正的IP，在这种情况下直接部署k8s flannel 插件会导致CoreDNS无法工作，所以我们需要添加上面这条
           配置强制flannel使用eth1
           
        //安装flannel：

        [root@master]# kubectl create -f kube-flannel.yml  
        
        输出如下所示时，表示安装成功
        
  <a href="https://ibb.co/WgXpnw7"><img src="https://i.ibb.co/f4WGM5Z/flannel.png" alt="flannel" border="0"></a>
               
        再次查看节点状态：

        [root@master]# kubectl get nodes       
        
  <a href="https://ibb.co/BG3Fd6g"><img src="https://i.ibb.co/hc9whdK/flannel3.png" alt="flannel3" border="0"></a>
        
        
        可以看到所有节点都是Ready状态。
        
        执行kubectl get pods --all-namespaces，验证Kubernetes集群的相关Pod是否都正常创建并运行
        
  <a href="https://ibb.co/bR35rGH"><img src="https://i.ibb.co/5x9h5JY/flannel4.png" alt="flannel4" border="0"></a>        


## 4B 安装Calico网络
