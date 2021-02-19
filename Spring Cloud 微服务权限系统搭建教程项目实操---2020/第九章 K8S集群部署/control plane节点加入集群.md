
# 目录

* [Master2虚拟机加入集群](#Master2虚拟机加入集群)
* [Master3虚拟机加入集群](#Master3虚拟机加入集群)


# Master2虚拟机加入集群

    [root@master2]# kubeadm reset
    [root@master2]# rm -rf $HOME/.kube/config
    [root@master2]# swapoff -a

    [root@master2]# kubeadm join 192.168.33.130:6443 --token 9vr73a.a8uxyaju799qwdjv                  //指定虚拟IP和端口        192.168.33.130：6443
                                                     --discovery-token-ca-cert-hash sha256:7c2e69131a36ae2a042a339b33381c6d0d43887e2de83720eff5359e26aec866 
                                                     --control-plane
                                                     --certificate-key f8902e114ef118304e561c3ecd4d0b543adc226b7a07f675f56564185ffe0c07 
                                                     --apiserver-advertise-address=192.168.33.10        //当前master2 虚拟机的IP
  
      等待一段时间，看到successful即说明Kubernetes集群初始化完成。加载kubeadm认证到系统环境变量中:
        
      //加载kubeadm认证到系统环境变量           
      // 注意：想要操作k8s的用户，必须在当前用户环境下生成 ~/.kube文件
      
     //如果是root用户
     [root@master2]# echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> /vagrant/.bash_profile
     [root@master2]# cd /vagrant
     [root@master2 vagrant]# source .bash_profile 

     or 
     
     //若为非root用户
     [root@master2]# sudo mkdir -p $HOME/.kube
     [root@master2]# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
     [root@master2]# sudo chown $(id -u):$(id -g) $HOME/.kube/config



# Master3虚拟机加入集群

    [root@master3]# kubeadm reset
    [root@master3]# rm -rf $HOME/.kube/config
    [root@master3]# swapoff -a

    [root@master3]# kubeadm join 192.168.33.130:6443 --token 9vr73a.a8uxyaju799qwdjv                  //指定虚拟IP和端口        192.168.33.130：6443
                                                     --discovery-token-ca-cert-hash sha256:7c2e69131a36ae2a042a339b33381c6d0d43887e2de83720eff5359e26aec866 
                                                     --control-plane
                                                     --certificate-key f8902e114ef118304e561c3ecd4d0b543adc226b7a07f675f56564185ffe0c07 
                                                     --apiserver-advertise-address=192.168.33.9        //当前master3 虚拟机的IP


      等待一段时间，看到successful即说明Kubernetes集群初始化完成。加载kubeadm认证到系统环境变量中:
        
      //加载kubeadm认证到系统环境变量           
      // 注意：想要操作k8s的用户，必须在当前用户环境下生成 ~/.kube文件
      
     //如果是root用户
     [root@master3]# echo "export KUBECONFIG=/etc/kubernetes/admin.conf" >> /vagrant/.bash_profile
     [root@master3]# cd /vagrant
     [root@master3 vagrant]# source .bash_profile 

     or 
     
     //若为非root用户
     [root@master3]# sudo mkdir -p $HOME/.kube
     [root@master3]# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
     [root@master3]# sudo chown $(id -u):$(id -g) $HOME/.kube/config




