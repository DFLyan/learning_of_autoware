## 一、前期准备
sudo apt-get -y install git

git clone https://github.com/autowarefoundation/autoware.git
cd autoware

## 二、安装ros
### You will need to add the ROS 2 apt repository to your system. First, make sure that the Ubuntu Universe repository is enabled by checking the output of this command.
apt-cache policy | grep universe

### The output of the above command should contain following:

### 500 http://us.archive.ubuntu.com/ubuntu focal/universe amd64 Packages
###     release v=20.04,o=Ubuntu,a=focal,n=focal,l=Ubuntu,c=universe,b=amd64

### If you don’t see an output line like the one above, then enable the Universe repository with these instructions.
sudo apt install software-properties-common
sudo add-apt-repository universe

### Now add the ROS 2 apt repository to your system. First authorize our GPG key with apt.
sudo apt update && sudo apt install curl gnupg lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

### Then add the repository to your sources list.
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

### Update your apt repository caches after setting up the repositories.
sudo apt update

### Desktop Install
rosdistro=galactic(这是ubuntu20，18的话eloquent）
installation_type=desktop
sudo apt install ros-${rosdistro}-${installation_type}

### Environment setup
### (Optional) You can source ros2 in the ~/.bashrc file.
echo '' >> ~/.bashrc && echo "source /opt/ros/${rosdistro}/setup.bash" >> ~/.bashrc

### ros2命令的自动补全
sudo apt install python3-argcomplete

### 通信测试
开一个终端：ros2 run demo_nodes_cpp talker

再开一个终端：ros2 run demo_nodes_py listene


## 三、ROS 2 Dev Tools安装
### Taken from https://docs.ros.org/en/galactic/Installation/Ubuntu-Development-Setup.html
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

### Install some pip packages needed for testing
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

### Initialize rosdep
sudo rosdep init
rosdep update
