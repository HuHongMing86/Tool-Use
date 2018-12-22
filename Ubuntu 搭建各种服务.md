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

##### 2.1 关于apache2的配置问题
 
 首先，如果需要修改监听端口的话，需要先修改`etc/apache2/`文件夹下的`port.conf`文件，然后修改`sites-availavle/000-default.conf`文件，
 将第一行的端口修改为对应的端口，然后这个文件同时配置了站点的目录，修改`DocumentRoot`可以修改地点。
 
 `apache2`可以同时开放多个端口然后提供多个网站服务。这是需要修改一下配置文件：
 
 - `port.conf`。将打开的端口添加到Listen port_number 中。
 - `site_availavle/****.conf` ，可以复制默认的`000-default.conf`，然后修改端口和站点目录来指向新的网站。
 - `site-enabled/***.conf` ，在这个文件夹中建立软连接连接到上面创建的`***.conf`文件。
 
 然后重启apache2服务`sudo service apache2 restart`就可以看到对应的端口打开同时通过浏览器就能够访问到自己的站点。
 
 
#### 3. 搭建shadowsocks服务

- 首先安装pip `sudo apt-get install python-pip`
- 然后安装 shadowsocks `pip install shadowsocks`

然后配置Shadowsocks
```

 {                                                                                                                                                 
     "server": "0.0.0.0",                  # 这一行感觉在服务器端也没什么作用。。。。。                                                                                               
     "local_address": "127.0.0.1",         # 感觉这个和下面的那个没有什么作用，删掉了页没关系。。。                                                                                                    
     "local_port": 1080,                  # 感觉这一行没什么用。。。。。 

     "timeout": 600,                                                                                                                               
     "method": "aes-256-cfb",                                                                                                                      
     "port_password":                                                                                                                              
     {                                                                                                                                             
         "5200":"1234",    # 需要开设的端口喝密码                                                                                                                        
         "9999":"1234"                                                                                                          
     }                                                                                                                                             
 } 
 
```

然后启动shadowsocks
```
# 启动
ssserver -c /etc/shadowsocks.json -d start

# 停止
ssserver -c /etc/shadowsocks.json -d stop

# 重启
ssserver -c /etc/shadowsocks.json -d restart

```
可以设计开机自启动：
修改`/etc/rc.local`，然后在其中添加启动ss的命令：`ssserver -c /etc/shadowsocks.json -d start`即可。

#### 4.远程访问jupyter notebook
１　生成一个notebook配置文件
`jupyter notebook --generate-config`

2 生成密码：
`jupyter notebook password` 生成的密码在`.jupyter/jupyter_notebook_config,json`

3 修改配置文件
`jupyter_notebook_config.py`
```
c.NotebookApp.ip='*'
c.NotebookApp.password = u'sha:ce...刚才复制的那个密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port =8888 #可自行指定一个端口, 访问时使用该端口
```
然后就可以使用`jupyter notebook`打开notebook，然后使用ip:8888访问jupyter notebook.

#### 5 搭建git服务器

1 首先安装git，然后创建一个git用户。

2 在git用户下面创建下面的文件`.ssh/authorized_keys`,并将可以登录的用户的公钥存入，一行一个。

3 在合适的位置创建仓库，然后远程使用`git clone ssh://ip:port/file_path`来访问。

#### 6 搭建个人博客

具体操作见[链接](https://blog.csdn.net/qq_31714339/article/details/78237512)
