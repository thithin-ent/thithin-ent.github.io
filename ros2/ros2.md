# ros2 설치

> ROS2를 공부하면서 ROS2 설치에 관련하여 올리는 페이지 입니다. 현 페이지에 나와있는 내용은 모두 https://index.ros.org/doc/ros2/Installation/Dashing/Linux-Development-Setup/ 에서 확인 가능합니다. 또한 우분투 16.04, 18.04의 경우 Dashing, 20.04의 경우 Foxy 버전을 사용하여야 합니다.

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
