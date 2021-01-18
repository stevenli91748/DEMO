
# 目录
* [1 证书分发](#1-证书分发)
  * [1.1 master分发证书](#1A-master分发证书)
  * [1.2 master2移动证书至指定目录](#1B-master2移动证书至指定目录)
  * [1.3 master3移动证书至指定目录](#1C-master3移动证书至指定目录)
* [2. Control Plane节点加入集群](#2-Control Plane节点加入集群)
  * [2.1 master2加入集群](#2A-master2加入集群)
  * [2.2 master3加入集群](#2B-master3加入集群)
---

#  1 证书分发

## 1A master分发证书：
 
 如果在之前的步骤中已执行过免密登录操作，可免去 Step 1 步骤
 
 Step 1: [三主三从的K8S集群结构之免密登录](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E4%B8%89%E4%B8%BB%E4%B8%89%E4%BB%8E%E7%9A%84K8S%E9%9B%86%E7%BE%A4%E7%BB%93%E6%9E%84%E4%B9%8B%E5%85%8D%E5%AF%86%E7%99%BB%E5%BD%95.md)

Step 2:  在master上建立并运行脚本cert-main-master.sh，将证书分发至master2和master3
     
     [root@master]# cd /febs
     [root@master]# vi cert-main-master.sh
     
      USER=root # customizable
      // CONTROL_PLANE_IPS="192.168.33.10 192.168.33.9"        // 如果设置master2 master3 的 IP 地址会有很多问题
      CONTROL_PLANE_IPS="master2 master3"                      //master2 master3是从三主三从的K8S集群结构之免密登录章节中得来的
      for host in ${CONTROL_PLANE_IPS}; do
          // scp /etc/kubernetes/pki/ca.crt "${USER}"@$host:/febs      //例子：从master机上copy 指定目录下的文件都远程master2 master3 的/febs目录下  
          scp /etc/kubernetes/pki/ca.crt "${USER}"@$host:              //从master机上copy 指定目录下的文件都远程master2 master3 的root用户的目录(/root)下
          scp /etc/kubernetes/pki/ca.key "${USER}"@$host:              //一定不能漏了 “：”冒号，不然会出错的
          scp /etc/kubernetes/pki/sa.key "${USER}"@$host:
          scp /etc/kubernetes/pki/sa.pub "${USER}"@$host:
          scp /etc/kubernetes/pki/front-proxy-ca.crt "${USER}"@$host:
          scp /etc/kubernetes/pki/front-proxy-ca.key "${USER}"@$host:
          scp /etc/kubernetes/pki/etcd/ca.crt "${USER}"@$host:etcd-ca.crt
          scp /etc/kubernetes/pki/etcd/ca.key "${USER}"@$host:etcd-ca.key
      done

     //然后查看cert-main-master.sh执行权限 
     
     [root@master]#  ll|grep cert-main-master.sh
     -rw-r--r-- 1 root root .................
     
     // 设置cert-main-master.sh可执行权限 
     [root@master]# chmod +x cert-main-master.sh                      
     
     [root@master]#  ll|grep cert-main-master.sh
     -rwxr--r-- 1 root root .................
     
     执行该代码进行分发
     [root@master]# ./cert-main-master.sh
     
## 1B master2移动证书至指定目录

   在master2机上创建 cert-other-master.sh
   
     USER=root # customizable
     mkdir -p /etc/kubernetes/pki/etcd
     mv /${USER}/ca.crt /etc/kubernetes/pki/
     mv /${USER}/ca.key /etc/kubernetes/pki/
     mv /${USER}/sa.pub /etc/kubernetes/pki/
     mv /${USER}/sa.key /etc/kubernetes/pki/
     mv /${USER}/front-proxy-ca.crt /etc/kubernetes/pki/
     mv /${USER}/front-proxy-ca.key /etc/kubernetes/pki/
     mv /${USER}/etcd-ca.crt /etc/kubernetes/pki/etcd/ca.crt
     mv /${USER}/etcd-ca.key /etc/kubernetes/pki/etcd/ca.key
   
   然后
   
    [root@master2]# chmod +x cert-other-master.sh
    [root@master2]# ./cert-other-master.sh

## 1C master3移动证书至指定目录

   在master3机上创建 cert-other-master.sh
   
     USER=root # customizable
     mkdir -p /etc/kubernetes/pki/etcd
     mv /${USER}/ca.crt /etc/kubernetes/pki/
     mv /${USER}/ca.key /etc/kubernetes/pki/
     mv /${USER}/sa.pub /etc/kubernetes/pki/
     mv /${USER}/sa.key /etc/kubernetes/pki/
     mv /${USER}/front-proxy-ca.crt /etc/kubernetes/pki/
     mv /${USER}/front-proxy-ca.key /etc/kubernetes/pki/
     mv /${USER}/etcd-ca.crt /etc/kubernetes/pki/etcd/ca.crt
     mv /${USER}/etcd-ca.key /etc/kubernetes/pki/etcd/ca.key
   
   然后
   
    [root@master3]# chmod +x cert-other-master.sh
    [root@master3]# ./cert-other-master.sh


# 2 Control Plane节点加入集群

## 2A-master2加入集群

## 2B-master3加入集群
