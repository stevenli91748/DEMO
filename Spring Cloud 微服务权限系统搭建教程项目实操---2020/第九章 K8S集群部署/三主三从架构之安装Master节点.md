
目录
* [1. 初始化Master](#1-初始化Master)
* [2. master初始化](#2-master初始化)
* [3. 加载环境变量](#3-加载环境变量)
* [4. 安装网络]()
  * [4.1 安装flannel网络](#4A-安装flannel网络)
  * [4.2 安装Calico网络](#4B-安装Calico网络)

# 1 初始化Master

  [root@master]# vi /etc/hosts
  
  192.168.33.130  cluster.kube.com           // 配置VIP 的域名

  [root@master]# mkdir febs
  [root@master]# cd /febs
  
  create kubeadmconfig.yaml at /febs
  
  在master虚拟机设置配置文件， kubeadm-config.yaml 为初始化的配置文件
  
        [root@master]# vi kubeadm-config.yaml 
        apiVersion: kubeadm.k8s.io/v1beta2
        kind: ClusterConfiguration
        kubernetesVersion: v1.19.3
        apiServer:
          certSANs:    #填写所有kube-apiserver节点的hostname、IP、VIP
          - master
          - master2
          - master3
          - node01
          - node02
          - node03
          - 192.168.33.11
          - 192.168.33.10
          - 192.168.33.9
          - 192.168.33.12
          - 192.168.33.13
          - 192.168.33.14
          - 192.168.33.130
          or 
          - cluster.kube.com
        controlPlaneEndpoint: "192.168.33.130:6443"  
        or  
        controlPlaneEndpoint: "cluster.kube.com:6443"
        networking:
          podSubnet: "10.244.0.0/16"
          
          
          
          
          
          
          
          

#  2 master初始化

      [root@master]# swapoff -a              //清除缓冲区
      [root@master]# kubeadm init --config=kubeadm-config.yaml | tee kubeadm-init.log  //# 初始化,并且将标准输出同时写入至kubeadm-init.log文件
      
 <a href="https://ibb.co/fVR2VKj"><img src="https://i.ibb.co/spS2pD8/kube-20210117.jpg" alt="kube-20210117" border="0"></a>


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
