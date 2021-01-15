---
sort: 2
---

#  정기구학 

> 카메라 자동 캘리브레이션을 위한 고정거치대는 로봇팔과 유사하게 만들어 집니다. 이를 위한 정기구학을 정리한 페이지 입니다. 또한 현재 진행중인 프로젝트의 생각들을 정리해 놓기도 하였습니다.

## 기구학 개요 
로봇 기구학은 정기구학, 역기구학으로 나눠질 수 있습니다. 정기구학은 원점으로부터 말단의 위치와 자세를 구하는 문제이고 역기구학은 그 반대로 말단의 위치와 자세로부터 관절의 각도를 구하는 문제입니다. 정기구학의 경우 상대적으로 간단하나 역기구학의 경우 해가 여러개이거나 해가 없는 경우도 있습니다. (해가 없어 로봇이 그에 맞는 형태를 가지지 못할 경우 특이점이라 부르는 듯 합니다.)

## 공부 방향 (?)
일단 ... 현재 필요한 카메라 고정거치대는 로봇의 앞 뒤를 x축, 양 옆을 y축, 위 아래를 z축으로 가정하면 x, y, z축 병진운동 + y, z축 회전운동이 가능하여야 합니다. 

그러나 카메라 고정 거치대 자체는 로봇의 몸체 좌표계를 따라갑니다. 로봇은 고정 좌표계에서 x, y 병진운동 + z축 회전운동이 가능합니다. 그럼 .... 카메라 고정고치대는 z축 병진운동 + y축 회전운동이 가능하여야 겠군요. 그럼 기본형을 2축 로봇팔의 형태를 가진 거치대로 생각하고 설계하도록 하겠습니다.

## 2차원 변환 

$$
\begin{aligned}
  \left( \begin{array}{c}
      x_1 \\
      y_1 \\
      1
    \end{array} \right)
   =
    \left( \begin{array}{c}
      cos(\phi) & -sin(\phi) & m \\
      sin(\phi) & cos(\phi) & n \\
      0 & 0 & 1 
    \end{array} \right)
    \left( \begin{array}{c}
      x\\
      y\\
      1
    \end{array} \right)
\end{aligned}
$$

Φ는 기존 좌표에서 다음 좌표의 사이각, (m,n)은 병진 운동량

## 3차원 변환

### 병진 운동

$$
\begin{aligned}
    \left( \begin{array}{c}
      1 & 0 & 0 & x \\
      0 & 1 & 0 & y \\
      0 & 0 & 1 & z \\
      0 & 0 & 0 & 1
    \end{array} \right)
\end{aligned}
$$

### x축 회전 운동 (roll) 

$$
\begin{aligned}
    \left( \begin{array}{c}
      1 & 0 & 0 & 0 \\
      0 & cos(\phi) & -sin(\phi) & 0 \\
      0 & sin(\phi) & cos(\phi) & 0 \\
      0 & 0 & 0 & 1
    \end{array} \right)
\end{aligned}
$$

### y축 회전운동 (pitch)

$$
\begin{aligned}
    \left( \begin{array}{c}
      cos(\phi) & 0 & sin(\phi) & 0 \\
      0 & 1 & 0 & 0 \\
      -sin(\phi) & 0 & cos(\phi) & 0 \\
      0 & 0 & 0 & 1
    \end{array} \right)
\end{aligned}
$$

### z축 회전운동 (yaw)

$$
\begin{aligned}
    \left( \begin{array}{c}
      cos(\phi) & -sin(\phi) & 0 & 0 \\
      sin(\phi) & cos(\phi) & 0 & 0 \\
      0 & 0 & 1 & 0 \\
      0 & 0 & 0 & 1
    \end{array} \right)
\end{aligned}
$$

병진과 회전을 순차적으로 할 경우 차례대로 행렬의 곱을 연산한다. 단, 연산의 순서가 바뀔경우 네번째 열(병진운동) 값이 달라질 수 있다.
