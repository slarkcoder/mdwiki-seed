# 树莓派安装 MySQL

昨天爬到的数据都在 Windows 上的 MySQL 数据库里放着，但是以后用 Mac 访问就有点麻烦了，总不能就为了跑个 MySQL 就把 Windows 整天开着吧。最好还是把 MySQL 装到树莓派上，Windows 和 Mac 访问起来都方便，如果要在外网访问，也可以在路由器上开个端口转发。树莓派本身功耗就很低，最合适干这个事情了。

## 安装

登陆树莓派，先更新下系统：

```shell
sudo apt-get update && sudo apt-get upgrade
```

然后安装 MySQL，期间会提示设置密码：

```shell
sudo apt-get install mysql-server
```

安装好之后可以先测试一下是否 OK：

```shell
mysql -u root -p
```

进入 mysql 之后，可以使用 `exit` 退出。 

## 开启远程访问

MySQL 默认只能从 localhost 访问，所以我们需要开启远程访问：

```shell
sudo vim /etc/mysql/my.cnf
```

将 `bind-address = 127.0.0.1` 改为 `bind-address = 0.0.0.0`。

设置远程用户权限和密码：

```shell
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
```

然后重启 MySQL：

```shell
sudo /etc/init.d/mysql restart
```

## 解决中文乱码

由于 MySQL 默认编码是 `latin1`，所以中文会显示乱码，所以我们需要将编码方式修改为 `utf8`。

依然是编辑 my.cnf 这个文件：

```shell
sudo vim /etc/mysql/my.cnf
```

在 [client] 中添加：

```shell
default-character-set=utf8
```

在 [mysql] 中添加：

```shell
default-character-set=utf8
```

在 [mysqld] 中添加：

```shell
collation-server = utf8_unicode_ci
init-connect='SET NAMES utf8'
character-set-server = utf8
```

保存退出，并重启 MySQL：

```shell
sudo service mysql restart
```

登陆 MySQL，查看编码方式：

```mysql
mysql> show variables like 'character%';
```

可以看到编码方式已经都变成 utf8 了：

```mysql
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```