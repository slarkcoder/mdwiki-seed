很早之前就已经听说过 [树莓派](https://www.raspberrypi.org/) 了，价格便宜，功能强大，是学习编程，折腾玩机的神器。只需要 200 多 RMB，就可以拥有一个能安装 Linux 或者 Windows 10 IoT 定制版的树莓派，可以做个 NAS、下载机，或者安装 Nginx 搭个博客都是轻而易举。

昨天在淘宝买了个树莓派 3，今天一大早就收到了，屁颠屁颠地装好系统，开始了折腾之路。。。

另外大家购买树莓派的时候注意以下几点：

- 尽量不要购买亚巧克力板的外壳，安装的时候很容易损坏；
- 风扇声音有点大，可以选择稍微好点的风扇；
- 电源最好用 5V 2A 的，接口为 MicroUSB，一般安卓机的电源就可以用，有多余的电源就不用额外购买了；
- TF 卡最好使用 Class 10 规格的，可以加快启动速度，安装完毕后系统占用不到 4G ，8G 或者 16G 的 TF 卡就够了，建议单独购买。

## 树莓派的私房照

包装盒
![WechatIMG3](http://7ktosw.com1.z0.glb.clouddn.com/2016-08-28-WechatIMG3-1.jpeg)

开箱照
![WechatIMG4](http://7ktosw.com1.z0.glb.clouddn.com/2016-08-28-WechatIMG4.jpeg)

安装好外壳了
![WechatIMG1](http://7ktosw.com1.z0.glb.clouddn.com/2016-08-28-WechatIMG1-1.jpeg)

## 配置

- 处理器：BCM2837 四核处理器（ARM Cortex-A53 1.2GHz）
- 内存：1GB RAM （900MHz）
- GPU：VideoCore IV GPU（400MHz）
- 内置 WiFi，蓝牙 4.1

## 系统介绍

树莓派官方的系统为：Raspbian，你可以单独下载 Raspbian 的镜像，然后刻录到 TF 卡中启动。官方推荐使用 NOOBS 来安装系统。

NOOBS 全称为 New Out Of Box Software (NOOBS)，不仅可以非常方便的让一张空白的 TF 卡摇身变为安装了 Raspbian 系统的启动盘，而且还可以预包装其他可选的树莓派操作系统，比如 Pidora（基于Fedora 的系统）、RISC OS 、Arch（Arch Linux 的树莓派版）。

## 格式化 TF 卡

安装系统前首先要格式化 TF 卡，最好格式化成 FAT32 格式的，开始我使用 Windows 自带的格式化工具格式化，但是在启动的时候出现问题。所以最好按照官方的建议，使用 [SD Formatter](https://www.sdcard.org) 工具格式化。

## 启动系统

下载 [NOOBS](https://www.raspberrypi.org/downloads/noobs/)，然后解压到 TF 卡的根目录。将 TF 插入树莓派的卡槽处，插上电源系统就可以启动了。第一次启动会稍微慢点，稍等几分钟会有一个选择系统的列表，你可以选择 Raspbian 和其他的系统来进行安装。安装系统大约需要 10分钟左右，安装完毕之后会自动重启。

## 安装中文字体

树莓派默认语言环境是英文，使用浏览器的时候，你会发现有的地方显示乱码，因为没有安装中文字体。我们可以使用下面命令安装中文字体：

```
sudo apt-get install ttf-wqy-zenhei
```

完成之后重启。

## 修改用户密码

树莓派的默认用户是 `Pi`，密码为 `raspberry`，可以使用

```
sudo passwd pi 				#修改 Pi 密码
sudo passwd root 			#设置 root 密码
sudo passwd --unlock root	#解锁 root 用户
su root 					#切换到 root 用户
su pi 						#切换到 pi 用户
```

来更改 Pi 或者 root 用户密码、切换用户等操作。

## 使用 SSH 连接树莓派

用户密码设置好之后，就可以用 SSH 连接了。在路由器里，可以找到树莓派的 ip，使用 putty 填好 ip 之后，点击连接进去，输入用户名和密码，就能连上了。😄

接下来，就可以做一些自己想做的东西了！


