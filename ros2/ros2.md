# ros2 설치

> ROS2를 공부하면서 ROS2 설치에 관련하여 올리는 페이지 입니다. 현 페이지에 나와있는 내용은 모두 [https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Development-Setup/](https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Development-Setup/) 에서 확인 가능합니다. 또한, 우분투 16.04, 18.04의 경우 Dashing, 20.04의 경우 Foxy 버전을 사용하여야 합니다.

## locale 설정

먼저 locale 설정을 확인합니다. build 가이드에선 예시로 영어 UTF-8를 사용했으나 UTF-8을 지원하는 다른 언어 모두 사용 가능합니다.

```
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```

## ROS2 apt repository 추가 

ROS2 apt repository 추가를 하기위해 GPG키를 먼저 추가 해야 하는데 리눅스 패키지 관리 개념을 정확히 알아야 이해가 가능할 듯 싶습니다.
```
sudo apt update && sudo apt install curl gnupg2 lsb-release
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
```

## ROS 툴, 개발 툴 설치
```
sudo apt update && sudo apt install -y \
  build-essential \
  cmake \
  git \
  python3-colcon-common-extensions \
  python3-pip \
  python-rosdep \
  python3-vcstool \
  wget
# install some pip packages needed for testing
python3 -m pip install -U \
  argcomplete \
  flake8 \
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
  pytest-cov \
  pytest-runner \
  setuptools
# install Fast-RTPS dependencies
sudo apt install --no-install-recommends -y \
  libasio-dev \
  libtinyxml2-dev
# install Cyclone DDS dependencies
sudo apt install --no-install-recommends -y \
  libcunit1-dev
```
```note
설치 항목에서 install 항목과 building항목이 나뉘어져 있는것을 확인할 수 있는데, 이는 개발 도구를 사용자가 설정 하느냐 안하느냐의 차이입니다. ROS2를 시작하는 단계에서는 어떤 방법을 사용하든 무방한것으로 보입니다.
```

## ROS2 코드 가져오기
ros2_dashing을 위한 폴더를 만들고 ROS2코드를 가져옵니다. 
```
mkdir -p ~/ros2_dashing/src
cd ~/ros2_dashing
wget https://raw.githubusercontent.com/ros2/ros2/dashing/ros2.repos
vcs import src < ros2.repos
```

## rosdep를 이용하여 종속성 설치
```
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro dashing -y --skip-keys "console_bridge fastcdr fastrtps libopensplice67 libopensplice69 rti-connext-dds-5.3.1 urdfdom_headers"
```
## 추가 DDS 구현 설치(선택 사항)
node간의 통신 방식이 ROS1과 다르게 DDS / RTPS 방식을 사용한다고는 하는데... 해당 내용을 알기 위해선 좀더 깊은 이해가 필요할 듯 합니다.

## 코드 빌드 
catkin_make의 역할을 하는 colcon을 사용하여 빌드를 합니다. 
```
cd ~/ros2_dashing/
colcon build --symlink-install
```
이외에 빌드 오류가 발생시 오류 발생 부분만 제외할 수 있습니다.
```note
Could not find "name" - skipping 'package_name'와 같은 경고 메세지가 뜨면서 몇몇 패키지가 설치되지 않는 경우가 있는데, 이는 해당 패키지의 종속 툴이 없어 생기는 오류입니다. 해당 패키지를 사용하여야 하는 경우가 아니면 무시하셔도 무방한것으로 보입니다. 
```
이후 install의 local_setup.bash를 실행시킵니다.
```
. ~/ros2_dashing/install/setup.bash
```
## 예제 실행
발행자와 구독자를 실행시킵니다. 설치가 제대로 완료되었을경우 c++과 python API가 정상적으로 작동하는것을 확인할 수 있습니다.
터미널 창을 열어 c++ 발행자를 실행합니다.
```
. ~/ros2_dashing/install/local_setup.bash
ros2 run demo_nodes_cpp talker
```
또다른 터미널 창을 열어 py 구독자를 실행합니다.
```
. ~/ros2_dashing/install/local_setup.bash
ros2 run demo_nodes_py listener
```
