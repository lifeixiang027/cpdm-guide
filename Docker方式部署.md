*本文档适用于CentOS 7。*
## 安装Docker
1. 安装Docker
```sh
curl -fsSL https://get.docker.com | bash -s docker
```
2. 将操作系统用户添加到docker组
```sh
sudo usermod -aG docker $(whoami)
newgrp docker
```
3. 修改docker配置
```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
      "https://registry.cn-hangzhou.aliyuncs.com"
  ],
  "insecure-registries" : [
    "docker-registry.corilead.com"
  ]
}
EOF
```
4. 注册docker服务
```
sudo systemctl daemon-reload
sudo systemctl enable docker
sudo systemctl restart docker
```
5. 检查docker版本
```
docker version
```

## 安装Docker Compose
1. 下载Docker Compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```
2. 检查Docker Compose版本
```
docker-compose --version
```

## 登录Docker镜像服务器
```sh
docker login docker-registry.corilead.com
```

## 启动平台服务
```sh
git clone --depth 1 http://git.corilead.com/cplm/cplm-cloud-deploy.git
chmod -R 755 cplm-cloud-deploy
cd cplm-cloud-deploy/docker
./startup-dev.sh
```
*注意：首次启动自动下载Docker镜像，需要等待一段时间。*

## 验证服务状态
1. 进入[Nacos控制台](http://localhost:8848/nacos)，以Nacos默认用户名和密码登录
2. 点击`服务管理 > 服务列表`，确认以下服务健康实例数大于0；
    - auth-server
    - gateway-server
    - admin-service
    - business-service
    - workflow-service
    - scheduler-service

## 加载演示数据
TODO

## 使用主机IP
* 默认情况下，只能在Docker主机中通过`localhost`访问基础平台应用
* 通过`ifconfig`命令查询Docker主机的IP地址，如192.168.0.77
* 访问Nacos控制台[`http://localhost:8848/nacos`](http://localhost:8848/nacos)，修改配置`cpdm-gateway.properties`
```
spring.security.oauth2.client.provider.uaa.authorization-uri=http://192.168.0.77/uaa/oauth/authorize
```
## 应用地址
* 系统控制台 [`http://192.168.0.77/plm/app/system-console`](http://localhost/plm/app/system-console)
* 租户控制台 [`http://192.168.0.77/plm/app/tenant-console`](http://localhost/plm/app/tenant-console)
*注意：访问时讲192.168.0.77修改为Docker主机IP地址*

## 更新镜像命令
需要更新镜像时，使用以下命令删除docker容器和镜像，重新启动服务。
```
#停止docker容器
docker stop `docker ps -a | grep cplm | awk '{print $1}'`

#删除docker容器
docker rm `docker ps -a | grep cplm | awk '{print $1}'`

#删除docker镜像
docker rmi `docker images | grep cplm | awk '{print $3}'`
```