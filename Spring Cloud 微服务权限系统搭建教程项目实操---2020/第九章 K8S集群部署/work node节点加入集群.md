
# 目录

* [1. node1 节点加入集群](#node1-节点加入集群)
* [2. node2 节点加入集群](#node2-节点加入集群)
* [3. node3 节点加入集群](#node3-节点加入集群)

## node1 节点加入集群

     [root@node1]# kubeadm reset

     [root@node1]# rm -rf $HOME/.kube/config   //如果是以root用户登录的话，就是 /root/.kube/config

     [root@node1]# swapoff -a

     [root@node1]# systemctl enable --now kubelet

     [root@node1]# systemctl start kubelet
     
     [root@node1]# kubeadm join 192.168.33.130:6443 --token 9vr73a.a8uxyaju799qwdjv       //虚拟IP地址和端口
                                                    --discovery-token-ca-cert-hash sha256:7c2e69131a36ae2a042a339b33381c6d0d43887e2de83720eff5359e26aec866

## node2 节点加入集群
     [root@node2]# kubeadm reset

     [root@node2]# rm -rf $HOME/.kube/config   //如果是以root用户登录的话，就是 /root/.kube/config

     [root@node2]# swapoff -a

     [root@node2]# systemctl enable --now kubelet

     [root@node2]# systemctl start kubelet

     [root@node2]# kubeadm join 192.168.33.130:6443 --token 9vr73a.a8uxyaju799qwdjv        //虚拟IP地址和端口
                                                    --discovery-token-ca-cert-hash sha256:7c2e69131a36ae2a042a339b33381c6d0d43887e2de83720eff5359e26aec866

## node3 节点加入集群
 
     [root@node3]# kubeadm reset

     [root@node3]# rm -rf $HOME/.kube/config   //如果是以root用户登录的话，就是 /root/.kube/config

     [root@node3]# swapoff -a

     [root@node3]# systemctl enable --now kubelet

     [root@node3]# systemctl start kubelet

     [root@node3]# kubeadm join 192.168.33.130:6443 --token 9vr73a.a8uxyaju799qwdjv        //虚拟IP地址和端口
                                                    --discovery-token-ca-cert-hash sha256:7c2e69131a36ae2a042a339b33381c6d0d43887e2de83720eff5359e26aec866



