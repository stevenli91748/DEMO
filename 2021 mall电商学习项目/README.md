**以**[mall](https://github.com/macrozheng/)**项目为基础，微服务采用领域驱动设计架构(DDD)，其中权限系统可结合mybird的**[“Spring Cloud 微服务权限系统搭建教程（看云）”](https://www.kancloud.cn/mrbird/spring-cloud) , **再结合周志明的**[“凤凰架构---构建可靠的大型分布式系统 ”](https://github.com/stevenli91748/System-Design/blob/master/%E5%87%A4%E5%87%B0%E6%9E%B6%E6%9E%84---%E6%9E%84%E5%BB%BA%E5%8F%AF%E9%9D%A0%E7%9A%84%E5%A4%A7%E5%9E%8B%E5%88%86%E5%B8%83%E5%BC%8F%E7%B3%BB%E7%BB%9F%20%E5%91%A8%E5%BF%97%E6%98%8E.md)**从以SpringCloud技术栈为基础设施的微服务架构升级到Kubernetes 为基础设施的微服务架构，然后再升级到以 Istio 为基础设施的服务网格架构，最终发展为以AWS Lambda 为基础的无服务架构**

# 目录
* 序章
  * mall架构及功能概览 
    * mall项目简介
    * 工具
    * 核心功能
    * 运行环境
    * 项目演示 
* 前端系统版本
  * [VUE前端开发](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%89%8D%E7%AB%AF%E9%A1%B9%E7%9B%AE%E5%BC%80%E5%8F%91/VUE%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/README.md)
  * [React前端开发](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%89%8D%E7%AB%AF%E9%A1%B9%E7%9B%AE%E5%BC%80%E5%8F%91/React%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/README.md)
  * [Angular前端开发](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%89%8D%E7%AB%AF%E9%A1%B9%E7%9B%AE%E5%BC%80%E5%8F%91/Angular%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/README.md)
  * 移动端前端开发
    * [Android端前端开发](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%89%8D%E7%AB%AF%E9%A1%B9%E7%9B%AE%E5%BC%80%E5%8F%91/%E7%A7%BB%E5%8A%A8%E7%AB%AF%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/Android%E7%AB%AF%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/README.md)
    * [IOS端前端开发](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%89%8D%E7%AB%AF%E9%A1%B9%E7%9B%AE%E5%BC%80%E5%8F%91/%E7%A7%BB%E5%8A%A8%E7%AB%AF%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/IOS%E7%AB%AF%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91/README.md)
 
* 后端系统版本     
  * [Spring Boot 实现单体架构版本](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%A4%A7%E5%9E%8B%E5%8D%95%E4%BD%93%E6%9E%B6%E6%9E%84%E9%A3%8E%E6%A0%BC%EF%BC%88Monolithic%EF%BC%89/README.md)
  * [Spring Cloud 实现微服务架构版本](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E9%A3%8E%E6%A0%BC%EF%BC%88Microservices%EF%BC%89---Spring%20Cloud%E6%9E%B6%E6%9E%84/README.md)
  * [Kubernetes 为基础设施的微服务架构版本](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E9%A3%8E%E6%A0%BC%EF%BC%88Microservices%EF%BC%89---Kubernetes%E6%9E%B6%E6%9E%84%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%88%98/README.md)
  * [Istio 为基础设施的服务网格架构版本](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E5%90%8E%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E9%A3%8E%E6%A0%BC%EF%BC%9A%E4%BA%91%E5%8E%9F%E7%94%9F%E6%97%B6%E4%BB%A3%EF%BC%88Cloud%20Native%EF%BC%89%EF%BC%9A%E6%9C%8D%E5%8A%A1%E7%BD%91%E6%A0%BCIstio/README.md)
  * [AWS Lambda 为基础的无服务架构版本](https://github.com/stevenli91748/DEMO/blob/master/2021%20mall%E7%94%B5%E5%95%86%E5%AD%A6%E4%B9%A0%E9%A1%B9%E7%9B%AE/%E6%97%A0%E6%9C%8D%E5%8A%A1%E6%9E%B6%E6%9E%84%E9%A3%8E%E6%A0%BC%EF%BC%88Serverless%EF%BC%89---AWS%20lambda/README.md)


