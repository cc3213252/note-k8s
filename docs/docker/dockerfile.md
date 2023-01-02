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
