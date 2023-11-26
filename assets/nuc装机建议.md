
# nuc装机建议
作者：蒙亦帆

2023.11.26修改：段壮壮
## 前言

建议先了解一些linux的终端命令和cmake基础再阅读本文档
.md文件已上传github，如有疑问欢迎联系我们

---
## 装前警示
**不要**随便使用apt-get autoremove!!!

若根据创建文件夹时(mkdir)发现文件夹已存在,则删除原有文件夹再创建.

### 1 基础配置(g++,gcc,make,cmake)

参见[博客](https://blog.csdn.net/FL1623863129/article/details/123344607?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_utm_term~default-0-123344607-blog-50853196.235^v38^pc_relevant_default_base3&spm=1001.2101.3001.4242.1&utm_relevant_index=3)

注意cmake安装建议使用第一种方法.若采用第二种方法,cmake-3.25.2-linux-x86_64.tar.gz已被提供,注意cmake 版本.

### 2 vscode
安装.deb文件就行

```sudo dpkg -i code_1.79.2-1686734195_amd64.deb```

（或在应用商店里下载安装）
### 3 opencv

建议安装4.5.5(4.4.0版本还没有安装过)

参见[博客](https://blog.csdn.net/zooo520/article/details/124586567)

opencv的依赖一直以来是很麻烦的东西,注意哪些依赖必需，哪些可选。可选依赖一般来说opencv会有默认自带依赖。

注意opencv-contrib的版本一定要与opencv版本一致。官网上下载opencv-contrib后，将其放进之前的opencv安装包中一起编译。所给opencv-4.5.5里面包含了opencv_contrib-4.x,无需下载直接cmake编译即可.
### 4 eigen-3.4.0
查看INSTALL:
Method 2. Installing using CMake
 \********************************

Let's call this directory 'source_dir' (where this INSTALL file is).
Before starting, create another directory which we will call 'build_dir'.(build_dir上级文件夹为eigen-3.4.0)

Do:
```
  mkdir build_dir
  cd build_dir
  cmake source_dir
  sudo make install
 ```
The "make install" step may require administrator privileges.

安装好后,编译程序会出现编译器找不到eigen库的情况.这是因为安装完成后，编译器会去/usr/local/include或者/usr/include目录找头文件，但找到的是eigen3，并没有Eigen和unsupported，所以需要建立一个软连接链接到这两个文件夹即可.

要先确定你的Eigen3安装在 **/usr/local/include** 还是 **/usr/include**
如安装在了后者
cd /usr/include
sudo ln -sf eigen3/Eigen Eigen
sudo ln -sf eigen3/unsupported unsupported

5）ceres-solver-2.1.0(现在最新版本可能是2.2.0了，实测也没问题)
参见[官网](http://www.ceres-solver.org/installation.html#linux)
将ceres-solver-2.1.0.zip解压后,进入该文件,运行以下几个命令:

```
sudo apt-get install libgoogle-glog-dev libgflags-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install libsuitesparse-dev

mkdir ceres-bin
cd ceres-bin
cmake ..
make -j3
make test
sudo make install
 ```

ceres，eigen与opencv三者之间耦连性紧密；程序中eigen库的报错很可能是ceres没装或是三者版本对应不上


6）Galaxy(相机驱动)
	将Galaxy_Linux-x86_Gige-U3_32bits-64bits_1.1.1905.9241.zip解压后进入该文件,运行Galaxy_camera.run文件:
	./Galaxy_camera.run

后续选项选项如下:
![](https://bozhiblogimage.oss-cn-beijing.aliyuncs.com/pic/18266a515ad3dd0675eebc2bdc3e3a90.JPG)

注意重启之后相机驱动才生效.

## 7 ros2安装
鱼香ros一键安装:
```wget http://fishros.com/install -O fishros && . fishros```
按键选择:
\[1]:(一键安装:ROS(支持ROS和ROS2,树莓派Jetson)

\[1]:更换系统源再继续安装
\[2]:更换系统源并清理第三方源
\[2]:foxy(ROS2)       **看准是foxy**
注意：若ubuntu版本为22，可能此处没有foxy

ros2安装完成之后,仍无法定位camera-info-manager包,则输入
```sudo apt install ros-foxy-camera-info-manager ```

## 8 opencl与openvino
    注意操作顺序！
**安装opencl:**
```
cd nn_environment/opencl/
bash install download_opencl.sh
bash install_opencl.sh
 ```

**安装 openvino:**
注意先运行ovrt的sh文件,再运行ovdt的sh文件
```
cd nn_environment/openvino/
cd ovrt
bash download_opencl.sh
bash install_ovrt.sh
cd ..
cd ovdt
bash install_ovdt.sh
 ```
重启ubuntu后出现:
```[setupvars.sh] OpenVINO environment initialized ```
则安装完成.


**至此，环境配置完毕**

---
## 后续
### 编译程序 :RMOS_Infantry_dl
将RMOS_Infantry_dl放置在Desktop目录下
colcon build
若无报错,则编译通过
### 自启动程序
脚本为watchdog.sh,稍后发布

在主目录新建sh文件,将wachdog.sh放入该文件.

脚本赋能:进入脚本所在文件夹,输入:

sudo chmod 777 watchdog.sh

ls

若ls命令查看文件,watchdog.sh文件名变成绿色,则赋能成功.

使用gnome-session-properties 设置自启动:

gnome-session-properties

点击add,

Name:watchdog

Command: gnome-terminal -e 'bash -c "/home/nuc12/sh/watchdog.sh" '

再点add.

将RMOS_Infantry放在Desktop下.

若重启后弹出终端,则自启动设置成功.

若未弹出,可能是watchdog.sh路径出现问题.



### 上电开机设置
参看[博客](https://blog.csdn.net/qq_43613216/article/details/128864770?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168861360816800192285774%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168861360816800192285774&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-128864770-null-null.142^v88^insert_down1,239^v2^insert_chatgpt&utm_term=ubuntu%E4%B8%8A%E7%94%B5%E8%87%AA%E5%8A%A8%E5%BC%80%E6%9C%BA&spm=1018.2226.3001.4187)
