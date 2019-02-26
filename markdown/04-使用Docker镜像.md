Docker 运行容器前需要本地存在对应的镜像，如果本地不存在该镜像，Docker 会从镜像仓库下载该镜像。

## 一、获取镜像
以 [Docker Hub](https://hub.docker.com/explore/) 为例.
```shell
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
docker pull [OPTIONS] NAME[:TAG|@DIGEST]

docker pull ubuntu:18.04
# 没有给定Registry，则从Docker Hub中拉取
```

## 二、启动并运行镜像

```shell
# 启动里面的 bash 并且进行交互式操作
docker run -it --rm ubuntu:18.04 bash
# -i交互式操作,-t终端
# --rm 容器退出后随之将其删除
# bash 是命令

# 用 nginx 镜像启动一个容器，命名为 webserver,映射 80端口
docker run --name webserver -d -p 80:80 nginx
# 进入容器进行修改
docker exec -it webserver bash
# 查看改动
docker diff webserver
# 将改动的容器保存为镜像
docker commit \
  --author "Tao Wang <cva.engineer.ding@gmail.com>" \
  --message "修改了默认网页" \
  webserver \
  nginx:v2
```

### 慎用 docker commit

使用 docker commit 意味着所有对镜像的操作都是黑箱操作，生成的镜像也被称为黑箱镜像，换句话说，就是除了制作镜像的人知道执行过什么命令、怎么生成的镜像，别人根本无从得知。

## 三、列出镜像

### 虚悬镜像
```shell
# 显示 dangling image
docker image ls -f dangling=true
# 删除 dangling image
docker image prune
```
新旧镜像同名，旧镜像名称被取消，从而出现仓库名、标签均为 <none> 的镜像。这类无标签镜像也被称为 虚悬镜像(dangling image) 

```shell
# 只显示顶层镜像
docker image ls

# 根据仓库名列出镜像
docker image ls ubuntu

# 指定仓库名和标签
docker image ls ubuntu:18.04

# 显示包括中间层镜像在内的所有镜像
docker image ls -a
# 中间层的dangling image不能删

# 过滤器参数 --filter (since before)
# mongo:3.2 之后建立的镜像
docker image ls -f since=mongo:3.2
# LABEL过滤
docker image ls -f label=com.example.version=0.1
```

**以特定格式显示**
```shell
# 只列出ID
docker image ls -q

# 镜像ID和仓库名
docker image ls --format "{{.ID}}: {{.Repository}}"

docker image ls --format "table {{.ID}}\t{{.Repository}}\t{{.T
ag}}"
```

## 四、删除镜像
```shell
docker image rm [选项] <镜像1> [<镜像2> ...]
# <镜像> 可以是 镜像短 ID 、 镜像长 ID 、 镜像名 或者 镜像摘要
```

Untagged: 一个镜像可以对应多个标签,当我们删除了所指定的标签后，可能还有别的标签指向了这个镜像，如果是这种情况，那么 Delete行为就不会发生。

```shell
docker image rm $(docker image ls -q redis)
docker image rm $(docker image ls -q -f before=mongo:3.2)
```

## 五、Dockerfile

使用Dockerfile定制镜像
```shell
FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```
```shell
#表示从空白镜像构建
FROM scratch
```
一条 RUN 指令就是一次 docker commit，没有意义，所以一般采取这种方式：
![](http://image.creat.kim/picgo/20190221120812.png)
命令的最后添加了**清理工作的命令**，删除了为了编译构建所需要的软件，清理了所有下载、展开的文件，并且还清理了 apt 缓存文件。

### 构建镜像
```shell
docker build -t nginx:v3 . 
# 这个点是指定上下文路径，所以一般将Dockerfile 置于一个空目录下
# 利用相对路径进行构建
# 类似.gitignore，使用.dockerignore忽略不需要的文件
# 可以指定某个文件作为 Dockerfile
docker build -f ../Dockerfile.php -t nginx:v3 .
```

## 六、其他 `docker build` 用法

### 直接从 Git repo 中构建：

```shell
docker build https://github.com/twang2218/gitlab-ce-zh.git#:11.1
# #11.1指目录为 /11.1/
```

Docker 就会自己去 `git clone` 这个项目、切换到指定分支、并进入到指定目录后开始构建。

### 用给定的 tar 压缩包构建

```shell
docker build http://server/context.tar.gz
```

### 从标准输入中读取 Dockerfile 进行构建

```shell
docker build - < Dockerfile
cat Dockerfile | docker build -
```