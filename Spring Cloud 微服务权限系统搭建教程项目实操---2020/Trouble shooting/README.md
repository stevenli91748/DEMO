
1. 不能用vagrant ssh node2连接到远程的 node2计算机，show error message "vagrant@127.0.0.1: Permission denied (publickey, gssapi-keyex,gssapi-wih-mic)

2. 在docker环境上安装redis容器，容器不能运行

3. docker本机启动多台容器导致出现后续容器启动失败

   解决方法：
   
   vim /etc/sysctl.conf
   
   添加参数 fs.aio-max-nr = 1048576
   
   sysctl -p
