#                     Docker 实战

## 一.基本概念

#### 1.Docker 架构

![](.\images\image-20221031144132395.png)

k8s: CRI(Container Runtime Interface [容器运行时接口](https://jimmysong.io/kubernetes-handbook/concepts/cri.html))

Client: 客户端;操作`docker`服务器的客户端(命令行或者界面)

Docker_Host: Docker主机,安装Docker服务的主机

Containers: 容器;在Docker服务器中的容器(一个容器一般是一个应用实例,,容器间相互隔离)

Images: 镜像、映像、程序包；Image 是只读模板,其中包含创建Docker容器的说明.容器由Image运行而来,Image 固定不变.

Registries: 仓库;存储Docker Image 的地方.

> Docker用Go编程语言编写,并利用Linux内核的多种功能来交付其功能.Docker使用一种称为名称空间的技术来提供容器的隔离工作区.运行容器事时,Docker会为该容器创建一组名称空间.这些名称空间提供了一层隔离.容器的每个方面都在单独的名称空间中运行,并且对其的访问仅限于该名称空间.

|     Docker      |  面相对象  |
| :-------------: | :--------: |
|   镜像(Image)   |     类     |
| 容器(Container) | 对象(实例) |


* 容器与虚拟机

   ![](.\images\image-20221031151828602.png)

#### 2.Docker 隔离原理

* namespace ***6项隔离***(资源隔离)

| namespace | 系统调用参数  |          隔离内容          |
| :-------: | :-----------: | :------------------------: |
|    UTS    | CLONE_NEWUTS  |         主机和域名         |
|    IPC    | CLONE_NEWIPC  | 信号量、消息队列和共享内存 |
|    PID    | CLONE_NEWPID  |          进程编号          |
|  NetWork  | CLONE_NEWNET  |   网络设备、网格线、端口   |
|   Mount   |  CLONE_NEWNS  |      挂载点(文件系统)      |
|   User    | CLONE_NEWUSER |        用户和用户组        |

* cgroups **资源限制**

   cgroup 提供的主要功能如下

  * 资源限制: 限制任务使用的资源总额,并在超过这个`配额`时发出提示
  * 优先级分配: 分配`cpu`时间片数量及磁盘`io`带宽大小、控制任务运行的优先级
  * 资源统计: 统计系统资源使用量,如`cpu`使用时长、内存用量等
  * 任务控制: 对任务执行挂起、恢复等操作
  
    ```tex
    cgroup资源控制系统,每种系统独立地控制一种资源.如下
    ```
  
    |             子系统              |                             功能                             |
    | :-----------------------------: | :----------------------------------------------------------: |
    |               cpu               |              使用调度程序控制任务对cpu的使用。               |
    |     cpuacct(CPU Accounting)     |        自动生成cgroup中任务对cpu资源使用情况的报告。         |
    |             cpuset              |    为cgroup中的任务分配独立的CPU(多处理器系统时)和内存。     |
    |             devices             |              开启或关闭cgroup中任务对设备的访问              |
    |             reezer              |                   挂起或恢复cgroup中的任务                   |
    |             memory              | 设定cgroup中任务对内存使用量的限定，并生成这些任务对内存资源使用 情况的报告 |
    | perf_event(Linux CPU性能探测器) |            使cgroup中的任务可以进行统一的性能测试            |
    |      net_cls(Docker未使用)      | 通过等级识别符标记网络数据包，从而允许Linux流量监控程序(Tra  |
  
    #### 3.Docker安装 
    
    > 以下以centos为例； 
    > 更多其他安装方式，详细参照文档： https://docs.docker.com/engine/install/centos/ 
    
   * 移除旧版本 
  
      > sudo yum remove docker*
  
  - 设置docker yum源 
  
    > yum-config-manager --add-repo [http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo](http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo "http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo")
  
  - 安装最新docker engine 
  
    

  - 安装指定版本docker engine 1、在线安装