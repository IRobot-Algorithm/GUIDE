# rm算法组的最简docker入门文档


| 版本 | 日期       | 人员                     | 修改记录                                                     |
| ---- | ---------- | ---------------------- | ------------------------------------------------------------ |
| v1.0 | 2024-2-04 | 池威律, 694621753@qq.com  | 添加rm算法组的最简docker入门文档          |


### 前言

这是一份适用于**rm算法组的最简docker入门文档**，列出了最常用的docker命令。

- [docker的官方文档](https://docs.docker.com/guides/get-started/)内容丰富，但是对于初学者来说可能过于丰富了。
- DockerFile示例及Docker部署使用可查看这两个仓库[Vision_Control](https://github.com/IRobot-Algorithm/Vision_Control) ， [rm_vision](https://gitlab.com/rm_vision/rm_vision)



### docker的作用

- 方便搭建各种不同环境，可以屏蔽环境差异。

- 方便代码快速部署。

- 有了docker就不用一个nuc接一个nuc地配环境了，只需拉取镜像运行即可。修改环境也是非常简便，直接改`Dockerfile`或`docker push`修改好的`image`即可



### docker重要概念

docker最重要的三个概念为**Dockerfile**、**image**、**container**。这篇[文章]( https://zhuanlan.zhihu.com/p/187505981)较详细地解释了三者的概念。

- 一种常见流程是

  1.开发者将代码的运行环境写入`docker/Dockerfile`文件，在Dockerfile中指定需要哪些程序、依赖什么样的配置,与开发的代码一同发布在github上。
  
  2.使用者通过`git clone`命令获得代码及`docker/Dockerfile`文件。
  
  3.根据`docker/Dockerfile`文件使用`docker build`命令得到一份`image`
  
  4.根据得到的一份`image`使用`docker run`命令就可得到无数`container`（在内存允许的情况下）。可以设置自动启动代码，也可以手动进入终端编译或执行代码。

- 另一种常见流程是

  1.开发者将代码环境配置打包，`docker push`到`dockerhub`或`ghcr`等处
  
  2.使用者从`dockerhub`或`ghcr`处`docker pull`拉取`image`
  
  3.根据得到的`image`使用`docker run`命令就可得到`container`运行代码





### docker安装命令

  ``` 
  # 从Docker的官方网站下载安装脚本
  curl -fsSL https://get.docker.com -o getdocker.sh
  sudo sh ./getdocker.sh
  
  # 到此docker已经安装结束，在nuc上安装docker建议继续执行以下命令
  # 将当前登录的用户添加到'docker'组允许该用户在不使用sudo的情况下运行Docker命令
  sudo usermod -aG docker $UESR
  # 将当前会话的用户切换到指定的docker组
  newgrp docker
  # 设置 Docker 服务在系统启动时自动启动
  sudo systemctl enable docker.service
  ```



### docker运行命令

  ``` 
  # 在与Dockerfile同级的目录下根据Dockerfile编译镜像
  docker build -t vc_image .
  # 根据已编译好的镜像创建容器并运行，示例中容器名为vc_devel，镜像名为vc_image
  docker run -it --name vc_devel \
  --privileged --network host \
  -v /dev:/dev   \
  vc_image \
  
  # 创建过容器后，下次只需start
  docker start vc_devel
  # 增加更多终端
  docker exec -it vc_devel bash
  # 停止容器
  docker stop vc_devel
  # 退出docker容器
  exit
  ```

### docker发布与拉取命令

  ``` 
  # 这里以ghcr为例，GHCR是由GitHub提供的用于存储Docker镜像的服务。
  docker pull ghcr.io/shitoujie/vision_control:latest
  ```
- related links: [ghcr 快速上手教程](https://blog.csdn.net/easylife206/article/details/108480444?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170424990016800188531527%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=170424990016800188531527&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-108480444-null-null.142^v99^pc_search_result_base4&utm_term=github%20ghcr&spm=1018.2226.3001.4187)

  

### docker相关删除命令

  ``` 
  docker ps -a
  # 删除容器（container）
  docker rm [CONTAINER ID]
  docker images 	#REPOSITORY下的即为image name
  # 删除镜像（image）
  docker image rm [IMAGE NAME] 
  ```



