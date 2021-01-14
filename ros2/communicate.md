---
sort: 2
---

# ros2 node
> ros2의 노드와 관련된 이론을 정리한 페이지 입니다. 토픽, 서비스 등과 같이 노드 사이의 통신 역시 정리하였습니다. 모든 내용은 [https://index.ros.org/doc/ros2/Tutorials/Understanding-ROS2-Nodes/#ros2nodes](https://index.ros.org/doc/ros2/Tutorials/Understanding-ROS2-Nodes/#ros2nodes)을 참고하였습니다. 또한 그림의 경우[ROS History](http://www.slideshare.net/yoonseokpyo/20160406-ros-1-for)을 참고하였습니다.

## node 

ros에서 노드는 단일 목적을 담당하여 수행하여야 합니다. 또한 데이터를 다른 노드로 주거나 받을 수 있습니다. 한 프로그램이 모든 작업을 담당하는것이 아닌, 노드들이 작업을 분할하여 담당한다고 이해하면 될듯 합니다.

## 예시
설치 후 확인 과정에서 사용하였던 구독자, 발행자 노드를 실행시켜 봅시다.
터미널 창을 열어 발행자를 실행합니다.
```
. ~/ros2_dashing/install/local_setup.bash
ros2 run demo_nodes_cpp talker
```
또다른 터미널 창을 열어 구독자를 실행합니다.
```
. ~/ros2_dashing/install/local_setup.bash
ros2 run demo_nodes_py listener
```
구독자와 발행자 둘다 각각의 노드로서 기능을 합니다. 만약 노드의 이름을 확인하기 위해선 `node list`를 확인합니다.
```
ros2 node list
```
그 후 터미널 창을 확인하면 현재 실행되는 노드를 확인할 수 있습니다.
```
/listener
/talker
```
```note
현재 예시에선 실행가능한 이름이 talker, listener이고 노드이름 역시 같지만, 실행가능한 이름과 노드의 이름이 다른 경우도 있습니다.
```
## remapping
remapping을 통해 노드, 토픽, 서비스의 이름을 재정의 할 수 있습니다. 이를 통해 실행파일을 수정하지 않고도 원하는 노드 혹은 토픽의 이름을 수정하여 다른 결과를 내보낼 수도 있습니다.
위의 예시부분을 실행 한 후 또다른 터미널창을 열어 또다른 구독자를 실행합니다.
```
ros2 run demo_nodes_cpp listener __node:=listener2
```
그 후 현재 실행되는 노드를 확인하면 /listener2가 추가된 것을 확인할 수 있습니다.
```
/listener2
/talker
/listener
```

## 노드 정보
노드의 이름을 알고있을 경우 해당 노드의 정보를 알 수 있습니다.
```
ros2 node info <node_name>
```
예를 들어 `talker`노드의 정보를 알아보고자 한다면 다음과 같은 명령어를 실행시킵니다.
```
ros2 node info /talker
```
그럼 다음과 같은 정보를 확인할 수 있습니다.
```
/talker
  Subscribers:
    /parameter_events: rcl_interfaces/msg/ParameterEvent
  Publishers:
    /chatter: std_msgs/msg/String
    /parameter_events: rcl_interfaces/msg/ParameterEvent
    /rosout: rcl_interfaces/msg/Log
  Services:
    /talker/describe_parameters: rcl_interfaces/srv/DescribeParameters
    /talker/get_parameter_types: rcl_interfaces/srv/GetParameterTypes
    /talker/get_parameters: rcl_interfaces/srv/GetParameters
    /talker/list_parameters: rcl_interfaces/srv/ListParameters
    /talker/set_parameters: rcl_interfaces/srv/SetParameters
    /talker/set_parameters_atomically: rcl_interfaces/srv/SetParametersAtomically
```

## ROS1과 다른 점?
ROS1과 다른 점중 하나로 마스터와 파라미터 서버가 다른 위치에 존재한다는 것입니다.

![ros1](/ros2/config/image.png)

다음 그림과 같이 ROS1은 ROS Master가 노드간의 정보를 관리하고 통신을 관리하게 됩니다. 또한 ROS parameter server에서 노드들의 파라미터를 관리하도록 되어 있습니다. Master와 parameter server 노드는 roscore를 통해 실행되었습니다. 이 때문에 ROS Master에 문제가 생기는 경우 전체 시스템을 사용하지 못하는 문제점이 있었습니다.

![ros2](/ros2/config/Nodes-TopicandService.gif)

이와 같은 문제점으로 인해 ROS2에서는 master가 사라지고 parameter server는 각 노드안에서 개별적으로 관리하게 되었습니다. 그 덕분에 parameter가 충돌하거나, 전체 시스템을 사용하지 못하는 문제점이 사라지게 되었습니다. 또한 통신방식 역시 기존의 TCP통신이 아닌 DDS방식으로 보안성을 높였다고는 하는데.... 통신에 대해선 이해가 좀 더 필요할 듯 합니다. 

이 외에도 기존 ROS와 ROS2의 다른점은 많이 있습니다. Real-Time 지원, roslaunch의 파이썬 지원 등 여러 점이 추가되었고 각 내용들은 천천히 알아보도록 하겠습니다. 

