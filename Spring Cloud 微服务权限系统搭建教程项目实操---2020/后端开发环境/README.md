
# 代码开发的步骤---[一个列子让你弄懂SpringBoot实现后台框架的搭建](https://blog.csdn.net/qq_33883389/article/details/81322481)
  * step 1 首先项目搭建---IDEA 
    * 记住Application文件一定要在根目录底下不然程序会运行不起来的 
  * step 2 然后开始新建包
    * constant :常量包，存放一些常量数据，如定义服务器响应状态码。
    * controller: 控制器，存放各种控制器，来提供数据或者返回界面
    * entity：实体类包，存放各种与数据库对应的实体类
    * mapper:存放各种与数据库映射的类
    * modle:封装返回数据json的格式样式
    * service:返回数据给控制调用
  * step 3 Application：程序的入口
    * @SpringBootApplication
    * @MapperScan("com.domain.mapper") 这个用来扫描当前项目的实体类的映射
  * step 4 POM文件的修改
  * step 5 配置application.yml文件
  * step 6 创建实体类层（entity）---创建对应每个表的实体类
  * step 7 封装一个（通用）json数据格式, 在modle目录中创建一个basemodel,在model目录中的其他的json模板继承他就可以了
  * step 8 在modle目录中创建各种model的json数据层
  * step 9 开始写我们的Mapper层（操作数据库层）
  * step 10 再写我们的Service层（调用Mapper层来完成相应操作）
  * step 11 再写Controller层

---

* 软件开发环境搭建
  * 常见架构与工具
    * [各种开发工具](https://weread.qq.com/web/reader/71032d60719ad5af7104ca2k6f4322302126f4922f45dec) 
    * [Java中的常见架构与工具](https://weread.qq.com/web/reader/6ba32c40726e7c066bad7edk73532580243735b90b45ac8)
  * [Maven环境搭建](https://github.com/stevenli91748/Engineering-special/blob/master/Maven/Maven%E9%85%8D%E7%BD%AE.md)
  * [IDEA环境搭建](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/IntellIJ%20IDEA%E4%B8%AD%E9%85%8D%E7%BD%AEgithub.md)
  * Docker
  * [集群环境部署](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/README.md#K8S集群环境部署)
  * 代码分析和审查
    * [java静态代码分析工具](https://weread.qq.com/web/reader/71032d60719ad5af7104ca2kaab325601eaab3238922e53)
      * Checkstyle
      * FindBugs
      * P3C---相对于FindBugs等其他工具，阿里巴巴推出的P3C插件对代码缺陷的描述及改进意见十分详细，是开发人员提升代码质量的必备工具
    * [代码审查](https://weread.qq.com/web/reader/71032d60719ad5af7104ca2k9bf32f301f9bf31c7ff0a60)
      * [Phabricator---Facebook的内部审查工具](https://weread.qq.com/web/reader/71032d60719ad5af7104ca2k9bf32f301f9bf31c7ff0a60)
      * [Gerrit---是Google为Android系统研发量身定制的一套免费的开源代码审查系统 ](https://weread.qq.com/web/reader/71032d60719ad5af7104ca2k9bf32f301f9bf31c7ff0a60)
  * 基础框架搭建
    * 必要基础框架 
      * [搭建Maven父模块](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E6%90%AD%E5%BB%BAMaven%E7%88%B6%E6%A8%A1%E5%9D%97/README.md)
      * 搭建微服务网关
        * [使用ZUUL搭建微服务网关](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E4%BD%BF%E7%94%A8ZUUL%E6%90%AD%E5%BB%BA%E5%BE%AE%E6%9C%8D%E5%8A%A1%E7%BD%91%E5%85%B3/README.md)
        * [使用Spring Cloud Gateway搭建微服务网关  ](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E4%BD%BF%E7%94%A8Spring%20Cloud%20Gateway%E6%90%AD%E5%BB%BA%E5%BE%AE%E6%9C%8D%E5%8A%A1%E7%BD%91%E5%85%B3/README.md)
      * 搭建微服务注册中心
        * [搭建Alibaba Nacos注册中心](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E6%90%AD%E5%BB%BAAlibaba%20Nacos%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83/README.md)
        * [搭建Eureka微服务注册中心](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E6%90%AD%E5%BB%BAEureka%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%B3%A8%E5%86%8C%E4%B8%AD%E5%BF%83/README.md)
      * 搭建认证服务器
        * [搭建认证服务器](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E6%90%AD%E5%BB%BA%E8%AE%A4%E8%AF%81%E6%9C%8D%E5%8A%A1%E5%99%A8/README.md) 
      * 搭建配置中心集群 OR Nacos配置中心集群
        * [使用Alibaba Nacos搭建配置中心](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E6%90%AD%E5%BB%BAConfig%E9%85%8D%E7%BD%AE%E4%B8%AD%E5%BF%83/README.md) 
      * 搭建多层业务微服务
        * [搭建多层业务微服务](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E5%90%8E%E7%AB%AF%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/%E6%90%AD%E5%BB%BA%E5%A4%9A%E5%B1%82%E4%B8%9A%E5%8A%A1%E5%BE%AE%E6%9C%8D%E5%8A%A1/README.md) 
    * 扩展模块 
