- 容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟.

- Docker与虚拟机

  - 传统虚拟机

    ![img](https://img.mubu.com/document_image/e4b941c0-6cab-40b8-b5c9-2bb68a28a71e-1393053.jpg)

  - Docker

    ![img](https://img.mubu.com/document_image/198b2273-0455-4fc5-b717-a4d6c1f87273-1393053.jpg)

- Docker三要素：

  - 镜像（Image）
    - Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。镜像不包含任何动态数据，其内容在构建之后也不会被改变。Docker 镜像（Image），就相当于是一个 root 文件系统.
    - 镜像构建时，会一层层构建，前一层是后一层的基础，即采用分层存储。
  - 容器（Container）
    - 镜像（ Image ）和容器（ Container ）的关系，就像是面向对象程序设计中的类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
    - 容器不应该向其存储层内写入任何数据，容器存储层要保持无状态化。所有的文件写入操作，都应该使用 数据卷（Volume）、或者绑定宿主目录，在这些位置的读写会跳过容器存储层，直接对宿主（或网络存储）发生读写，其性能和稳定性更高。
    - 数据卷的生存周期独立于容器，容器消亡，数据卷不会消亡。因此，使用数据卷后，容器删除或者重新运行之后，数据却不会丢失。
  - 仓库（Repository）
    - 我们在其它服务器上使用这个镜像，就需要一个集中的存储、分发镜像的服务，即仓库（Repository）
    - 一个 Docker Registory 中可以包含多个仓库（ Repository ）；每个仓库可以包含多个标签（ Tag ）；每个标签对应一个镜像。
    - 通过 <仓库名>:<标签> 的格式来指定具体是这个软件哪个版本的镜像。如果不给出标签，将以 latest 作为默认标签。仓库名经常以 两段式路径 形式出现，比如 jwilder/nginx-proxy ，前者往往意味着 Docker Registry 多用户环境下的用户名，后者则往往是对应的软件名。

- Docker Registry 公开服务

  - 国外服务：
    - Docker Hub：https://hub.docker.com/
    - CoreOS：https://coreos.com/
    - Quay.io：https://quay.io/repository/
    - Google 的 Google Container Registry，Kubernetes
  - 国内镜像：
    - 阿里云加速器：https://cr.console.aliyun.com/#/accelerator
    - DaoCloud 加速器：https://www.daocloud.io/mirror#accelerator-doc