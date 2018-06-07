ubuntu16.04 + opencv3 + Nvidia 显卡驱动 + cuda8.0 + cudnn5.1 + caffe 安装 + python2
==
## 安装opencv3
直接使用pip install opencv-python 即可安装成功，如果需要一些拓展功能，则需要使用pip安装contrib版本。



## 安装Nvidia 显卡驱动

###1. 去Nvidia官网选择自己电脑显卡对应的型号的驱动，下载.run文件

### 2. 卸载原先所有的驱动：
```bash
sudo apt-get remobe --purge nvidia*
sudo chmod a+x *.run
```
### 3.禁用集成的nouveau驱动。
将驱动添加到黑名单blacklist.conf中，但是由于该文件的属性不允许修改。所以需要先修改文件属性。
```bash
查看属性
sudo ls -lh /etc/modprob.d/blacklist.conf
修改属性
sudo chmod 666 /etc/modprobe.d/blacklist.conf

sudo vim /etc/modprob.d/blacklist.conf
```
然后在文件后添加以下几行：
```bash
blacklist vga16fb
blacklist nouveau
blacklist rivafb
blacklist rivatv
blacklist nvidiafb
```
### 4.开始安装
先按Ctrl+Alt+F1到控制台，关闭当前图形环境
```bash
sudo service lightdm stop
```
安装驱动
```bash
sudo chmod a+x NVIDIA-LINUX...run
sudo ./NVIDIA.....run -no-x-check -no-nouveau-check -no-opengl-files
```
最后重启图形环境
```bash
sudo service lightdm start
```
### 5. 查看显卡驱动版本
可以通过以下命令确定驱动是否正确安装
```bash
cat /proc/driver/nvidia/version
```
或者
```bash
nvidia-smi
```
如果有一个表个显示自己的显卡型号，说明安装成功
![](./images/1.png)
**ps: 如果使用nvidia-setting 时 本人出现X-server错误的情况，但是按照网上很多教程没有能够弄好。但是对于后面用GPU进行训练没有什么影响。**

## 安装cuda8.0+cudnn5.1
cuda是nvidia的编程语言平台，想使用GPU就必须使用cuda。
###1. 下载cuda8.0源文件
[cuda官网](https://developer.nvidia.com/)
选择runfile(local)
###2. 安装
```bash
sudo sh cuda_8.0.......run
```
在安装的过程中，由于之前已经安装了nvidia的驱动，所以在是否安装驱动这一选项选择n其他直接默认护在哦和y就行。

###3.修改配置文件
修改启动bash的配置文件，将一些路经直接预加载。
```bash
sudo vim /etc/profile
```
在文件末尾添加以下语句
```bash
export PATH=/usr/local/cuda-8.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64$LD_LIBRARY_PATH
```
###4. 重启电脑

```bash
sudo shutdown -r now
```

### 5.测试cuda的Samples
```bash
cd /usr/local/cuda-8.0/samples/1_Utilities/deviceQuery
sudo make
./deviceQuery
```
![](./images/2.png)
 
### 6.安装cudnn
首先去官网下载cudnn，下载时需要注册帐号。然忽选择对应的cuda版本进行下载即可。

下载完cudnn之后，进行解压，然后将内部文件复制到cuda安装路经即可。
```bash
sudo cp cudnn.h /usr/local/cuda/include/    #复制头文件
```
然后在cd到lib64目录下进行复制链接：
```bash
sudo cp lib* /usr/local/cuda/lib64/    #复制动态链接库
cd /usr/local/cuda/lib64/
sudo rm -rf libcudnn.so libcudnn.so.5    #删除原有动态文件
sudo ln -s libcudnn.so.5.0.5 libcudnn.so.5  #生成软衔接
sudo ln -s libcudnn.so.5 libcudnn.so      #生成软链接
```
这样就完成了cudnn的安装

## 安装caffe
###1. 首先安装各种依赖包
```bash
sudo apt-get update 
sudo apt-get install -y build-essential cmake git pkg-config 
 sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler 
 sudo apt-get install -y libatlas-base-dev 
 sudo apt-get install -y--no-install-recommends libboost-all-dev 
 sudo apt-get install -y libgflags-dev libgoogle-glog-dev liblmdb-dev 
 sudo apt-get install -y python-pip 
 sudo apt-get install -y python-dev 
 sudo apt-get install -y python-numpy python-scipy 
```
###2. 然后从github上下载caffe源码
cd 到要安装的位置，然后：
```bash
git clone https://github.com/BVLC/caffe.git  //从github上git caffe
cd caffe //打开到刚刚git下来的caffe 
sudo cp Makefile.config.example Makefile.config   //将Makefile.config.example的内容复制到Makefile.config 
//因为make指令只能make Makefile.config文件，而Makefile.config.example是caffe给出的makefile例子 
sudo gedit Makefile.config //打开Makefile.config文件
```
###3. 对Makefile.config进行修改。

- 如果使用cudnn 则去掉cudnn前面的注释

- 如果使用的是opencv3 则去掉opencv3前面的注释

- //重要的一项 将# Whatever else you find you need goes here.下面的 INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib 
修改为： INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial 
 LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial //这是因为ubuntu16.04的文件包含位置发生了变化，尤其是需要用到的hdf5的位置，所以需要更改这一路径
      
- 可能需要修改numpy的安装路经，因为给的文档好像没有加local。

- 去掉CUDA_ARCH的前两行，不然在后面编译的时候会有警告。

- 其余默认即可

###4. 执行下面的语句来进行编译
```bash
make all -j8
make test -j8
make runtest -j8
make pycaffe -j8
```
###5.可能遇到的问题
error while loading shared libraries: libcudart.so.8.0: cannot open shared object file: No such file or directory

解决方法：
```bash
sudo cp /usr/local/cuda-8.0/lib64/libcudart.so.8.0 /usr/local/lib/libcudart.so.8.0 && sudo ldconfig
sudo cp /usr/local/cuda-8.0/lib64/libcublas.so.8.0 /usr/local/lib/libcublas.so.8.0 && sudo ldconfig
sudo cp /usr/local/cuda-8.0/lib64/libcurand.so.8.0 /usr/local/lib/libcurand.so.8.0 && sudo ldconfig
ps. ldconfig命令是一个动态链接库管理命令，是为了让动态链接库为系统共享
```
###6. 将caffe加到Python导入时的路经中

```bash
sudo vim ~/.bashrc
在文档末尾添加
export PYTHONPATH=/home/wangdh15/caffe_cuda_cudnn/python:$PYTHONPATH

重新加载配置文件
source ~/.bashrc
```


###7. 测试是否成功
```bash
python
import caffe
```
![](./images/3.png)

###8. 安装完成

----


## 参考文献
[Ubuntu16.04+cuda8.0+caffe安装教程](https://blog.csdn.net/autocyz/article/details/52299889)

[ubuntu16.04安装cuda8.0](https://blog.csdn.net/qq_40155090/article/details/79153530)