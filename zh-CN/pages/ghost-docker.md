## 轻量级的博客 Ghost

Ghost 是一个非常简洁优雅的博客系统，由 前 WordPress 的员工开发。默认采用 Sqlite（一个在移动端中普遍使用的轻量级数据库） 存储，访问量少的话完全够用，而且如果文章比较多的话还可以替换为 Mysql，另外数据备份也很简单，支持导入导出。Ghost 也为移动端做了适配，手机上的阅读体验也很不错。写起来也很方便，随时随地都能写，写完就能立即发布。自带的主题就很简洁美观，想换主题的话也很简单，直接从官网下载主题 zip 包，在管理面板上传并选为默认即可。

- 写作简单，随时随地都能写；
- 数据备份、导入导出非常容易；
- UI 简洁、美观，支持非常多的主题；
- 排版很棒，响应式布局，移动设备上表现良好；
- 控制面板用起来也很简单，相比 WordPress 非常轻量。

我的博客用的就是 Ghost 系统，如果你觉得不错，可以按下面的方法自己搭一个。当然如果不想折腾，也可以试试我的 [博客托管服务](http://slarker.me/ghost-write/)，剩下的工作交给我吧。

## 什么是 Docker？

Docker 是一个开源项目，诞生于 2013 年初，最初是 dotCloud 公司内部的一个业余项目。它基于 Google 公司推出的 Go 语言实现。 项目后来加入了 Linux 基金会，遵从了 Apache 2.0 协议，项目代码在 GitHub 上进行维护。

Docker 项目的目标是实现轻量级的操作系统虚拟化解决方案。 Docker 的基础是 Linux 容器（LXC）等技术。在 LXC 的基础上 Docker 进行了进一步的封装，让用户不需要去关心容器的管理，使得操作更为简便。用户操作 Docker 的容器就像操作一个快速轻量级的虚拟机一样简单。

相对于传统的虚拟技术，Docker 更轻量，更加节省资源，利用效率更高。更重要的是，可以极大的节省运维成本。下面我们就开始用 Docker 试着部署 Ghost 博客吧。

## 购买、新建 VPS

> 小尾巴：使用我的这个 [推荐链接](http://www.vultr.com/?ref=6914477-3B) 注册 Vultr 可以获得 20 刀余额。需要绑定 Paypal 或者信用卡。

首先我们需要准备好一台 VPS，推荐使用 [Vultr](http://www.vultr.com/?ref=6914477-3B) 东京机房的 VPS，5 刀一个月的就足够了，同等价格的 VPS 中，Vultr 的配置算是比较好的了，而且更重要的是网络质量也不错。Vultr 是按照使用时长计费的，也就是说，如果你彻底不用 VPS 的话，需要把 VPS 删除才能停止计费，请大家注意一下。

注册好账号之后，我们可以在控制面板里点击右侧的 `Deploy New Server` 按钮新建 VPS，第 1 步选择 `Tokyo` 机房。目前只有 `Tokyo` 机房的网络对于国内用户来说最好了，建议选择 `Tokyo` 机房。

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830253707364.png)

第 2、3 步选择操作系统为 `CentOS 7 x64` 和价格 `$5/mo`。

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830254376206.png)

接下来的 4、5、6、7 步都可以不用填。如果有了解第 6 步的 SSH Keys，并知道其用法的，可以自行填上，不了解的可以跳过。第 7 步我们可以为 VPS 起个名字，比如 `Ghost`。

最后我们点击右下角的 `Deploy Now` 按钮，就新建 VPS 成功了。稍等我们就可以在控制面板的 `Servers` 中看到已经启动的 VPS 了。

## 登录 VPS

如果你使用 macOS，自带的应用程序 `终端` 就可以登录 VPS，当然也可以用 iTerm2 之类的工具。如果是 Windows，可以用 Putty、Xshell 等工具登录 VPS。下面以 macOS 的 iTerm2 为例介绍如何登录到 VPS。

登录 VPS 需要使用 SSH 协议，SSH（Secure Shell）协议是一种在不安全网络上提供安全远程登录及其它安全网络服务的协议。也就是说通信过程中是加密的，大家也就不用担心密码被窃取。一般情况下，如果 VPS 没有特别提示，服务器的默认远程 `SSH` 端口是 22，我们可以用 22 端口进行远程登录。实际使用中，22 端口也一般省略不写。

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830702774829.png)

点击刚刚建好的 VPS，可以看到更详细的信息，下面可以看到 VPS 的 IP 地址、用户名和密码。

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830702331041.png)

下面我们在 `iTerm2` 或者 `终端` 中按如下格式使用 SSH 登录 VPS。

```
ssh root@45.76.99.242
```

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830730789707.png)

中间会提示 `Are you sure you want to continue connecting (yes/no)?` 输入 `yes`。然后会提示输入密码，将 VPS 密码复制一下，粘贴到这里就可以了，注意这里输入的密码不会显示 ***，然后回车，如果显示 `[root@ghost ~]#` 类似的字样，就表示登录成功了。

如果刚才添加过 ssh keys 的话，这时会发现不用输密码就已经登录成功了。对，这就是 ssh keys 的作用，因为我们本机有一对公钥和私钥，我们把公钥加到 VPS 里，就可以免去输入密码的麻烦。有兴趣的朋友可以自行 Google 了解下。

## 安装 Docker

登录成功后我们先安装 Docker，在终端中输入 Docker 安装命令（该命令仅适用于 CentOS 7）：

```
sudo yum install docker
```

如果是 CentOS 6.x，需要用如下命令安装：

```
sudo yum install http://mirrors.yun-idc.com/epel/6/i386/epel-release-6-8.noarch.rpm
sudo yum install docker-io
```

等待安装完成，我们启动 Docker 服务，并设置 Docker 为开机启动：

```
sudo service docker start
sudo chkconfig docker on
```

## 启动 Ghost

安装好 Docker 之后，我们就可以使用 [Ghost 官方 Docker 镜像](https://hub.docker.com/_/ghost/) 来启动 Ghost 了，首先需要使用下面的命令下载 Ghost 镜像：

```
docker pull ghost
```

下载完成后，我们可以使用这个命令来查看已经存在的 Docker 镜像：

```
docker images
```

如果 pull ghost 镜像时遇到 "Cannot connect to the Docker daemon. Is the docker daemon running on this host?"，可以先重启下 Docker 服务。

```
sudo service docker restart
```

接下来我们就可以启动 Ghost 啦！

```
docker run --name myghost -p 8080:2368 -d ghost
```

这个命令的含义是使用 Docker 基于刚刚下载的 ghost 镜像，创建一个名为 myblog 的实例，该实例的实际端口为 2368，映射到了 VPS 的 8080 端口上。因此，我们现在使用该 VPS 的 IP 地址加上 8080 端口，就可以访问 Ghost 博客了！

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830754654507.png)

## 配置域名

如果没有域名，那我们只能使用 IP 地址来访问我们的博客，而 IP 地址不便于记忆，所以为了能够访问起来方便，我们还是去申请个域名吧。

### 购买域名

目前国内主要的域名注册商有 [万网](https://wanwang.aliyun.com/)，国外的有 [godaddy](https://www.godaddy.com)、[namecheap](https://www.namecheap.com/) 等等，大家可以根据自己的需要自行注册域名。如果想问我哪里注册域名比较好的话，我推荐 godaddy 和 namecheap。

### 设置 [DNSPod](https://www.dnspod.cn) 域名解析

有了域名，我们可以将域名的 nameserver 映射到 DNSPod 上，使用 DNSPod 来解析，国内访问的速度也会快一些。

首先注册一个 DNSPod 账号，然后将注册好的域名加到 DNSPod 上，接着将 DNSPod 提供的 nameserver 添加到域名控制面板的 nameserver 中。以 namespace 为例：

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830745446969.png)

设置完成后，需要在 DNSPod 中添加记录：

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830782245368.png)

分别添加 `*`，`@`，`www` 类型的记录，记录值为 VPS 的 IP 地址。

### 使用 Nginx 为 Ghost 配置域名

为了给 Ghost 配置域名，我们要用到 Nginx 这个工具，所以先安装 Nginx 吧，命令也很简单：

```
yum install nginx
```
接下来我们编辑 Nginx 的配置文件：

```
vi /etc/nginx/conf.d/ghost.conf
```

进去后，在英文输入法下，按 `i` 进入编辑模式，这时会发现右下角变成 `-- INSERT --` 了。
接下来将下面这段配置中的 `abc.com` 换成自己的域名，然后复制粘贴进去，之后先按 `Esc` 返回，再按 `:`，进入命令模式，然后输入 `wq` 保存退出。

```
server {
listen 80;
server_name abc.com;

location / {
    proxy_set_header   X-Real-IP $remote_addr;
    proxy_set_header   Host      $http_host;
    proxy_pass         http://127.0.0.1:8080;
}
}
```

接下来重启 Nginx ：

```
service nginx restart
```

如果没问题的话，已经可以用域名访问博客了！

## 设置 Ghost

Ghost 博客的默认管理页面是你的博客域名或者 IP 后面加上 `/ghost`，第一次使用会让你设置邮箱和密码。进入之后，可以设置博客的名称，博客描述之类的，我在这里就不多说了，大家自己去玩玩吧。

需要注意的是，数据备份还是比较重要的，Ghost 提供了数据导入导出功能，建议大家每次写完博客后备份一下，防止由于意外导致数据丢失。

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830764110570.png)

## 开始使用 Ghost

左侧的 `New Post` 可以新增一篇文章，写完后点击右上角的设置，可以设置文章顶部图片，文章地址，写作日期等等。

![](http://7xpm6h.com1.z0.glb.clouddn.com/14830769319051.png)










