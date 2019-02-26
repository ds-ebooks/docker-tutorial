**启动 Docker CE**

```shell
sudo systemctl enable docker
sudo systemctl start docker
```

**建立 docker 用户组**

docker 命令默认会使用 Unix socket 与 Docker 引擎通讯，只有
root 用户和 docker 组的用户才可以访问 Docker 引擎的 Unix socket，所以一般将需要使用 docker 的用户加入 docker 用户组。

```shell
# 建立 docker 组
sudo groupadd docker
# 将当前用户加入 docker 组
sudo usermod -aG docker $USER
```

**测试安装**
```shell
docker run hello-world
```

**镜像加速器**

```shell
vi /etc/docker/daemon.json
# 添加以下内容
{
  "registry-mirrors": [
  "https://registry.docker-cn.com"
  ]
}

# 重启服务
sudo systemctl daemon-reload
sudo systemctl restart docker
```

