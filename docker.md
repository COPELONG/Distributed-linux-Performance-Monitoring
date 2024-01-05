# Docker

## 知识点：

## 1.namespace：

通过namespace实现资源隔离，linux内核提拱了6种namespace隔离的系统调用。

namespace是 Linux 内核提供的一种隔离机制，而容器是利用这些隔离机制创建的一种轻量级、可移植的软件单元。

namespace提供了隔离的基础，而容器则是在这种隔离基础上构建的应用程序运行环境。

![image-20240105154738038](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240105154738038.png)

每个容器在运行时都有自己的一组命名空间，每个容器都拥有自己独立的文件系统、网络、进程等视图，例如 PID 命名空间（进程隔离）、Network 命名空间（网络隔离）、Mount 命名空间（文件系统隔离）等。不会受到其他容器的影响。

## 2.Cgroups：

cgroups 允许将系统资源（如 CPU、内存、磁盘 I/O 等）划分成不同的组，并为每个组分配特定的资源限制。与命名空间一起使用，cgroups 提供了容器化环境中对资源的更精细控制。

![image-20240105161413755](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20240105161413755.png)

cgroups 允许系统管理员对每位厨师（或每个应用程序）的资源使用进行控制和调整，确保系统中的各个部分都得到适当的资源分配。



```dockerfile
MONITOR_HOME_DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )/../.." && pwd )"
#获取当前脚本文件的上两级目录的绝对路径，并将其保存在 MONITOR_HOME_DIR 变量中。

echo "start docker"
docker run -it -d \
--name linux_monitor \   #指定容器的名称为 "linux_monitor"。
-e DISPLAY=$display \
-v ${MONITOR_HOME_DIR}:/work \  #将主机上的 ${MONITOR_HOME_DIR} 代码目录映射到容器内的 /work 目录。
-v ${XDG_RUNTIME_DIR}:${XDG_RUNTIME_DIR} \
--net host \
linux:monitor   #使用名为 "linux:monitor" 的 Docker 镜像启动容器。
```



-----------------------------------

----------------------

## 难点：

### 1. dockerfile编写：

源码构建(grpc)：shell的编写，通过查看官方依赖库的文档，如何进行cmake

逻辑顺序，配置，需要先构建安装abseil才能安装grpc。

--------------------------------

Dockerfile 是用于构建 Docker 镜像的脚本文件。Docker 使用一种称为“缓存机制”的策略，以提高构建效率。这意味着如果 Docker 在构建过程中的某一步骤没有发生变化，它会重用之前的结果而不重新执行该步骤。这种缓存机制有效减少了构建时间。

在 Docker 中，一个镜像（Image）由一系列的文件系统层叠加而成，每一层都包含了对文件系统的修改。这些层被称为镜像层（Image Layer）。

- 当你第一次运行 `docker build` 时，Docker 会执行每个指令并生成相应的镜像层。
- 如果你再次运行相同的 `docker build` 命令，Docker 会检查 Dockerfile 中的每个指令。如果某个指令及其后续的指令没有发生变化，Docker 将重用之前的缓存层而不重新执行这部分步骤。
- 如果某个指令发生了变化（比如 `COPY` 的文件内容发生了变化），Docker 将从这个指令开始重新执行，生成新的镜像层。

  答：由于 Dockerfile 的缓存机制，一旦修改了 Dockerfile 中的代码，将导致从修改位置开始的所有步骤失效，因而需要重新构建整个镜像。在这些失效的步骤中，即使已经构建了某个镜像层，也会重新执行一次，确保镜像的完整性和正确性。这是因为 Docker 认为从修改位置开始的步骤可能会受到影响，因此需要重新生成相应的镜像层。

### 2.镜像大小：

如何压缩镜像大小：

安装第三方依赖库时，解压后所占的文件过大，需要在shell中执行rm -rf操作删除多余的文件，如解压缩的文件。

### 3.run and into：

```shell
monitor_docker_into.sh

xhost +local:root 1>/dev/null 2>&1
是在主机上开放 X Window 系统的权限，允许容器中的 root 用户连接到主机的 X 服务器。
通过开放 X 权限，容器内的应用程序可以将图形界面显示到主机上。
docker exec \
    -u root \
    -it linux_monitor \
    /bin/bash
xhost -local:root 1>/dev/null 2>&1
```



















