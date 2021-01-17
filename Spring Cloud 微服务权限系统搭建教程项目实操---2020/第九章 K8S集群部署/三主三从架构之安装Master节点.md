# 初始化Master

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

#  master初始化

      [root@master]# kubeadm init --config=kubeadm-config.yaml
