### Doker概述

#### 1.基本介绍

Docker 是一个开源的应用容器引擎，基于 ==Go 语言== 并遵从 ==Apache2.0== 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

Docker 从 17.03 版本之后分为 CE（Community Edition: 社区版） 和 EE（Enterprise Edition: 企业版），我们用社区版就可以了。官网：https://docs.docker.com/

#### 2.应用场景

- Web 应用的自动化打包和发布。



- 自动化测试和持续集成、发布。



- 在服务型环境中部署和调整数据库或其他的后台应用。



- 从头编译或者扩展现有的 OpenShift 或 Cloud Foundry 平台来搭建自己的 PaaS 环境。


#### 3.Docker 的优势

Docker 是一个用于开发，交付和运行应用程序的开放平台。Docker 使您能够将应用程序与基础架构分开，从而可以快速交付软件。借助 Docker，您可以与管理应用程序相同的方式来管理基础架构。通过利用 Docker 的方法来快速交付，测试和部署代码，您可以大大减少编写代码和在生产环境中运行代码之间的延迟。

（1）快速，一致地交付您的应用程序。Docker 允许开发人员使用您提供的应用程序或服务的本地容器在标准化环境中工作，从而简化了开发的生命周期。

容器非常适合持续集成和持续交付（CI / CD）工作流程，请考虑以下示例方案：

您的开发人员在本地编写代码，并使用 Docker 容器与同事共享他们的工作。
他们使用 Docker 将其应用程序推送到测试环境中，并执行自动或手动测试。
当开发人员发现错误时，他们可以在开发环境中对其进行修复，然后将其重新部署到测试环境中，以进行测试和验证。
测试完成后，将修补程序推送给生产环境，就像将更新的镜像推送到生产环境一样简单。
（2）响应式部署和扩展
Docker 是基于容器的平台，允许高度可移植的工作负载。Docker 容器可以在开发人员的本机上，数据中心的物理或虚拟机上，云服务上或混合环境中运行。

Docker 的可移植性和轻量级的特性，还可以使您轻松地完成动态管理的工作负担，并根据业务需求指示，实时扩展或拆除应用程序和服务。

（3）在同一硬件上运行更多工作负载
Docker 轻巧快速。它为基于虚拟机管理程序的虚拟机提供了可行、经济、高效的替代方案，因此您可以利用更多的计算能力来实现业务目标。Docker 非常适合于高密度环境以及中小型部署，而您可以用更少的资源做更多的事情。

#### 4.虚拟化技术和容器化技术

虚拟化技术特点：（1）资源占用多 （2）冗余步骤多 （3）启动很慢

容器化技术：容器化技术不是模拟的一个完整的操作系统

比较Docker和虚拟机的不同：
（1）传统虚拟机，虚拟出硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件。

（2）Docker容器内的应用直接运行在宿主机的内容，容器是没有自己的内核的，也没有虚拟硬件。

（3）每个容器都是相互隔离的，每个容器都有属于自己的文件系统，互不影响。

容器化带来的好处：

（1）应用更快速的交付和部署

传统：一堆帮助文档，安装程序	Docker:打包镜像发布测试，一键运行

（2）更便捷的升级和扩缩容

使用了Docker之后，我们部署应用就和搭积木一样！项目打包为一个镜像，扩展 服务器A！服务器B

（3）更简单的系统运维

在容器化之后，我们的开发，测试环境都是高度一致的

（4）更高效的计算资源利用

Docker是内核级别的虚拟化，可以再一个物理机上可以运行很多的容器实例！服务器的性能可以被压榨到极致。

#### 5.Docker的基本组成

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200215170429949.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjg0NjIx,size_16,color_FFFFFF,t_70)



**镜像（image）：**

docker镜像就好比是一个模版，可以通过这个模版来创建容器服务，tomcat镜像——>run——>tomcat01容器（提供服务器），通过

这个镜像可以创建多个容器（最终服务运行或项目运行都是在容器中的）。

**容器（container）：**

Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建的。

启动，停止，删除，基本命令要会！目前就可以把这个容器理解为就是一个简易的Linux系统

**仓库（repository）：**

仓库就是存放镜像的地方！仓库分为公有仓库和私有仓库！Docker Hub（默认是国外的）！

#### 4.Docker的安装

查看系统的内核：uname -r

```
[root@hewei ~]# uname -r
3.10.0-1160.45.1.el7.x86_64
```

查看系统配置：cat /etc/os-release

```
[root@hewei ~]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

按照官网下载：https://docs.docker.com/engine/install/centos/

一定要多看看官网！！！

#### 5.Docker的卸载

```
#1.卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
#2.删除资源 ./var/lib/docker是docker的默认工作路径
rm -rf/var/lib/docker
```

#### 6.配置阿里云镜像加速



#### 7.Docker容器运行流程

启动一个容器，Docker的运行流程图如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717125820781.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5namhhaQ==,size_16,color_FFFFFF,t_70)

#### 8.底层原理

Docker是一个Client-Server结构的系统，Docker的守护进程运行在主机上，通过Socker从客户端访问！Docker Server接收到Docker-Client的指令，就会执行这个指令！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717130715341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5namhhaQ==,size_16,color_FFFFFF,t_70)

Docker为什么比VM Ware快？

（1）Docker比虚拟机更少的抽象层

（2）Docker利用宿主机的内核，VM需要的是Guest OS

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717130758965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5namhhaQ==,size_16,color_FFFFFF,t_70)



Docker新建一个容器的时候，不需要像虚拟机一样重新加载一个操作系统内核，直接利用宿主机的操作系统，而虚拟机是需要加载Guest OS。Docker和VM的对比如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717134556550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5namhhaQ==,size_16,color_FFFFFF,t_70)



### Docker的常用命令

#### 1.Docker的基本命令

```
docker version   	#查看docker的版本信息
docker info      	#查看docker的系统信息，包括镜像和容器的数量
docker 命令 --help   #帮助命令(可查看可选的参数)
docker COMMAND --help
```

Docker的基本命令https://docs.docker.com/engine/reference/commandline/docker/

#### 2.镜像命令

（1）docker images 查看本地主机的所有镜像

```
[root@hewei ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
nginx         latest    605c77e624dd   5 days ago     141MB
hello-world   latest    feb5d9fea6a5   3 months ago   13.3kB
#解释 REPOSITORY 镜像的仓库源 TAG 镜像的标签 IMAGE ID 镜像的id CREATED 镜像的创建时间 SIZE镜像的大小
#可选参数 -a/--all 列出所有镜像   -q/--quiet 只显示镜像的id
```

（2）docker search 搜索镜像

```
[root@hewei ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11905     [OK]       
mariadb                           MariaDB Server is a high performing open sou…   4556      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   894                  [OK]
phpmyadmin                        phpMyAdmin - A web interface for MySQL and M…   411       [OK]       
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   92                   
mysql/mysql-cluster               Experimental MySQL Cluster Docker images. Cr…   90                   
centurylink/mysql                 Image containing mysql. Optimized to be link…   59                   [OK]
databack/mysql-backup             Back up mysql databases to... anywhere!         54 

#可选参数
Search the Docker Hub for images
Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output

#搜索收藏数大于3000的镜像
[root@hewei ~]# docker search mysql --filter=STARS=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   11905     [OK]       
mariadb   MariaDB Server is a high performing open sou…   4556      [OK]    
```

（3）docker pull 镜像名[:tag]下载镜像

```
[root@hewei ~]# docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
72a69066d2fe: Pull complete 
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
```

下载指定版本的镜像：

```
[root@hewei ~]# docker pull mysql:5.7
5.7: Pulling from library/mysql
72a69066d2fe: Already exists 
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
0ceb82207cd7: Pull complete 
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

（4）docker rmi 删除镜像

```
#1.删除指定的镜像id
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker rmi -f  镜像id
#2.删除多个镜像id
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker rmi -f  镜像id 镜像id 镜像id
#3.删除全部的镜像id
[root@iZwz99sm8v95sckz8bd2c4Z ~]# docker rmi -f  $(docker images -aq)
```

```
[root@hewei ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
nginx         latest    605c77e624dd   5 days ago     141MB
mysql         5.7       c20987f18b13   2 weeks ago    448MB
mysql         latest    3218b38490ce   2 weeks ago    516MB
hello-world   latest    feb5d9fea6a5   3 months ago   13.3kB
[root@hewei ~]# docker rmi -f 3218b38490ce
Untagged: mysql:latest
Untagged: mysql@sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
Deleted: sha256:3218b38490cec8d31976a40b92e09d61377359eab878db49f025e5d464367f3b
Deleted: sha256:aa81ca46575069829fe1b3c654d9e8feb43b4373932159fe2cad1ac13524a2f5
Deleted: sha256:0558823b9fbe967ea6d7174999be3cc9250b3423036370dc1a6888168cbd224d
Deleted: sha256:a46013db1d31231a0e1bac7eeda5ad4786dea0b1773927b45f92ea352a6d7ff9
Deleted: sha256:af161a47bb22852e9e3caf39f1dcd590b64bb8fae54315f9c2e7dc35b025e4e3
Deleted: sha256:feff1495e6982a7e91edc59b96ea74fd80e03674d92c7ec8a502b417268822ff
[root@hewei ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
nginx         latest    605c77e624dd   5 days ago     141MB
mysql         5.7       c20987f18b13   2 weeks ago    448MB
hello-world   latest    feb5d9fea6a5   3 months ago   13.3kB
```

#### 3.容器命令

如何拉取一个centos容器

```
docker pull centos    #为啥感觉和镜像差不多
```

运行容器

```
docker run [可选参数] image

#参数说明
--name="名字"           指定容器名字
-d                     后台方式运行
-it                    使用交互方式运行,进入容器查看内容
-p                     指定容器的端口
(
-p ip:主机端口:容器端口  配置主机端口映射到容器端口
-p 主机端口:容器端口
-p 容器端口
)
-P                     随机指定端口(大写的P)
```

进入容器

```
[root@hewei ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
nginx         latest    605c77e624dd   5 days ago     141MB
mysql         5.7       c20987f18b13   2 weeks ago    448MB
hello-world   latest    feb5d9fea6a5   3 months ago   13.3kB
centos        latest    5d0da3dc9764   3 months ago   231MB
[root@hewei ~]# docker run -it centos /bin/bash
[root@75bfbc3aeb10 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

==退出容器一定要注意exit和Ctrl+P+Q的区别==

```
#exit 停止并退出容器（后台方式运行则仅退出）
#Ctrl+P+Q  不停止容器退出!!!!
[root@75bfbc3aeb10 /]# exit
exit
[root@hewei ~]# 
```

列出运行过的容器

```
#docker ps 
     # 列出当前正在运行的容器
-a   # 列出所有容器的运行记录
-n=? # 显示最近创建的n个容器
-q   # 只显示容器的编号

[root@hewei ~]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS       PORTS                               NAMES
bf41435cf96b   nginx:latest   "/docker-entrypoint.…"   21 hours ago   Up 8 hours   0.0.0.0:80->80/tcp, :::80->80/tcp   my_nginx
[root@hewei ~]# docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                     PORTS                               NAMES
75bfbc3aeb10   centos         "/bin/bash"              4 minutes ago   Exited (0) 2 minutes ago                                       optimistic_johnson
3472709bcbe3   centos         "/bin/bsh"               4 minutes ago   Created                                                        boring_shaw
bf41435cf96b   nginx:latest   "/docker-entrypoint.…"   21 hours ago    Up 8 hours                 0.0.0.0:80->80/tcp, :::80->80/tcp   my_nginx
b437f57e7254   hello-world    "/hello"                 44 hours ago    Exited (0) 44 hours ago 
```

删除容器

```
docker rm 容器id                 #删除指定的容器,不能删除正在运行的容器,强制删除使用 rm -f
docker rm -f $(docker ps -aq)   #删除所有的容器
docker ps -a -q|xargs docker rm #删除所有的容器
```

启动和停止容器

```
docker start 容器id          #启动容器
docker restart 容器id        #重启容器
docker stop 容器id           #停止当前运行的容器
docker kill 容器id           #强制停止当前容器
```

#### 4.其他常用命令

（1）日志的查看

```
[root@hewei ~]# docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or
                       relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or
                       relative (e.g. 42m for 42 minutes)
                       
常用：
docker logs -tf 容器id
docker logs --tail number 容器id   #num为要显示的日志条数

#docker容器后台运行，必须要有一个前台的进程，否则会自动停止
#编写shell脚本循环执行，使得centos容器保持运行状态
[root@hewei ~]# docker run -d centos /bin/sh -c "while true; do echo hi; sleep 5; done"
3d2689ca9a1e535d47d2360b6c1c9ba2dda631f067a7b1cd28b2082947383783
[root@hewei ~]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                               NAMES
3d2689ca9a1e   centos         "/bin/sh -c 'while t…"   2 minutes ago   Up 2 minutes                                       bold_dubinsky
bf41435cf96b   nginx:latest   "/docker-entrypoint.…"   22 hours ago    Up 8 hours     0.0.0.0:80->80/tcp, :::80->80/tcp   my_nginx
[root@hewei ~]# docker logs -tf --tail 10 3d2689ca9a1e
2022-01-04T12:34:14.351346026Z hi
2022-01-04T12:34:19.353912863Z hi
2022-01-04T12:34:24.355942107Z hi
2022-01-04T12:34:29.358259535Z hi
2022-01-04T12:34:34.360427826Z hi
2022-01-04T12:34:39.363270319Z hi
2022-01-04T12:34:44.365439898Z hi
2022-01-04T12:34:49.367589521Z hi
2022-01-04T12:34:54.369929915Z hi
2022-01-04T12:34:59.372081365Z hi
```

（2）进入当前正在运行的容器

因为通常我们的容器都是使用后台方式来运行的，有时需要进入容器修改配置

```
#方式1  docker exec 进入容器后开启一个新的终端，可以在里面操作
[root@hewei ~]# docker exec -it 3d2689ca9a1e /bin/bash
[root@3d2689ca9a1e /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
[root@3d2689ca9a1e /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 12:32 ?        00:00:00 /bin/sh -c while true; do echo hi; sleep 5;
root        96     0  0 12:39 pts/0    00:00:00 /bin/bash
root       113     1  0 12:39 ?        00:00:00 /usr/bin/coreutils --coreutils-prog-shebang
root       114    96  0 12:40 pts/0    00:00:00 ps -ef
```

```
#方式2	docker attach 进入容器正在执行的终端，不会启动新的进程
[root@hewei ~]# docker attach 3d2689ca9a1e
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210717134852290.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5namhhaQ==,size_16,color_FFFFFF,t_70)



### 图形化管理工具Portaniner

Portaniner是Docker的图形化管理工具，类似的工具还有Rancher(CI/CD再用)

下载运行Portaniner镜像并运行，设置本机映射端口为8088

```
[root@hewei ~]#  docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
Unable to find image 'portainer/portainer:latest' locally
latest: Pulling from portainer/portainer
94cfa856b2b1: Pull complete 
49d59ee0881a: Pull complete 
a2300fd28637: Pull complete 
Digest: sha256:fb45b43738646048a0a0cc74fcee2865b69efde857e710126084ee5de9be0f3f
Status: Downloaded newer image for portainer/portainer:latest
250eb64636a692efab94380bee5e86f25382d1e3e83b8d5f81f548470d536512
[root@hewei ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                       NAMES
250eb64636a6   portainer/portainer   "/portainer"             45 seconds ago   Up 44 seconds   0.0.0.0:8088->9000/tcp, :::8088->9000/tcp   frosty_rosalind
3d2689ca9a1e   centos                "/bin/sh -c 'while t…"   23 minutes ago   Up 23 minutes                                               bold_dubinsky
bf41435cf96b   nginx:latest          "/docker-entrypoint.…"   22 hours ago     Up 8 hours      0.0.0.0:80->80/tcp, :::80->80/tcp           my_nginx
```

第一次登录设置admin用户的密码

如果是阿里云服务器记得设置安全组，选择连接本地的Docker。

### Docker镜像详解

（1）什么是镜像

镜像是一种轻量级，可执行性的独立软件包，用来打包软件运行环境和基本运行环境开发的软件，它包含运行某个软件所需要的所有内容，包括代码，运行时（一个程序在运行或者在被执行的依赖），库，环境变量和配置文件。

（2）Docker镜像加载原理

Docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统是UnionFS联合文件系统。

（3）分层理解

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层一层的在下载！

```
[root@hewei ~]# docker  pull redis
Using default tag: latest
latest: Pulling from library/redis
a2abf6c4d29d: Already exists 
c7a4e4382001: Pull complete 
4044b9ba67c9: Pull complete 
c8388a79482f: Pull complete 
413c8bb60be2: Pull complete 
1abfd3011519: Pull complete 
Digest: sha256:db485f2e245b5b3329fdc7eff4eb00f913e09d8feb9ca720788059fdc2ed8339
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
```

为什么Docker镜像要采用这种分层的结构呢？

最大的好处，我觉得莫过于是资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。查看镜像分层的方式可以通过docker image inspect命令。

```
[root@hewei ~]# docker image inspect nginx:latest
```

![img](https://img-blog.csdnimg.cn/20210718123636415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5namhhaQ==,size_16,color_FFFFFF,t_70)



![img](https://img-blog.csdnimg.cn/2021071812372978.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5namhhaQ==,size_16,color_FFFFFF,t_70)

![img](https://img-blog.csdnimg.cn/2021071812374035.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h1YW5namhhaQ==,size_16,color_FFFFFF,t_70)

（4）提交镜像

```
使用docker commit 命令提交容器成为一个新的版本
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[TAG]
```

由于默认的Tomcat镜像的webapps文件夹中没有任何内容，需要从webapps.dist中拷贝文件到webapps文件夹。下面自行制作镜像：就是从webapps.dist中拷贝文件到webapps文件夹下，并提交该镜像作为一个新的镜像。使得该镜像默认的webapps文件夹下就有文件。具体命令如下：

```
#1.复制文件夹
[root@hewei ~]# docker run -it tomcat /bin/bash
root@cc739f1f1852:/usr/local/tomcat# cd webapps
root@cc739f1f1852:/usr/local/tomcat/webapps# ls
root@cc739f1f1852:/usr/local/tomcat/webapps# cd ../
root@cc739f1f1852:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@cc739f1f1852:/usr/local/tomcat# cd webapps
root@cc739f1f1852:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
#此处使用ctrl+p+q退出docker容器，但不停止容器
[root@hewei ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                                       
f9e0c3c5e775   tomcat                "/bin/bash"              4 minutes ago   Up 4 minutes   8080/tcp                                    
250eb64636a6   portainer/portainer   "/portainer"             15 hours ago    Up 15 hours    0.0.0.0:8088->9000/tcp, :::8088->9000/tcp   
3d2689ca9a1e   centos                "/bin/sh -c 'while t…"   15 hours ago    Up 15 hours                                                bold_dubinsky
bf41435cf96b   nginx:latest          "/docker-entrypoint.…"   36 hours ago    Up 23 hours    0.0.0.0:80->80/tcp, :::80->80/tcp  
#此处使用ctrl+p+q退出docker容器，但不停止容器
[root@hewei ~]# docker exec -it f9e0c3c5e775 /bin/bash
root@f9e0c3c5e775:/usr/local/tomcat# cd webapps
root@f9e0c3c5e775:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
root@f9e0c3c5e775:/usr/local/tomcat/webapps# cd ../
root@f9e0c3c5e775:/usr/local/tomcat# read escape sequence
read escape sequence
[root@hewei ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                                       
f9e0c3c5e775   tomcat                "/bin/bash"              7 minutes ago   Up 7 minutes   8080/tcp                                    
250eb64636a6   portainer/portainer   "/portainer"             15 hours ago    Up 15 hours    0.0.0.0:8088->9000/tcp, :::8088->9000/tcp   
3d2689ca9a1e   centos                "/bin/sh -c 'while t…"   15 hours ago    Up 15 hours                                                
bf41435cf96b   nginx:latest          "/docker-entrypoint.…"   37 hours ago    Up 23 hours    0.0.0.0:80->80/tcp, :::80->80/tcp  

#2.提交镜像作为一个新的镜像
[root@hewei ~]# docker commit -m="add webapps" -a="Ethan" f9e0c3c5e775 mytomcat:1.0
sha256:87d4987abe7540aba5b3643224bf8f8b79789f614f088d12676d0d7c8e97c322
[root@hewei ~]# docker images
REPOSITORY            TAG       IMAGE ID       CREATED          SIZE
mytomcat              1.0       87d4987abe75   13 seconds ago   684MB
nginx                 latest    605c77e624dd   6 days ago       141MB
tomcat                latest    fb5657adc892   13 days ago      680MB
redis                 latest    7614ae9453d1   2 weeks ago      113MB
mysql                 5.7       c20987f18b13   2 weeks ago      448MB
hello-world           latest    feb5d9fea6a5   3 months ago     13.3kB
centos                latest    5d0da3dc9764   3 months ago     231MB
portainer/portainer   latest    580c0e4e98b0   9 months ago     79.1MB

#3.运行容器
[root@hewei ~]# docker run -it mytomcat:1.0 /bin/bash
root@e68be2c58b91:/usr/local/tomcat# cd webapps
root@e68be2c58b91:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
#此处使用ctrl+p+q退出docker容器，但不停止容器
[root@hewei ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                       
e68be2c58b91   mytomcat:1.0          "/bin/bash"              46 seconds ago   Up 46 seconds   8080/tcp                                    
f9e0c3c5e775   tomcat                "/bin/bash"              21 minutes ago   Up 21 minutes   8080/tcp                                    
250eb64636a6   portainer/portainer   "/portainer"             15 hours ago     Up 15 hours     0.0.0.0:8088->9000/tcp, :::8088->9000/tcp   
3d2689ca9a1e   centos                "/bin/sh -c 'while t…"   15 hours ago     Up 15 hours                                                 
bf41435cf96b   nginx:latest          "/docker-entrypoint.…"   37 hours ago     Up 23 hours     0.0.0.0:80->80/tcp, :::80->80/tcp      
```

### 常用容器部署

（1）Nginx部署

（2）Tomcat部署

（3）MySQL部署







































