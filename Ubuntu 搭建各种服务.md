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
