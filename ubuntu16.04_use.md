### 配置Ubuntu16.04
#### 安装
使用U盘进行安装，然后进入boot，选择从U盘启动。但是此时可能会卡在logo界面，这时可能是因为显卡驱动问题。这个时候需要在install ubuntu选项上按e，然后在倒数第二行末尾的quiet splash 的后面空一格加上acpi_osi=linux nomodeset .如果在quiet splash 后面有---.直接删除就行。然后按F10进行安装。

在系统分区的时候，选择500MB给boot分区，1G给swap分区，剩下的都给/分区，然后进行安装。

安装完成之后，可能会在输入密码之后卡住，进不去桌面，这个时候需要现在系统启动的时候，按e 然后在倒数第几行的quiet splash后面加上nomodeset.然后就可以启动。这个时候查看一下about this computer，如果使用的不是intel的显卡或者Mvidiad的显卡，可以编辑下面的配置文件来禁用第三方驱动。
```
sudo gedit /etc/modprobe.d/blacklist.conf
```
然后在文件末尾加上blacklist nouveau.
然后重启系统，此时应该能够正常进入桌面，查看about this computer应该可以看到intel的显卡驱动。

如果想要安装Nvidia显卡驱动，可以查看我的caffe_install文章。

这个时候系统就正常安装完成了。

#### 修改软件源
参考下面这篇文章：

[Ubuntu修改为清华大学源](https://blog.csdn.net/dearsq/article/details/51492847)


#### 修改主题

参考下面这篇文章：
[ubuntu16.04主题美化和软件推荐](https://blog.csdn.net/xw12138/article/details/78005554)


#### 安装ss
参考下面这篇文章：

[Ubuntu16.04下配置shadowsocks](https://blog.csdn.net/bevison/article/details/79384414)


#### 安装google chrome 

参考百度经验：

[在 Ubuntu 16.04 中安装谷歌 Chrome 浏览器](https://jingyan.baidu.com/article/335530da98061b19cb41c31d.html)

#### 推荐安装的常用软件：

##### 搜狗输入法
输入法，去官网下载deb包安装即可，缺少的库直接
```
sudo apt-get install -f
```
即可安装成功。安装完成之后，选择fitc，然后logout，在输入法配置中添加sogou 然后就可以使用，shift +Ctrl进行切换。

##### guake 
超级好用的终端， apt-get 安装就行

##### Remarkable
好用的markdown编辑器。 官网下载deb包包安装就行。

##### IPython
Python的交互界面，功能比较强大。 apt安装就行。

##### PyCharm
Python的IDE，官网下载安装包安装即可。

##### 网易云音乐
音乐播放软件，去官网下载即可。 安装玩之后想要使用的话直接在调节音量的地方就能够启动。

##### shutter
截屏软件，apt安装即可。

##### sublime text
编辑软件，去官网按照教程安装即可。



##### WPS
文档编辑，表格，PPT制作工具，去官网下载deb包进行安装即可。但是会缺少字体。具体解决方法参考下面的文章：

[Ubuntu16.04安装wps并解决系统缺失字体问题](https://blog.csdn.net/zmken497300/article/details/77531982)

##### LaTeX
直接apt安装就行 

```
sudo apt-get install texlive-full
sudo  apt-get install texstudio
```
然后打开texstudio，将build改成xdlatex，然后就可以使用中文了。


##### 微信
直接在网页版微信通过google chrome 转化为应用即可。

##### 添加开机自启动
参考下面的文章，一般将guake和ss添加到开机自启动当中。
commad一般在/usr/bin下面找 就行。

[ubuntu 开机自启管理](https://blog.csdn.net/qiangwei1212/article/details/68923394)

