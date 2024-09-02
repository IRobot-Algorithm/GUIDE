# 其他 docker 常用方法

| 版本 | 日期       | 人员                     | 修改记录                                                     |
| ---- | ---------- | ---------------------- | ------------------------------------------------------------ |
| v1.0 | 2024-09-01 | 关超, 23049200367@qq.com  | 添加其他docker常用方法文档          |

## ---共享图形化界面---
### 原理简介
与访问宿主机设备一样，显卡在默认情况下是无法被容器访问的，所以需要在启动的时候手动给容器添加访问宿主机的权限
### 在docker容器中安装X11
```shell
sudo apt-get install x11-xserver-utils
```
### 开放宿主机权限，允许所有用户,访问X11的显示接口
```shell
xhost +
```
### 启动docker时添加下面的参数
```shell
 -v /tmp/.X11-unix:/tmp/.X11-unix \           #共享本地unix端口

 -e DISPLAY=unix$DISPLAY \                    #修改环境变量DISPLAY
```
## ---访问宿主机设备---
### 原理简介
docker 容器默认情况下没有使用宿主机设备的权限，所以在启动容器的时候需要手动将权限赋予容器
### 在/dev目录下面可以看到所有设备
```shell
docker run -it \
    --device=/dev/video0 \
    --privileged \
    --ipc=host \
    --net=host \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -e DISPLAY=unix$DISPLAY \
    docker-image
```
### 参数说明
 **`-it`**: 这两个选项一起使用时会启动一个交互式的终端会话。
- **`-i`**: 保持标准输入流打开，即使没有连接到终端。
- **`-t`**: 分配一个伪终端。

**`--device=/dev/video0`**: 将宿主机上的设备文件 `/dev/video0` 映射到容器中。这样容器内的进程可以访问和使用该设备。通常用于访问视频设备，如摄像头。

 **`--privileged`**: 以特权模式运行容器。这将授予容器几乎完全的访问权限，包括设备和所有的内核功能。
 
**`--ipc=host`**: 使容器与宿主机共享 IPC 命名空间。这使得容器可以直接访问宿主机的进程间通信设施，如共享内存和信号量。

**`--net=host`**: 使用宿主机的网络命名空间。这意味着容器将共享宿主机的网络接口和 IP 地址。这样可以使容器内的应用程序直接使用宿主机的网络设置，而不需要通过虚拟网络桥。
## ---在容器中使用显卡---

### 安装NVIDIA Container Toolkit
[参考链接](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
### 配置仓库
```shell
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
### 开始安装
```shell
sudo apt-get update
```
```shell
sudo apt-get install -y nvidia-container-toolkit
```
### 启动容器添加参数
```shell
--runtime=nvidia
--gpus all
```
### 如果出现找不到nvidia的runtime
```shell
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```
### 想要在docker里面使用 cuda 最方便的是直接pull docker hub上的官方镜像
[docker hub nvidia 官方链接](https://hub.docker.com/r/nvidia/cuda/)

直接拉取官方镜像兼容性会更好一些，如果拉取出现问题可以看看后面的方法
## ---拉取docker image遇到网络问题---（弃用，可以使用鱼香ros的一键安装中的docker代理解决此问题）
在进行对image的pull的时候可能会遇到由于网络问题无法pull的情况(包括开全局梯子也不行)
这种情况下可以使用[skopeo](https://github.com/containers/skopeo)工具进行pull
### 更新 也可以用 fishros的一键安装系列 更新docker的代理
### 1,Ubuntu(20.10以上)环境下输入下面的命令
```
sudo apt-get -y update
sudo apt-get -y install skopeo
```

### 2,使用 skopeo
```
skopeo copy docker://<image_path> docker-archive:<image_name>.tar
```
直接将image下载为tar
### 3,将 image 导入 docker
```
docker load -i <image_name>.tar
```
### 4,给 image加一个标签
```
docker tag <image_id> <image_path>
```
### 5,完成可以直接run镜像了
