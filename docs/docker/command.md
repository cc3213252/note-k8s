# 命令

搜索前五个redis:  
docker search --limit 5 redis  

查看镜像、容器、数据卷所占的空间：  
docker system df

docker images

查所有镜像id: docker images -qa  
删除所有镜像: docker rmi -f $(docker images -qa)  
删除正在运行中的镜像 docker rm -f <container_id>  

docker的虚悬镜像:  
仓库名、标签都是<none>的镜像


-i 以交互式运行容器  
-t 为容器重新分配一个伪输入终端tty    
docker run -it  

-p 指定端口  
-P 随机端口

交互式启动redis：  
docker run -it redis:6.0.8

### docker ps

-a 列出当前正在运行的和历史上运行过的容器  
-l 显示最近创建的容器  
-n 显示最近n个创建的容器  
-q 静默模式，只显示容器编号  

在容器内部ctrl + p + q可以不关闭容器退出终端  

### 删除

删除已停止的容器 docker rm <容器id或容器名> 
-f 强制删除  
删除容器镜像    docker rmi <镜像id>
删除所有容器：  docker rm -f $(docker ps -a -q)  
              docker ps -a -q | xargs docker rm  

## 问题

docker run ubuntu  
docker ps看不到  

## 查看容器内部细节

docker inspect <container_id>

## 进入容器两种方式区别

docker exec -it <container id> /bin/bash 会起一个新的进程，exit不会关闭原容器  
docker attach <container id> 直接进入原容器，exit会导致原容器退出  

## 容器拷贝

从容器到主机：  docker cp 容器id:容器内路径 目的主机路径  

## 容器导出

docker export 容器id > 文件名.tar

## 容器导入

cat 文件名.tar | docker import - 镜像用户/镜像名:镜像版本号

## 提交新的镜像

原ubuntu镜像没有安装vim，现在安装后并提交成一个新镜像

docker commit -m="vim cmd add ok" -a="yudan" 容器id 要创建的目标镜像名:标签名
docker commit -m="vim cmd add ok" -a="yudan" fhud3e42 atguigu/myubuntu:1.3

## 打tag

docker tag 镜像名 registry.cn-qingdao.aliyuncs.com/atguigu/myubuntu:1.3

## 建私有仓库命令

docker run -d -p 5000:5000 -v /zzyyuse/myregistry/:/tmp/registry --privileged=true registry

查看私服镜像列表：  
curl -XGET http://localhost:5000/v2/_catalog

## 数据卷

docker run -d -p 5000:5000 -v /zzyyuse/myregistry/:/tmp/registry:ro --privileged=true registry  
以上表示主机目录读写，容器目录只读  

## 启动redis

docker run -p 6379:6379 --name myr3 --privileged=true
-v /app/redis/redis.conf:/etc/redis/redis.conf
-v /app/redis/data:/data
-d redis:6.0.8 redis-server /etc/redis/redis.conf

redis客户端连集群中某一台机器： redis-cli -p 6381  
这种方式集群环境有问题，可能会存不进去，必须加-c参数  

感觉redis集群一定要开始操作