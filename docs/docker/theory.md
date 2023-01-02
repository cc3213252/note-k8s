# 原理

1、用户使用docker client与docker daemon建立通信，发送请求给后者  
2、docker daemon作为docker架构中的主体部分，首先提供docker server功能接收请求  
3、docker engine执行docker内部工作，每个工作以job形式存在  
4、job运行过程中，当需要容器镜像时，从docker registry中下载镜像，并通过镜像管理驱动graph driver
将下载镜像以graph的形式存储  
5、当需要为docker创建网络环境时，通过网络管理驱动network driver创建并配置docker容器网络环境  
6、当需要限制docker容器运行资源或执行用户指令等操作时，则通过exec driver来完成  
7、libcontainer是一项独立的容器管理包，network driver以及exec driver都是通过libcontainer来
实现具体对容器进行的操作  


redis集群默认16384个槽，原因：  
1、心跳包大小，过大会拥堵    
2、数据传输最大化  