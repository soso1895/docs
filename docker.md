运行
docker run --name container-name -d image-name:tag
如:docker run --name myredis –d redis 	

--name：自定义容器名
-d：表示后台运行
image-name:指定运行的镜像名称

tag:镜像的版本
||||
|---|:-------------------------------------------------------|-------|
|列表 	|docker ps|（查看运行中的容器） 	加上-a；可以查看所有容器|
|停止 	|docker stop container_name / container-id| 	停止当前运行的指定容器|
|启动 	|docker start container_name / container-id| 	启动容器|
|删除 	|docker rm container-id| 	删除指定容器|
|端口映射 	|-p 6379:6379|

如:docker run  --name myredis  -d -p 6379:6379 docker.io/redis 	

-p:主机端口映射到容器内部的端口
容器日志 	docker logs container-name/container-id 	 