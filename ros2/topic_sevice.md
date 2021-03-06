---
sort: 3
---

# 토픽과 서비스

> ros2에서 토픽과 서비스에 대하여 학습한 것을 정리한 페이지입니다. 대부분의 개념은 ros1과 비슷하나 일부분 다른점이 있습니다. 


## 개요

ros에서 토픽과 서비스는 데이터를 주고 받는 역할을 합니다. 토픽의 경우 단방향 통신으로 publisher(발행자)가 데이터를 주면 subscriber(구독자)가 받게 됩니다. 반면 서비스는 클라이언트가 서버에게 데이터를 요구하면 서버는 그에 맞는 데이터를 주는 형식입니다. 

## 토픽

토픽은 ros에서 가장 많이 사용되는 데이터 통신 방식입니다. 앞서 말한 것과 같이 단방향 통신으로 발행자가 주는 동시에 데이터를 받을 수는 없습니다. 그러나 그림과 같이 일 대 다수, 다수 대 일, 다수 대 다수로 데이터를 통신할 수 있습니다.

![ros1](/ros2/config/토픽.gif)

## 토픽 발행 및 정보 확인

토픽은 노드에서 발행되나 원하는 데이터를 노드 실행 없이 발행 시킬 수도 있습니다.

```
ros2 topic pub <topic_name> <msg_type> '<args>'
``` 

또한, 현재 발행되고있는 토픽의 이름이나 토픽의 데이터, 토픽의 발행자, 구독자 수 및 데이터 형식을 확인해볼 수도 있습니다.
```
ros2 topic list               # 발행되고있는 토픽의 이름 확인
```
```
ros2 topic echo <topic_name>  # 해당 토픽의 데이터 확인
```
```
ros2 topic info <topic_name>  # 해당 토픽의 발행자, 구독자 수 및 데이터 형식 확인
```

마지막으로 해당 토픽의 발행 주기(hz)를 확인할 수 있습니다.(노드에 따라 해당 토픽이 일정 시간 이상 갱신이 안되는 경우 에러 메시지를 내보내는 경우가 있으니 주의하시길 바랍니다.)
```
ros2 topic hz <topic_name>
```

## 서비스

서비스는 토픽과는 다르게 양방향 통신입니다. 클라이언트가 서버에게 데이터를 요청하면 서버는 클라이언트에게 해당 데이터를 주는 방식입니다. 토픽의 경우 수시로 데이터를 주고받아야 할때 사용한다면 서비스의 경우 데이터의 갱신이 필요 없거나 적은 경우에 사용됩니다.(자율 주행의 예로 들면 지도 데이터가 해당될 수 있습니다.) 또한 그림과 같이 클라이언트는 다수일 수 있으나 서버는 한개여야 합니다.

![ros1](/ros2/config/서비스.gif)

## 서비스 발행 및 정보 확인

서비스는 노드에서 발행되나 원하는 데이터를 노드 실행 없이 요청 시킬 수도 있습니다.
```
ros2 service call <service_name> <service_type> <arguments>
```

또한, 현재 발행되고있는 서비스의 이름 및 데이터 형식을 확인할 수 있습니다.
```
ros2 service list                # 서비스 이름 확인
```
```
ros2 service type <service_name> # 해당 서비스의 데이터 형식 확인
```

```note
ros2 service list 로 현재 서비스를 확인하면 parameters가 있는 것을 볼 수 있습니다. 이는 ros1에선 parameters서버가 따로 존재하여 각 노드의 파라미터를 일괄 처리하였으나 ros2에서는 각 노드가 파라미터를 처리하기 때문에 이에 대한 정보를 서비스로 주고 받습니다.
```


