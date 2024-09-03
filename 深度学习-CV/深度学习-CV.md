## 深度学习-CV

目录

- [深度学习-CV](#深度学习-cv)
  - [1.1 深度学习方向基础](#11-深度学习方向基础)
  - [1.2 推荐学习资料](#12-推荐学习资料)
  - [1.3 深度学习方向研究方向建议](#13-深度学习方向研究方向建议)
  - [附录](#附录)
    - [更新日志](#更新日志)

### 1.1 深度学习方向基础

1. #### 深度学习基础

   - 感知机算法+激活函数（Sigmoid、Tanh、ReLU、Leaky ReLU、PReLU、Softmax等）
   - 数据集读取
   - 定义网络模型（多层感知机）
   - 基本损失函数（均方误差、交叉熵误差等）
   - 优化算法（梯度法）
   - 模型结果评估（拟合、过拟合、欠拟合、不收敛）
   - Pytorch基本使用

2. #### 更好的优化算法

   - 随机梯度下降法（SGD）
   - 小批量梯度下降法
   - 动量法
   - Adam算法


3. #### 深度卷积神经网络

   - 基本架构层：卷积层、池化层、全连接层
   - 经典网络模型：AlexNet、VGG、GoogLeNet、ResNet、DenseNet
   - 数据增强方法：hsv通道值随机修改；翻转；背景变换；添加椒盐噪声等

4. #### **目标检测网络**

   - SSD

   - R-CNN系列

   - YOLO系列：

     - 输入端：Mosaic数据增强、自适应锚框计算，正负样本分配策略以及自适应图片缩放

       主干网络：Focus结构、CSP结构

       Neck网络：FPN+PAN结构

       输出端：分类损失函数+定位损失+回归损失（GIOU_Loss）

   - YOLO-pose(关键点检测)：

     - yolopose的BBox loss func：CIoU
     - OKS--主流的关键点评估指标
     - 关键点损失函数（如简单的L1 Loss等）
     - 关键点置信度损失函数

5. #### **YOLOv5 的主干网络更改、裁剪**

   - 更换主干网络的Block
     - 更换Shuffle Block
     - 更换Mobile Block
     - 添加SE注意力机制
     - ...
   - 修改网络深度、宽度

6. #### **损失函数的设计与改进**
   - Focal Loss、OHEM loss、IOU loss等
   - 如回归损失函数：Smooth L1 Loss -> IOU Loss（2016）-> GIOU Loss（2019）-> DIOU Loss（2020）-> CIOU Loss（2020）

7. #### **网络轻量化方法**
   - 量化
   - 剪枝
   - 蒸馏

8. #### **后端推理（C++部署）**
   1. [TensorRT官方教程](https://docs.nvidia.com/deeplearning/tensorrt/developer-guide/index.html)
   2. [ONNX官方教程](https://onnxruntime.ai/docs)
   3. [OpenVINO官方教程](https://docs.openvino.ai/latest/openvino_docs_install_guides_overview.html)
   4. OpenCV(dnn)

9. #### **Vision Transformer**（比赛暂未使用）

   - RNN->LSTM->Transformer->ViT->DETR
  
### 1.2 推荐学习资料

1. #### **入门推荐书籍**

   - 《深度学习入门：基于Python的理论和实现》
   - 《动手学深度学习》
   - 《Pytorch实战》

2. #### **推荐课程**
   - 吴恩达机器学习（课程较长，数理基础扎实，有兴趣和时间可学习，快速上手深度学习不必要看）
   - 跟李沐学AI（与《动手学深度学习》配套视频）
   - 斯坦福CS231n（经典深度学习入门课程）
   - 台大李宏毅系列视频
   - TensorFlow2.0（北京大学曹健老师，课程用的tensorflow，讲的不错，可选看）

### 1.3 深度学习方向研究方向建议

   1. 顶刊顶会论文阅读，关注热点，多看代码
   2. 尝试使用最新的CNN+Transformer网络模型进行目标检测，比较与目前方案的优劣性
   3. 探讨将追踪技术引入姿态估计中，比较与目前方案的优劣性
   4. 探讨将语义分割引入比赛（雷达站，哨兵）等，比较与目前方案的优劣性
   5. 探讨将单双目深度估计引入比赛（雷达站，哨兵）等，比较与目前方案的优劣性
   6. 探讨将深度强化学习引入比赛（控制，决策）等，比较与目前方案的优劣性  
   7.  GPU 编程
       1. [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html)
       2. [OpenCL Programming (C/C++)](https://ulhpc-tutorials.readthedocs.io/en/latest/gpu/opencl/)
   8.  更多请自由探索，发挥想象力
### 附录

#### 更新日志

| 版本  | 日期        | 人员                   | 修改记录                                                     |
| ---- | ---------- | ---------------------- | ------------------------------------------------------------ |
| v2.0 | 2024-09-03 | 关超，2533290454@qq.com    | 重新排版                                                |
| v1.4 | 2024-09-01 | 关超，2533290454@qq.com    | 添加工程进阶                                                |
| v1.3 | 2024-03-22 | 段智博，995291627@qq.com   | 细化ros2基础部分要求;添加推荐教程    |
| v1.2 | 2023-12-01 | 李曾阳， 975813725@qq.com  | 添加1.1.6 Git的常用操作；细化 2.1深度学习方向的内容          |
| v1.1 | 2023-11-16 | 吴勇前， 1102567801@qq.com | 添加 *2.2.1 slam基础部分* 的推荐学习课程与方法，细化了里程计与定位方法 |
| v1.0 | 2023-06-25 | 郑桂勇， 2712089295@qq.com | 首次提交                                                     |