# 一、ros安装
## 1 设置sources.list
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

## 2 设置密钥
```
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

## 3 更新包索引
```
sudo apt update
```

## 4 下载桌面完整版（推荐）
```
sudo apt install ros-melodic-desktop-full
```

## 5 初始化 rosdep
```
sudo apt update
sudo apt install python-pip
sudo pip install rosdepc （该方法源自鱼香ROS）
sudo rosdepc init 
rosdepc update
```

## 6 设置环境
```
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## 7 安装rosinstall这个工具和其他构建ROS包的依赖项
```
sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential
```

## 8 验证
### 启动主节点
```
roscore
```
### 启动小乌龟
```
rosrun turtlesim turtlesim_node 
```
**有小乌龟节点弹出来就说明ROS安装成功了！**

# 二、autoware安装
## 1. 依赖安装
```
sudo apt update
sudo apt install -y python-catkin-pkg python-rosdep ros-$ROS_DISTRO-catkin
sudo apt install -y python3-pip python3-colcon-common-extensions python3-setuptools python3-vcstool
pip3 install -U setuptools
```
## 2. cuda安装（可选择性）
**细节不阐述，其中需要安装额外包**
```
cd && wget https://gitlab.com/libeigen/eigen/-/archive/3.3.7/eigen-3.3.7.zip #Download Eigen
mkdir eigen && tar --strip-components=1 -xzvf 3.3.7.tar.gz -C eigen #Decompress
cd eigen && mkdir build && cd build && cmake .. && make && make install #Build and install
cd && rm -rf 3.3.7.tar.gz && rm -rf eigen #Remove downloaded and temporary files
```
