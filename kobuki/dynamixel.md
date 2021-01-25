---
sort: 3
---

# ros2 dynamixel

> 고정 거치대의 관절을 움직이기 위해서 dynamixel 모터를 사용하여 제어를 합니다. ros2를 통해 다이나믹셀을 제어하는 것을 정리해놓은 페이지입니다. [로보틱스e-maual](https://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_sdk/library_setup/cpp_linux/#cpp-linux)을 참고하였습니다. 또한 ros2버전은 ros-dashing버전을 기준으로 하였습니다.

## dynamixel test 

리눅스에서 다이나믹셀이 제대로 작동하는지 테스트를 해보기 위해 먼저 DynamixelSDK를 설치하도록 하겠습니다.

설치를 위해서 먼저 컴파일러를 확인합니다. gcc 버전이 5.4.0 이상인지 확인합니다.
```
gcc -v 				# 버전 확인
sudo apt-get install gcc-[버전]	# gcc 설치 
```

의존 패키지들을 설치합니다.
```
sudo apt-get install build-essential			
sudo apt-get install gcc-multilib g++-multilib		
```

DynamixelSDK를 다운로드 합니다.
```
git clone https://github.com/ROBOTIS-GIT/DynamixelSDK.git
```

ros에서 다이나믹셀을 구동시킬 것이기 때문에 DynamixelSDK에서는 구동 테스트만 하도록 하겠습니다. 예제파일을 컴파일하고 실행하도록 합니다. 현재 사용하는 모델이 XH430-w350-R이기 때문에 protocol2.0를 사용하도록 하겠습니다. 

```
cd [DynamixelSDK 위치]/c++/example/protocol2.0/read_write/linux64
make
```

그럼 read_write라는 파일이 생긴것을 확인할 수 있습니다. 이제 다이나믹셀을 연결하도록 합니다. 다이나믹셀을 컴퓨터와 연결하기 위해선 u2d2장치가 필요합니다.

![u2d2](/kobuki/config/u2d2.png)

u2d2장치는 다이나믹셀에 전원을 넣어주진 못하므로 외부전원을 따로 넣어줘야 합니다. u2d2와 다이나믹셀 그리고 외부전원 12V를 다음 그림과 같이 연결합니다. 정상적으로 전원을 인가한 경우 다이나믹셀 뒷편의 LED에 불이 들어왔다 꺼지는 것을 확인할 수 있습니다.

이제 다이나믹셀을 구동해보도록 합니다.  

```
sudo chmod a+rw /dev/ttyUSB0 
./read_write
```

![성공화면](/kobuki/config/다이나믹셀구동.png)

만약 그림과 같이 구동되지 않고 문제가 생긴다면 [DynamixelSDK 위치]/c++/example/protocol2.0/read_write 위치의 read_write.cpp파일을 확인합니다.

![수치](/kobuki/config/수치.png)

다음 그림과 같이 컨트롤 테이블 및 파라미터를 설정한 후 다시 다이나믹셀을 구동하도록 합니다.

```note
다이나믹셀 모델마다 파라미터가 다를 수 있습니다. [e-manual](https://emanual.robotis.com/docs/kr/)을 통해 사용하는 모델의 컨트롤 테이블을 확인할 수 있습니다.
```

## dynamixel ros2

다이나믹셀이 정상작동함을 확인하였으므로 다이나믹셀 ros2 패키지를 설치하도록 합니다. 

```
mkdir dynamixel_ws && cd dynamixel_ws
mkdir src && cd src
git clone -b ros2 https://github.com/ROBOTIS-GIT/DynamixelSDK.git 
git clone -b ros2 https://github.com/ROBOTIS-GIT/dynamixel-workbench.git
```

ros1의 Dynamixel Workbench은 예제파일이 있어 ros에서의 다이나믹셀 구동을 시험해볼 수 있으나 ros2 Dynamixel Workbench에는 없는것을 확인할 수 있습니다. 그러므로 간단한 구동 확인을 위한 실행 파일을 작성해보도록 합니다.
