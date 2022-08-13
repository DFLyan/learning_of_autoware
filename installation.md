# 所有安装过程总结于[官方教程](https://autowarefoundation.github.io/autoware-documentation/pr-82/installation/autoware/source-installation/)
## 一、前期准备
```
sudo apt-get -y install git

git clone https://github.com/autowarefoundation/autoware.git
cd autoware
```

## 二、安装ros
### You will need to add the ROS 2 apt repository to your system. First, make sure that the Ubuntu Universe repository is enabled by checking the output of this command.
```
apt-cache policy | grep universe
```

### The output of the above command should contain following:

### 500 http://us.archive.ubuntu.com/ubuntu focal/universe amd64 Packages
###     release v=20.04,o=Ubuntu,a=focal,n=focal,l=Ubuntu,c=universe,b=amd64

### If you don’t see an output line like the one above, then enable the Universe repository with these instructions.
```
sudo apt install software-properties-common
sudo add-apt-repository universe
```

### Now add the ROS 2 apt repository to your system. First authorize our GPG key with apt.
```
sudo apt update && sudo apt install curl gnupg lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

### Then add the repository to your sources list.
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```
### Update your apt repository caches after setting up the repositories.
```
sudo apt update
```
### Desktop Install
```
rosdistro=galactic(这是ubuntu20，18的话eloquent)
installation_type=desktop
sudo apt install ros-${rosdistro}-${installation_type}
```

### Environment setup
### (Optional) You can source ros2 in the ~/.bashrc file.
```
echo '' >> ~/.bashrc && echo "source /opt/ros/${rosdistro}/setup.bash" >> ~/.bashrc
```

### ros2命令的自动补全
```
sudo apt install python3-argcomplete
```

### 通信测试
开一个终端：
```
ros2 run demo_nodes_cpp talker
```

再开一个终端：
```
ros2 run demo_nodes_py listener
```

## 三、ROS 2 Dev Tools安装
### Taken from https://docs.ros.org/en/galactic/Installation/Ubuntu-Development-Setup.html
```
sudo apt update && sudo apt install -y \
  build-essential \
  cmake \
  git \
  python3-colcon-common-extensions \
  python3-flake8 \
  python3-pip \
  python3-pytest-cov \
  python3-rosdep \
  python3-setuptools \
  python3-vcstool \
  wget
```

### Install some pip packages needed for testing
```
pip install pyyaml
```
```
python3 -m pip install -U \
  flake8-blind-except \
  flake8-builtins \
  flake8-class-newline \
  flake8-comprehensions \
  flake8-deprecated \
  flake8-docstrings \
  flake8-import-order \
  flake8-quotes \
  pytest-repeat \
  pytest-rerunfailures \
  pytest \
  setuptools
```


### Initialize rosdep
```
sudo rosdep init
rosdep update
```

## 四、the RMW Implementation安装
### For details: https://docs.ros.org/en/galactic/How-To-Guides/Working-with-multiple-RMW-implementations.html
```
sudo apt update
rosdistro=galactic(这是ubuntu20，18的话eloquent)
rmw_implementation=rmw_cyclonedds_cpp
rmw_implementation_dashed=$(eval sed -e "s/_/-/g" <<< "${rmw_implementation}")
sudo apt install ros-${rosdistro}-${rmw_implementation_dashed}
```

### (Optional) You set the default RMW implementation in the ~/.bashrc file.
```
echo '' >> ~/.bashrc && echo "export RMW_IMPLEMENTATION=${rmw_implementation}" >> ~/.bashrc
```

## 五、 pacmod3安装
```
# Taken from https://github.com/astuff/pacmod3#installation
sudo apt install apt-transport-https
sudo sh -c 'echo "deb [trusted=yes] https://s3.amazonaws.com/autonomoustuff-repo/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/autonomoustuff-public.list'
sudo apt update
rosdistro=galactic(这边更新亚马逊源的时候一直连不上，apt-cache search pacmod后发现只有dashing，就先安装这个了)
sudo apt install ros-${rosdistro}-pacmod3
```

## 六、 Autoware Core dependencies安装
```
pip3 install gdown
```

## 七、 Autoware Universe dependencies安装
```
sudo apt install geographiclib-tools

# Add EGM2008 geoid grid to geographiclib
sudo geographiclib-get-geoids egm2008-1
```
**这一步会比较慢，如果不行就从输入命令后，出现的网址自己下载下来，然后解压所有文件到‘/usr/share/GeographicLib/geoids’**

## 八、 pre-commit dependencies安装
```
clang_format_version=14.0.6
pip3 install pre-commit clang-format==${clang_format_version}
```

# Install Golang (Add Go PPA for shfmt)
```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt install golang
```

## 九、 cuda、cudnn、tensorrt安装
由于虚拟机识别不了独立显卡，所以这一步不详细阐述，有需要的，在annaconda的虚拟环境中安装即可

## 十、设置工作区
### 1. 创建(使用vcstool)
```
cd autoware
mkdir src
vcs import src < autoware.repos
```
### 2. ROS包安装
```
source /opt/ros/galactic(这是ubuntu20，18的话eloquent)/setup.bash
rosdep update
rosdep install -y --from-paths src --ignore-src --rosdistro $ROS_DISTRO
```
**这一步可能会出现报错，先安装Gazebo**
```
sudo apt-get install gazebo
```
**如果还报错，那么就是因为管网的安装过程是基于ubuntu20，低于这个版本的时候，安装包的时候可能是版本不匹配，后面就报错了

### 3. 编译(使用colcon)
```
colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release
```
**这一步会缺少比较多的python包
```
pip install catkin_pkg numpy empy lark
```
