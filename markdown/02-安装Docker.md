Docker 分为 CE(社区版支持周期 7 个月) 和 EE(企业版付费使用支持周期 24 个月) 两大版本。一般我使用Ubuntu安装Docker。

## Ubuntu16.04+ 安装 Docker CE

**添加稳定版本的 Docker CE APT 镜像源**

```shell
# 1、卸载旧版本docker
sudo apt-get remove docker docker-engine docker.io

# 2、添加使用 HTTPS 传输的软件包以及 CA 证书
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# 3、添加软件源的 GPG 密钥
#国内源
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# 官方源
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# 4、向 source.list 中添加 Docker 软件源
# 国内源
sudo add-apt-repository "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# 官方源
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

**安装 Docker CE**
```shell
sudo apt-get update
sudo apt-get install docker-ce
```

## 脚本安装
```shell
# 脚本就会自动的将一切准备工作做好，并且把 Docker CE 的Edge 版本安装在系统中
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh --mirror Aliyun
```
