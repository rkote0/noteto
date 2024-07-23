# Linux容器的实现原理



## Docker 架构的发展

### 1.基于 LXC

起初 Docker 是基于单一的二进制文件完成的，Docker Client 和 Docker Daemon是同一个二进制文件，基于 LXC 实现容器的  Namespace 隔离和 Cgroup 限制。

[LXC ](https://linuxcontainers.org/lxc/introduction/)提供了对诸如命名空间(namespace) 和控制组(cgroup) 等基础工具等操作能力，是基于 LInux 内核的容器虚拟化技术的用户空间接口。

![未命名绘图 (1)](assets/未命名绘图 (1).png)







### 2.基于 libContainer

LXC 是底层 Linux 提供的，对于Docker 跨平台的目标来说还是要很大的问题，于是 Docker 自研了libcontainer
