## Ubuntu 搭建各种服务

### 王道烩 2018.12.20

#### 1. seafile 个人云盘。

- 首先安装MySQL，`sudo apt-get install mysql-server`，输入root用户的密码即可。

- 然后登录seafile官网，找到[安装教程](https://manual-cn.seafile.com/deploy/using_mysql.html), 按照安装教程安装依赖包建立数据库即可。
- 设置防火墙，打开服务器的8000端口(用于登录)和8082端口(用于同步)，使用域名:8000就可以登录然后管理自己的云盘，创建用户等等。

#### 2. 搭建个人主页

- 首先安装apache2`sudo apt-get install apache2`
- 然后会在`/var/www/html/文件夹下，这里是存放站点的文件夹。`
- 配置文件在`/etc/apache2/`文件夹下，如果需要修改端口的话，可以修改`port.conf`文件，默认是80端口。
