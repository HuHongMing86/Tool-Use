### Ubuntu18.04   Deepin使用
#### 安装
安装的时候主要是Nvidia显卡问题。
首先安装的时候，在install ubuntu按e，然后在倒数第二行末尾的quiet splash 的后面空一格加上acpi_osi=linux nomodeset .如果在quiet splash 后面有---.直接删除就行。然后按F10进行安装。
安装完成之后，可能会在输入密码之后卡住，进不去桌面，这个时候需要现在系统启动的时候，按e 然后在倒数第几行的quiet splash后面加上nomodeset.然后就可以启动。这个时候查看一下about this computer，如果使用的不是intel的显卡或者Mvidiad的显卡，可以编辑下面的配置文件来禁用第三方驱动。
```bash
sudo gedit /etc/modprobe.d/blacklist.conf

```
然后进行驱动的安装，这个时候Ubuntu18.04十分友好，直接执行下面的命令即可

```bash
sudo apt-get update
sudo apt-get upgrade
sudo ubuntu-drivers autoinstall
```

当然为了提升速度，先将软件源换成清华源。具体操作可参考16.04使用。


#### deepin 安装驱动

首先进入系统，遇到问题见这个[文档](https://blog.csdn.net/HuaCode/article/details/83216338) 

进入系统之后就安装驱动使用`sudo apt-get install nvidia-smi`会自动安装很多东西。

然后使用deepin自带的显卡管理方法的软件，选择独显即可。然后按照指导完成安装。

如果使用搜狗输入法出现问题，将家目录下的关于搜狗输入法的配置文件`.config/...   .sougou...`等 全部删除，然后再重启。

### 硬盘分区

`sudo fdisk -l` 查看自己的设备是否被识别到。

`sudo fdisk /dev/sda` 然后按照提示进行操作。

- 输入P查看现有分区
- 输入d删除分区
- 输入n划分新分区

然后按照提示操作下去。

然后格式化磁盘

`sudo mkfs.ext4 /dev/sda1`

然后使用`mount`命令将其挂载到一个文件夹上

`sudo mount /dev/sda1 ~/File`


如果想要以后开机自动挂载，那么需要修改`/etc/fstab`

然后按照其中的格式添加一行即可。

### deepin使用pytorch

首先安装cudatoolkit  cudnn 对应的版本

然后需要安装一个库`sudo apt-get install nvidia-opencl-dev` ,然后就可以使用了。

#### 常用软件
其他常用软件的安装参考16.04.

#### anaconda
使用anaconda来替代系统本身自带的环境，而且能够创造多个虚拟的python环境。

- 去清华开源镜像下载anaconda3
- 直接运行.sh文件进行安装，然后再修改.bashrc，将bin加入PATH变量。
- 添加清华镜像作为anaconda的源。

具体操作可以查看下面的文档;
[Anaconda镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)


#### anaconda常用命令
查看当前环境安装的库
```bash
conda list
```

安装库
```bash
conda install numpy 
```

查看信息

```
conda info
```

创建新的虚拟环境

```
conda create -n your_env_name python=X.X(2.7 3.6)

```
上述命令创python版本为X.X 名字为your_env_name的虚拟环境。your_env_name问价可以在Anaconda安装目录envs文件下找到。

使用激活(切换不同版本的Python)的虚拟环境

```
source activate your_env_name(虚拟环境名称)
```
然后使用python --version来查python版本是否是自己想要的

关闭虚拟环境
```
source deactivate

```

删除虚拟环境

```
conda remove -n your_env_name --all
```
对虚拟环境中安装额外的包。
如果已经在虚拟环境中，那么直接conda install即可。如果不在虚拟环境中，那么可以使用下面命令：
```
conda install -n your_env_name package
```
可安装package到your_env_name中

删除虚拟环境中的某个包
```
conda remove --name your_env_name package_name 

```
#### 参考文献

[用conda创建python虚拟环境](https://blog.csdn.net/lyy14011305/article/details/59500819)



#### 安装使用tensorflow
如果安装好了N卡驱动，那么可以直接使用下面命令安装tensorflow
```
conda install tensorflow-gpu
codna install cudnn
```
其中安装cudnn可以安装cudatoolkit以及cudnn。

### 桌面美化
可以参考下面文章：
[Ubuntu18.04美化](https://www.cnblogs.com/lishanlei/p/9090404.html)

### git 
首先建立本地的文件夹 然后在文件夹内部使用下面的命令进行初始化：
```bash
git init
```
然后将远程仓库与本地链接起来
```bash
git remote add origin git@github.com:michaelliao/learngit.git
```

然后可以讲本地的文件全部上传
```bash
git push -u origin master
```
从现在起，只要本地作了提交，就可以通过命令同步：
```bash
git push origin master
```

#### git 常用命令：
```bash
git add .
git commit -m "..."
git status
git push origin master
```

### DownGit

一块能够将github某一个文件的链接放入，下载固定文件的网址。

[DownGit](https://minhaskamal.github.io/DownGit/#/home)

### tmux 

tmux是一款终端分屏工具，可以复用终端

安装`sudo apt-get install tmux`

快捷键：

Ctrl+b " - split pane horizontally

Ctrl+b % - 将当前窗格垂直划分

Ctrl+b 方向键 - 在各窗格间切换

Ctrl+b，并且不要松开Ctrl，方向键 - 调整窗格大小

Ctrl+b c - (c)reate 生成一个新的窗口

Ctrl+b n - (n)ext 移动到下一个窗口

Ctrl+b p - (p)revious 移动到前一个窗口.


Ctrl+b 空格键 - 采用下一个内置布局 

Ctrl+b q - 显示分隔窗口的编号 

Ctrl+b o - 跳到下一个分隔窗口 

Ctrl+b & - 确认后退出 tmux 

**启动鼠标功能**

在tmux中默认是不能够通过鼠标进行翻页什么的，但是我们可以通过修改tmux配置文件来开启这个功能
```
vim ~/.tmux.conf
```

然后插入`sset-option -g mouse on ` 在source一下即可。

#### 复制粘贴

在tmux中，可以使用下面的快捷键进行目标的选中，复制以及粘贴

- 先按下Ctrl+b 然后按下\[ 进入复制模式
- 然后移动光标到想复制的开始的地方
- 然后按下Ctrl+Space，开始选择，移动光标，选择的部分会高量
- 然后到末尾按下Ctrl+W,结束选择，
- 按Ctrl+B 再按\]进行粘贴

但是这个缓冲区和系统的粘贴板缓冲区不是一个。如果想使用系统的缓冲区的话，使用`Ctrl+Shift+V`键进行粘贴。

### Windows 下的txt的乱码问题

在windows下编辑的txt文件在Linux下直接双击打开会出现乱码，出现这种情况的原因为两种操作系统的中文压缩方式不同，在windows环境中中文压缩一般为gbk，而在linux环境中为utf8，这就导致了在windows下能正常显示
txt文件在linux环境下打开呈现了乱码状态。

解决方法：

在linux用iconv命令，如乱码文件名为shujujiegou.txt，那么在终端输入如下命令：

```
iconv -f gbk -t utf8 shujujiegou.txt > shujujiegou.txt.utf8
```

然后就可以解决乱码问题了。

**PS**：在Linux下用vim打开不会出现乱码的问题。

### 系统资源监视器

在窗口右上角可以用来监视系统资源占用情况

```bash
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor
sudo apt-get update
sudo apt-get install indicator-sysmonitor

Search in the dash for "indicator-sysmonitor" to run
```

[Github链接](https://github.com/fossfreedom/indicator-sysmonitor)

### 便签 Xpad

```bash
sudo apt-get install xpad
```
可以放在桌面上的一款比较好的便签。

### 资源监视器

conky是一款比较好的系统资源监视器。安装conky-manager可以更好地配置，但是需要先安装realpath，然后才能够安装成狗conky-manager.

安装conky只需要运行

```bash
sudo apt-get install conky
```
[具体安装过程见此视频](https://www.youtube.com/watch?v=Xoez7OYFyeY)


### Vim

vim 是linux自带的一款编辑器，为了能够支持代码不全功能，更好的使用，可以在家目录下面添加一个`.vimrc`文件，然后写入配置文件，参考配置文件见下面的仓库。

[.vimrc配置文件参考](https://github.com/fisadev/fisa-vim-config)

[awe vim 配置](https://github.com/amix/vimrc)
现在到家目录中，然后运行install_awesome_vimrc.sh即可生成需要的.vimrc，然后在其中添加提示的信息来取消显示错误信息。

### Transmission

一款下载软件，可以用作北邮人做种使用。安装方法见搜索引擎。可直接`sudo apt-get install`即可。

### Terminus

一款跨平台的终端以及SSH远程登录软件。直接官网下载即可。

### SSH登录管理

#### 1. 免密码登录

每次使用ssh登录都需要输入密码，可以在要登录的用户家目录下建立一个`.ssh`文件夹，然后建立一个`authorized_keys`文件

将自己本地电脑上的公钥放到这个文件中，每一行放一个公钥，然后在下一次登录的时候就能够免密码登录了。

#### 2. 免登陆执行指令

而且如果只是登录上服务器然后执行固定的指令的话，可以使用ssh命令的下面格式：

`ssh username@hostname "codeToRun"`

装就能够执行对应的指令了，如果指令太多，可以在服务器端写一个shell脚本，然后执行这个shell脚本即可！

### 科学上网

首先安装shadowsocks，直接在应用商店安装jike。

然后配置chrome浏览器。先安装SwitchyOmega插件，由于67之后的浏览器不允许推动安装，所以参考[这篇文章](https://blog.csdn.net/achenyuan/article/details/81485092) 将插件变后缀名为zip然后解压，然后打开开发者模式，加载文件夹即可。

然后配置插件，使用这个[教程](https://shimo.im/doc/5RxSHM3CeHQT5Eys/)即可。
