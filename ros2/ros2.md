# ros2 설치

## locale 설정

먼저 locale 설정을 확인한다. build 가이드에선 예시로 영어 UTF-8를 사용했으나 UTF-8을 지원하는 다른 언어 모두 사용 가능하다.

```
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings

```

## ROS2 apt repository 추가 

ROS2 apt repository 추가를 하기위해 GPG키를 먼저 추가 해야 하는데 리눅스 패키지 관리 개념을 정확히 알아야 이해가 가능할 듯 싶다.
```
sudo apt update && sudo apt install curl gnupg2 lsb-release
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'

```

## 
