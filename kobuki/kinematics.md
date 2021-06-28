---

sort: 3
-------

kinematics - 강체운동
=====================

> 기구학은 로봇팔 뿐만 아니라 강체 운동에 대해서 다양하게 사용됩니다. 이러한 강체 운동에는 로봇의 움직임, 카메라와 같은 센서의 이동 역시 포함됩니다. 본 페이지는 이러한 내용들은 정리해놓은 페이지입니다.

회전 행렬과 와 lie군, lie 대수
------------------------------

회전 행렬은 직관적입니다. 그렇기에 가장 쉽게 사용가능하고, 깉은 수학적 지식이 없어도 빠르게 습득 가능합니다. 대표적으로 오일러 회전, Roll - Pitch - Yaw가 있습니다. 그러나 이러한 방식은 문제가 있는데, 짐벌락과 같은 회전을 표현할 수 없는 상태가 있다거나, 계산의 구현이 복잡해진다는 것입니다.

### 회전행렬의 특징

회전행렬에는 특징이 있습니다. 2차원 회전을 예로 들어보겠습니다.

$$\begin{aligned} \left( \begin{array}{c} cos(\theta) & -sin(\theta) \\ sin(\theta) & cos(\theta) \\ \end{array} \right)\end{aligned} $$

다음과 같은 회전행렬은 덧셈 연산이 불가능합니다. 오직 곱셈 연산만 가능한데, 이는 회전 행렬의 고유치는 1이여야 하기 때문입니다. 오직 곱셈 연산에서만 회전 행렬의 연산이 성립하는데, 이를 곱셈 연산에 대해서 군(group)이라고 합니다.

그럼 이제 군(group)이 무엇인지 알아야 하는데, 간단하게 어떤 집합이 어떤 연산에서 다음을 만족하여야 합니다.

1.	닫힘성(Closure)
2.	결합성(Associativity)
3.	항등성(identity element)
4.	역(inverse)

덧셈 연산에서는 닫힘성을 만족하지 않기에 회전 행렬은 덧셈 연산에 대하여 군이 아니게 됩니다.

이러한 특징은 계산을 복잡하게 만드는 주범이 됩니다.

### lie군 과 lie 대수의 관계

군 중에서 연속적인 상태의 군은 lie군이라 부릅니다. 정수로 이루어진 군은 덧셈에 닫혀있으나, 연속적이지 못합니다. 반면 실수로 이루어진 군역시 덧셈에 닫혀맀으면서, 연속적이므로 lie군으로 볼 수 있습니다. 회전 행렬 군 역시 lie 군에 포함됩니다.

수학적으로 계산하고 증명하기에 앞서 먼저 왜 lie군을 사용하는지에 대해 생각해봅시다. 회전 행렬은 덧셈연산에 닫혀있지 않고, 고유치가 무조건 1이여야하며, 역행렬과 전치행렬은 같아야 하는 등의 여러 제약조건이 존재합니다. 이를 선형성이 보장가능하고, 제약조건이 없는 상태로 바꿀 수 있다면 편하게 사용가능 하기에 lie군과 lie대수를 사용하게 됩니다.