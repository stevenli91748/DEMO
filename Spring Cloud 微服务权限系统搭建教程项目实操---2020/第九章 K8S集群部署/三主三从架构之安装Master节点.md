
目录
* [1. 初始化Master](#1-初始化Master)
* [2. master初始化](#2-master初始化)
* [3. 加载环境变量](#3-加载环境变量)
* [4. 安装网络]()
  * [4.1 安装flannel网络](#4A-安装flannel网络)
  * [4.2 安装Calico网络](#4B-安装Calico网络)

# 1 初始化Master

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
        controlPlaneEndpoint: "192.168.33.130:6443"
        networking:
          podSubnet: "10.244.0.0/16"

#  2 master初始化

      [root@master]# swapoff -a              //清除缓冲区
      [root@master]# kubeadm init --config=kubeadm-config.yaml
      
 <a href="https://ibb.co/fVR2VKj"><img src="https://i.ibb.co/spS2pD8/kube-20210117.jpg" alt="kube-20210117" border="0"></a>


  如果初始化失败，可执行kubeadm reset后重新初始化
  
    [root@master]# kubeadm reset
    [root@master]# rm -rf $HOME/.kube/config       // $HOME 就是 /root 目录，因为是以root用户登录的

    kubeadm reset后，之前flannel创建的bridge device cni0和网口设备flannel.1依然健在。为了保证环境彻底恢复到初始状态，我们可以通过下面命令删除这两个设备：

    [root@master]# ifconfig  cni0 down
    [root@master]# brctl delbr cni0
    [root@master]# ip link delete flannel.1           //flannel1.1 可能每个人都有不同的flannel版本,这里是V1.1
    
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
## 4B 安装Calico网络
