# Docker

## 镜像
静态，包含应用软件本身以及运行所需的一切环境配置与依赖性，打包成为一个镜像文件

## 容器
从镜像仓库中拉取并且在单独沙盒环境中运行的动态实例，本身也是一个系统内的具体进程

## docker engine和docker daemon
负责调度、管理镜像与容器

## docker hub
类似github，作为一个仓库，存储了大量官方和个人的镜像

## docker操作命令 

| 指令 | 描述 |
| ---- | ---- |
| `docker rm/rmi` | rm-删除容器/rmi-删除镜像 |
| `docker run` | 启动容器，参数较多 |
| `docker exec` | 使用容器运行相关命令 |
| `docker ps` | 列举容器 |
| `docker cp` | 在容器内外传输文件 |
| `docker build -f makeFile .` | 打包镜像命令 |
| `docker pull/push` | 拉取和推送镜像 |


