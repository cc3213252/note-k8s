# dockerfile

每条指令都会创建一个新的镜像层并提交

RUN的两种方式： 
shell方式：  RUN yum -y install vim  
exec方式：   RUN ["yum", "-y", "install", "vim"]

USER 以什么用户执行，不指定则为root

ADD copy加解压，将宿主机目录下的文件拷贝进镜像且会自动处理url和解压tar压缩包  
COPY

CMD  容器启动后要干的事情，格式同RUN
     可以有多个CMD命令，但只有最后一个生效  
     用了CMD，如果容器最后加参数会覆盖CMD，如docker run -it -p 8080:8080 <container_id> /bin/bash  
     RUN是在构建时运行，CMD是在启动后运行  
ENTRYPOINT

```dockerfile
FROM nginx

ENTRYPOINT ["nginx", "-c"]
CMD ["/etc/nginx/nginx.conf"]
```
这里ENTRYPOINT是定参，CMD是变参  
docker run nginx:test  实际加载了默认conf  
docker run nginx:test -c /etc/nginx/new.conf  实际加载了新conf  

## 虚拟机中无法拉取国外镜像解决

先在可以翻墙的机器上拉到镜像：  
docker pull registry.k8s.io/ingress-nginx/controller:v1.5.1@sha256:4ba73c697770664c1e00e9f968de14e08f606ff961c76e5d7033a4a9c593c629

保存为压缩文件：  
docker save -o ingress.contr.tar registry.k8s.io/ingress-nginx/controller@sha256:4ba73c697770664c1e00e9f968de14e08f606ff961c76e5d7033a4a9c593c629

tar -czvf ingress.contr.tar.gz ingress.contr.tar  
scp ingress.contr.tar.gz vagrant@192.168.30.10:~/  
tar -xzvf ingress.contr.tar.gz  
sudo docker load -i ingress.contr.tar