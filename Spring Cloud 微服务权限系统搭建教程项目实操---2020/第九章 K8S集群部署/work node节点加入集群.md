
# 目录

* [1. node1 节点加入集群](#node1-节点加入集群)
* [2. node2 节点加入集群](#node2-节点加入集群)
* [3. node3 节点加入集群](#node3-节点加入集群)

## node1 节点加入集群

     [root@node1]# kubeadm join 192.168.33.130:6443 --token 9vr73a.a8uxyaju799qwdjv       //虚拟IP地址和端口
                                                    --discovery-token-ca-cert-hash sha256:7c2e69131a36ae2a042a339b33381c6d0d43887e2de83720eff5359e26aec866

## node2 节点加入集群

     [root@node2]# kubeadm join 192.168.33.130:6443 --token 9vr73a.a8uxyaju799qwdjv        //虚拟IP地址和端口
                                                    --discovery-token-ca-cert-hash sha256:7c2e69131a36ae2a042a339b33381c6d0d43887e2de83720eff5359e26aec866

## node3 节点加入集群

     [root@node3]# kubeadm join 192.168.33.130:6443 --token 9vr73a.a8uxyaju799qwdjv        //虚拟IP地址和端口
                                                    --discovery-token-ca-cert-hash sha256:7c2e69131a36ae2a042a339b33381c6d0d43887e2de83720eff5359e26aec866



