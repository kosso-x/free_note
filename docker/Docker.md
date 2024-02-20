## 安装
* 桌面版：[https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)
* 服务器版：[https://docs.docker.com/engine/install/#server](https://docs.docker.com/engine/install/#server)
## 换源
配置文件：  /etc/docker/daemon.json   没有就创建
``` json
{
	"registry-mirrors": [
		"https://hub-mirror.c.163.com",
		"https://mirror.baidubce.com"
  ]
}
```
重启docker
``` bash
systemctl daemon-reload
systemctl restart docker
```
查看修改后的信息
``` bash
docker info
```
## 命令
* 进入容器命令行 ```docker exec -it (容器名 | 容器ID) bash  ```
* 查看镜像 ```docker images ```
* 搜索镜像 ``` docker search centos ```
* 拉取镜像（默认最新版本） ``` docker pull ```
* 删除镜像 ``` docker rmi 镜像ID ```
* 删除所有镜像 ``` docker rmi `docker images -q` ```
* 运行 | 停止 | 删除 容器 ``` docker start | stop | rm 容器名 | 容器ID ```
* 运行镜像
	```
	docker run -d \
		-p 8000:8000 \
		--name 容器命名 \
		镜像名 | 镜像ID \
		启动命令
	```
* 运行镜像本地文件挂载到镜像
	```
	docker run -d \
		-p 8080:8080 \
		--name mall2 \
		--mount type=bind,source=/dok_pros/mall-app/mall,target=/go-pro/mall \
		wx-mall:v1.0 \
		/go-pro/mall/main
	```
* 运行镜像挂载本地文件挂载docker卷 *** 挂载位置：/var/lib/docker/volumes/卷名/_data ***
	```
	docker run -d \
		-p 8080:8080 \
		--name mall2 \
		--mount type=bind,source=/dok_pros/mall-app/mall,target=/go-pro/mall \
		--mount source=mall-log,target=/go-pro/mall/var/log \
		wx-mall:v1.0 \
		/go-pro/mall/main
	```
* 共享网络
	```
	docker run -d \
        -p 8080:8080 \
        --name mall4 \
        --mount type=bind,source=/dok_pros/mall-app/mall,target=/go-pro/mall \
        --mount source=mall-log,target=/go-pro/mall/var/log \
		--network mall-net \
        wx-mall:v1.0 \
        /go-pro/mall/main
	```
* 设置环境变量
	```
	docker run \
		-p 3306:3306 \
		--name mysql \
		-e MYSQL_ROOT_PASSWORD=123456 \
		-d \
		mysql:latest
	```
