
## 学习所需准备
### 安装虚拟机
官方下载链接链接
[https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html](https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html)
### 安装Xshell和Xftp
Xshell用于连接服务器，而Xftp用于传输文件
官方下载链接
[https://www.xshell.com/zh/free-for-home-school/](https://www.xshell.com/zh/free-for-home-school/)

### 安装CentOS系统
官方下载链接
[http://isoredirect.centos.org/centos/7/isos/x86_64/](http://isoredirect.centos.org/centos/7/isos/x86_64/)

## 安装Nginx过程
1. 去官网下载`nginx`的`.tar.gz`文件，将`.tar.gz`文件放到服务器的`/usr/local`目录下
2. 输入`tar zxvf nginx-版本号.tar.gz`解压文件
3. 切换到`nginx`目录下，`cd nginx-版本号`，输入`ls`命令，可查看解压后的文件
	![在这里插入图片描述](https://img-blog.csdnimg.cn/1453a6c879d04301a035e40cf0f91aa4.png#pic_center)

4. 输入`./configure --prefix=/usr/local/nginx`，其中，`--prefix`选项设置为`/usr/local/nginx`，表示将软件安装到`/usr/local/nginx`目录下，如果出现以下报错，说明缺少依赖
    ![报错信息](https://img-blog.csdnimg.cn/9987cee925be49d5b463bd0323784bc9.png#pic_center)
5. 输入`yum install -y gcc`，安装gcc库
6. 安装`nignx`，`./configure --prefix=/usr/local/nginx`，提示缺少依赖`pcre`，输入`yum install -y pcre pcre-devel`安装`pcre`库
7. 安装`nignx`，`./configure --prefix=/usr/local/nginx`，又提示缺少依赖`zlib`，输入`yum install -y zlib zlib-devel`安装`zlib`库
8. 输入`./configure --prefix=/usr/local/nginx`，安装`nignx`成功，
9. 输入`make`，`make`是一个常用命令，用于自动化构建和编译软件项目。它读取一个名为`Makefile`的文件，其中包含了构建软件所需的指令和规则。
10. `make install`，`make install`是`make`命令的一个常用选择和参数组合。它用于将编译后的软件安装到系统中指定的目录
11. 输入`nginx -v`，如果出现`nginx`版本号，则说明`nginx`安装成功
14. 进入安装好的nginx目录`/usr/local/nginx/sbin` ，输入`./nginx`，启动`nginx`服务
15. 检验`nginx`是否启动可用，首先查看一下`ip`地址，输入`ip addr`，并在浏览器中输入对应的`ip`，发现不可用
16. 关闭防火墙`systemctl stop firewalld.service`，发现`nginx`可用
17. 查看`Nginx`的进程，`ps -ef | grep nginx`
## Nginx的常用指令
进入安装好的的目录`/usr/local/nginx/sbin`

```
./nginx 启动
./nginx -s stop 快速停止
./nginx -s quit 优雅关闭，在退出前完成已经接受的连接请求
./nginx -s reload 重新加载配置
```
## 关于防火墙
```
systemctl stop firewalld.service 关闭防火墙
systemctl disable firewalld.service 禁止防火墙开机启动
firewall-cmd --zone=public --add-port=80/tcp --permanent 放行端口
firewall-cmd --reload 重启防火墙
```
## 将Nginx安装成系统服务
创建服务脚本
`vi /usr/lib/systemd/system/nginx.service`
脚本服务内容

```
[Unit]
Description=nginx - web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
ExecQuit=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
重新加载系统服务
`systemctl daemon-reload`
启动Nginx
`systemctl start nginx`
查看Nginx状态
`systemctl status nginx`
# 开机启动Nginx
`systemctl enable nginx.service`
# Nginx目录
`Nginx`一般安装在`/usr/local/nginx`目录下（安装时`--prefix`可指定安装目录）
![nginx目录](https://img-blog.csdnimg.cn/556b8a6e409b49659a39b65960581428.png#pic_center)

```
conf #配置文件
	| -nginx.conf #主配置文件
	| -其他配置文件 #可通过那个include关键字，引入到了nginx.conf生效
html #静态页面
logs #日志
	| -access.log #访问日志（每次访问都会记录）
	| -error.log #错误日志
	| -nginx.pid #进程号
sbin
	| -nginx #主进程文件
*_temp #运行时，生成临时文件
```
## Nginx运行原理图
![nginx运行原理](https://img-blog.csdnimg.cn/f007a1a93cd54e9d8a2d8cb9d98fb59e.png)
## Nginx的配置文件Nginx.conf
`Nginx.conf`的默认配置文件解析
```
worker_processes  1; #默认是1，表示开启一个业务进程，主进程是master,子进程是worker

events {
    worker_connections  1024; #单个业务进程可接受连接数
}

http {
    include       mime.types; #引入http mime类型
    default_type  application/octet-stream; #如果mime类型没有匹配上，默认使用二进制流的方式传输
    sendfile        on; #使用linux的sendfile(socket, file, len)高效网络传输，也就是数据0拷贝
    keepalive_timeout  65; #保持连接，超时的时间
    #server虚拟主机vhost，一个serve就是一个主机
    server {
        listen       80; #端口
        server_name  localhost; #域名、主机名
        #location定义请求的处理规则
        location / {
            root   html;
            index  index.html index.htm; #指定默认的索引文件
        }
        #当报500，502，503，504的错误时，url重定向
        #比如说http://atguigu.com/xxoo/index.html会重定向为http://atguigu.com/50x.html
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

## 虚拟主机
原来一台服务器只能对应一个站点，通过虚拟主机技术可以虚拟化成多个站点同时对外提供服务
### servername匹配规则
我们需要注意的时servername匹配分前后顺序，写在前面的匹配上就不会继续往下匹配了
#### 完整匹配
我们可以在同一servername中匹配多个域名
`server_name vod.mmban.com www1.mmban.com`
#### 通配符匹配
`server_name *.mmban.com`
#### 通配符结束匹配
`server_name vod.*`
#### 正则匹配
`server_name ~^[0-9]+\.mmban\.com$`
## 反向代理
网关，正向代理，反向代理本质上是一个东西，只是站的角度不同，叫法不同。
反向代理：也叫隧道代理（请求和响应都必须通过同一个地方）。有性能瓶颈，因为所有的数据都经过Nginx，所以Nginx服务器的性能至关重要。
![反向代理服务器](https://img-blog.csdnimg.cn/f2f584dd8cdf4d5b959217955200d804.png#pic_center)

```
location / {
	proxy_pass http://atguigu.com/
}
```

## 负载均衡
负载均衡：把请求，按照一定算法规则，分配给多台业务服务器（即使其中一个坏了/维护升级，还有其他服务器可以继续提供服务）
![负载均衡](https://img-blog.csdnimg.cn/c818d1610e9d4172a68ba60f008717d6.png#pic_center)

```
upstream httpd {
	server 192.168.44.102:80;
	server 192.168.43.103:80;
}
```
### 负载均衡的策略
#### 轮询
默认情况下使用轮询方式，逐一转发，这种方式适用于无状态请求
#### wegith(权重）
指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况

```
upstream httpd {
	server 127.0.0.1:8050 weight=10 down;
	server 127.0.0.1:8050 weight=1;
	server 127.0.0.1:8060 weight=1 backup;
}
```
- down:表示当前的server展示不参与负载
- weight:默认为1，weight越大，负载的权重就越大
-  backup:其它所有的非backup机器down或者忙的时候，请求backup机器
#### ip_hash（不常用）
根据客户端的ip地址转发同一台服务器，可以保持回话
#### least_conn（不常用）
最少连接访问
#### url_hash（不常用）
根据用户访问的url定向转发请求
#### fair（不常用）
根据后端服务器响应时间转发请求

## 动静分离
当用户请求时，动态请求分配到Tomcat业务服务器，静态资源请求放在Nginx服务器中

```
server{
	listen 80;
	server_name localhost;
	# /的优先级比较低，如果下面的location没匹配到，就会走http://xxx这个地址的机器
	location / {
		proxy_pass http://xxx;
	}
	# root指的是html，location/css指的是root下的css，所以地址就是html/css
	location /css {
		root html;
		index index.html index.htm;
	}
	error_page 500 502 503 504 /50x.html;
	location = /50x.html {
		root html;
	}
}
```
使用正则

```
location ~*/(js|css|img){
	root html;
	index index.html index.htm;
}
```