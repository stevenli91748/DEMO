


#  Spring Cloud 微服务权限系统搭建教程项目实操---2020
                         
# 目录

---

## 系统介绍



<a href="https://ibb.co/mzZjbgr"><img src="https://i.ibb.co/pXV7b56/new-2.png" alt="new-2" border="0"></a>

<a href="https://ibb.co/51YtVKL"><img src="https://i.ibb.co/fX8B7DH/springcloud.png" alt="springcloud" border="0"></a>

<a href="https://ibb.co/HGq3CDP"><img src="https://i.ibb.co/mh4g0Cy/8015461-c10225e5f151d9c0.webp" alt="8015461-c10225e5f151d9c0" border="0"></a>

<a href="https://ibb.co/7VhTp2H"><img src="https://i.ibb.co/K7nQsFY/lvs-keepalived-nginx.png" alt="lvs-keepalived-nginx" border="0"></a>


### [前端开发环境](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/README.md)

### [后端开发环境](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/README.md)

     



## 第1章 基础框架搭建
   * [架构预览](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC1%E7%AB%A0%20%E5%9F%BA%E7%A1%80%E6%A1%86%E6%9E%B6%E6%90%AD%E5%BB%BA/%E6%9E%B6%E6%9E%84%E9%A2%84%E8%A7%88.md)

## 第9章 K8S集群部署
   * [凤凰架构部署例子: 部署 Docker CE 容器环境 ](https://icyfenix.cn/appendix/deployment-env-setup/setup-docker.html)
   * [凤凰架构部署例子: 部署 Kubernetes 集群](https://icyfenix.cn/appendix/deployment-env-setup/setup-kubernetes/)
   * [凤凰架构部署例子: 部署 Istio](https://icyfenix.cn/appendix/istio.html)
   * [凤凰架构部署例子: 部署 Elastic Stack](https://icyfenix.cn/appendix/operation-env-setup/elk-setup.html)
   * [凤凰架构部署例子: 部署 Prometheus](https://icyfenix.cn/appendix/operation-env-setup/prometheus-setup.html)

---

   * [9.0 集群环境准备](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/9.1%20%E9%9B%86%E7%BE%A4%E7%8E%AF%E5%A2%83%E5%87%86%E5%A4%87.md)
   * [9.1 安装第三方服务 --- Docker MySQL Docker Compose Redis ELK](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/9.2%20%E5%AE%89%E8%A3%85%E7%AC%AC%E4%B8%89%E6%96%B9%E6%9C%8D%E5%8A%A1.md)
   * [9.2 用Kubeadm搭建业务微服务K8S集群--- 一主三从的K8S集群结构 MySQL Redis ELK 都是单节点](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/9.3%20%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BAK8S%E9%9B%86%E7%BE%A4.md) 
   * 9.3 用Kubeadm搭建高可用业务微服务K8S集群--- 三主三从的K8S集群结构
     * [9.3.1 用Kubeadm搭建三节点Keepaliva + Nginx 高可用集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/9.4%20%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%9A%E5%8A%A1%E5%BE%AE%E6%9C%8D%E5%8A%A1K8S%E9%9B%86%E7%BE%A4---%20%E4%B8%89%E4%B8%BB%E4%B8%89%E4%BB%8E%E7%9A%84K8S%E9%9B%86%E7%BE%A4%E7%BB%93%E6%9E%84.md)
     * [9.3.2 用Kubeadm搭建三节点OAuth2统一认证中心集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9OAuth2%E7%BB%9F%E4%B8%80%E8%AE%A4%E8%AF%81%E4%B8%AD%E5%BF%83%E9%9B%86%E7%BE%A4.md)
     * [9.3.3 用Kubeadm搭建三节点网关集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9%E7%BD%91%E5%85%B3%E9%9B%86%E7%BE%A4.md)
     * [9.3.4 用Kubeadm搭建三节点Nacos服务注册中心集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9Nacos%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83%E9%9B%86%E7%BE%A4.md)
     * [9.3.5 用Kubeadm搭建三节点服务监控中心(Spring Boot Admin + Zipkin + Sleuth + Skywaking)集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9%E6%9C%8D%E5%8A%A1%E7%9B%91%E6%8E%A7%E4%B8%AD%E5%BF%83(Spring%20Boot%20Admin%20%2B%20Zipkin%20%2B%20Sleuth%20%2B%20Skywaking)%E9%9B%86%E7%BE%A4.md)
     * [9.3.6 用Kubeadm搭建三节点监控报警系统中心集群( Prometheu + Grafanas + AlertManager)](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9%E7%9B%91%E6%8E%A7%E6%8A%A5%E8%AD%A6%E7%B3%BB%E7%BB%9F%E4%B8%AD%E5%BF%83%E9%9B%86%E7%BE%A4(%20Prometheu%20%2B%20Grafanas%20%2B%20AlertManager).md)
     * [9.3.8 用Kubeadm搭建三节点SpringCloud Config配置中心集群 OR Nacos配置中心集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9SpringCloud%20Config%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83%E9%9B%86%E7%BE%A4%20OR%20Nacos%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83%E9%9B%86%E7%BE%A4.md)
     * 9.3.9 用Kubeadm搭建多层业务微服务集群
       * [9.3.9.1 搭建一个第三方的Docker镜像仓库(Harbor)放置由IDEA Docker插件构建febs-auth、febs-gateway、febs-monitor-admin、febs-server-system和febs-server-test微服务模块对应的Docker镜像](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/9.5%20%E6%90%AD%E5%BB%BA%E4%B8%80%E4%B8%AA%E7%AC%AC%E4%B8%89%E6%96%B9%E7%9A%84Docker%E9%95%9C%E5%83%8F%E4%BB%93%E5%BA%93.md)      
       * [9.3.9.2 NFS服务器搭建](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/9.6%20NFS%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%90%AD%E5%BB%BA.md) 
       * 9.3.9.3 FEBS系统微服务框架搭建
         * [软件开发环境搭建](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E8%BD%AF%E4%BB%B6%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA/README.md)
         * [父模块搭建和通用模块搭建](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC1%E7%AB%A0%20%E5%9F%BA%E7%A1%80%E6%A1%86%E6%9E%B6%E6%90%AD%E5%BB%BA/%E7%88%B6%E6%A8%A1%E5%9D%97%E6%90%AD%E5%BB%BA%E5%92%8C%E9%80%9A%E7%94%A8%E6%A8%A1%E5%9D%97%E6%90%AD%E5%BB%BA.md)
         * [搭建微服务注册中心(febs-register)](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC1%E7%AB%A0%20%E5%9F%BA%E7%A1%80%E6%A1%86%E6%9E%B6%E6%90%AD%E5%BB%BA/%E6%90%AD%E5%BB%BA%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83(febs-register).md)
         * [为SpringBoot应用的业务微服务模块构建Docker镜像](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E4%B8%BASpringBoot%E5%BA%94%E7%94%A8%E7%9A%84%E4%B8%9A%E5%8A%A1%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%A8%A1%E5%9D%97%E6%9E%84%E5%BB%BADocker%E9%95%9C%E5%83%8F.md)
         * [部署SpringBoot应用](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E9%83%A8%E7%BD%B2SpringBoot%E5%BA%94%E7%94%A8.md)        
     * [9.3.10 用Kubeadm搭建三节点分布式事务集群(Seata)](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%8B%E5%8A%A1%E9%9B%86%E7%BE%A4(Seata).md)
     * [9.3.11 用Kubeadm搭建三节点分布式任务调度集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9%E5%88%86%E5%B8%83%E5%BC%8F%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6%E9%9B%86%E7%BE%A4.md)
     * [9.3.12 用Kubeadm搭建三节点Redis缓冲数据库集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9Redis%E7%BC%93%E5%86%B2%E6%95%B0%E6%8D%AE%E5%BA%93%E9%9B%86%E7%BE%A4.md)
     * [9.3.13 用Kubeadm搭建四节点MySQL数据库集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E4%B8%89%E4%B8%BB%E4%B8%89%E4%BB%8E%E7%9A%84K8S%E9%9B%86%E7%BE%A4%E7%BB%93%E6%9E%84%E4%B9%8B%E5%85%8D%E5%AF%86%E7%99%BB%E5%BD%95.md)
     * 9.3.14 用Kubeadm搭建三节点消息队列集群
       * 9.3.14.1 用Kubeadm搭建三节点RabbinMQ消息队列集群
       * 9.3.14.2 用Kubeadm搭建三节点Kafka消息队列集群
     * 9.3.15 用Kubeadm搭建三节点日志系统集群
       * 9.3.15.1 用Kubeadm搭建三节点ELK(logstash + ElasticSearch + Kibana) 日志系统集群 
       * 9.3.15.2 用Kubeadm搭建三节点ELK2(filebeat + Kafka + logstash + ElasticSearch + Kibana) 日志系统集群 
     * [9.3.16 用Kubeadm搭建三节点对象存储系统集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9%E5%AF%B9%E8%B1%A1%E5%AD%98%E5%82%A8%E7%B3%BB%E7%BB%9F%E9%9B%86%E7%BE%A4.md)
     * [9.3.17 用Kubeadm搭建三节点ElasticSearch系统集群](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E7%94%A8Kubeadm%E6%90%AD%E5%BB%BA%E4%B8%89%E8%8A%82%E7%82%B9ElasticSearch%E7%B3%BB%E7%BB%9F%E9%9B%86%E7%BE%A4.md)
   

## SQL脚本

[febs_cloud/febs_cloud_base.sql脚本](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/SQL%E8%84%9A%E6%9C%AC/Febs_Cloud/febs_cloud_base.sql)

## [Trouble shooting](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/Trouble%20shooting/README.md)

