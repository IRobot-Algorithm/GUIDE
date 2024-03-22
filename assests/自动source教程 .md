# 自动source教程
| 版本 | 日期       | 人员                     | 修改记录                                                     |
| ---- | ---------- | ---------------------- | ------------------------------------------------------------ |
| v1.0 | 2024-3-22 | 段智博 ， 995291627@qq.com |  首次创建        |

# 动机
每次在工作区想要launch节点都得输入以下命令
```source install/setup.bash```
如果想要分别run每一个节点，还得再不同终端都source一遍。
非常麻烦。
有没有办法解决呢？
# 方案
```.bashrc```是一个神奇的文件，每次启动bash终端，都会执行这里面的命令。
因此，咱们从这里入手即可。
把下面代码复制到.bashrc的最底部（其实其他位置也可以）
```bash
# ROS 2 工作区自动 source
function auto_source_ros2_ws() {
    # 检查是否在 ROS 2 工作区内
    if [ -f "install/setup.bash" ]; then
        # 获取当前工作区路径
        ws_path=$(pwd)
        source "$ws_path/install/setup.bash"
        echo "already source." 
    else
        # 检查是否在 ROS 2 工作区的 src 目录内
        if [ -f "../install/setup.bash"]; then
            # 获取当前工作区路径
            ws_path=$(pwd | sed 's/\/src$//')
            source "$ws_path/install/setup.bash"
            echo "already source."
        fi
    fi
}

# 在每次提示符显示之前运行自动 source 函数
PROMPT_COMMAND="auto_source_ros2_ws; $PROMPT_COMMAND"
```
重新在咱们工作区下开一个终端，你会发现一行输出
```already source.```
以后就不用source了。

但上面的方案有一点问题，就是有可能误触发（即在我们不想source的地方也source了）。因此可以把上述代码改为：
```bash

# ROS 2 工作区自动 source
function auto_source_ros2_ws() {
    # 检查是否在 ROS 2 工作区内
    if [ -f "install/setup.bash" ] && [ -f "auto_source.txt" ]; then
        # 获取当前工作区路径
        ws_path=$(pwd)
        source "$ws_path/install/setup.bash"
        echo "already source." 
    else
        # 检查是否在 ROS 2 工作区的 src 目录内
        if [ -f "../install/setup.bash" ] && [ -f "../auto_source.txt" ]; then
            # 获取当前工作区路径
            ws_path=$(pwd | sed 's/\/src$//')
            source "$ws_path/install/setup.bash"
            echo "already source."
        fi
    fi
}

# 在每次提示符显示之前运行自动 source 函数
PROMPT_COMMAND="auto_source_ros2_ws; $PROMPT_COMMAND"
```
并在我们的工作区目录下创建一个auto_source.txt文件，即可自动source。如果之后不想在此工作区下自动source，再把auto_source.txt删掉即可。