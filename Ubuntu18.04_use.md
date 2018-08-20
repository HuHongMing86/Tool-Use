### Ubuntu18.04使用
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
