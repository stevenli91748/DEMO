
1. 不能用vagrant ssh node2连接到远程的 node2计算机，show error message "vagrant@127.0.0.1: Permission denied (publickey, gssapi-keyex,gssapi-wih-mic)

2. 在docker环境上安装redis容器，容器不能运行

3. docker本机启动多台容器导致出现后续容器启动失败

   解决方法：
   
   vim /etc/sysctl.conf
   
   添加参数 fs.aio-max-nr = 1048576
   
   sysctl -p
   
4. NFSserver安装
5. Linux免密码登录
   
   [解决方法](https://github.com/stevenli91748/DEMO/blob/master/Spring%20Cloud%20%E5%BE%AE%E6%9C%8D%E5%8A%A1%E6%9D%83%E9%99%90%E7%B3%BB%E7%BB%9F%E6%90%AD%E5%BB%BA%E6%95%99%E7%A8%8B%E9%A1%B9%E7%9B%AE%E5%AE%9E%E6%93%8D---2020/%E7%AC%AC%E4%B9%9D%E7%AB%A0%20K8S%E9%9B%86%E7%BE%A4%E9%83%A8%E7%BD%B2/%E4%B8%89%E4%B8%BB%E4%B8%89%E4%BB%8E%E7%9A%84K8S%E9%9B%86%E7%BE%A4%E7%BB%93%E6%9E%84%E4%B9%8B%E5%85%8D%E5%AF%86%E7%99%BB%E5%BD%95.md)
