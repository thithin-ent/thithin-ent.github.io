---
sort: 3
---


# Gradient

> udacity self driving car의 computer vision 부분을 통해 학습한 내용을 정리한 페이지 입니다. 

## 개요

앞서 차선을 인식하기 위해 차선의 흰색 부분만 추출하고 가장자리를 찾는 기본적인 알고리즘을 찾아 보았습니다. 가장자리를 찾기 위해선 픽셀 수치의 변화량인 gradient를 계산하였는데, 이를 계산하기 위해선 sobel 연산자를 사용합니다. 이러한 원리를 이해하기 위해 sobel을 통한 가장자리 검출에 대한 내용을 알아보고, 차선의 경우 흰색 뿐만 아니라 그림자가 있거나 혹은 여러가지 색상의 차선을 잘 구분하기 위한 방법을 알아보도록 합니다.

## sobel

sobel을 알기위해선 먼저 sobel 연산자에 대하여 알아야 합니다. sobel 연산자는 다음 그림과 같습니다.

![ros1](/computervision/config/sobel.png)

```note
sobel 연산자가 3* 3만 존재하는 것은 아니나 대표적으로 많이 사용됩니다. 5* 5, 7* 7과 같이 홀수 크기는 모두 사용됩니다. 
```

픽셀의 gradient를 계산할 때에는 sobel연산자와 픽셀값의 내적으로 gradient가 계산이 됩니다. 간단히 

x sobel 연산자를 사용하여 픽셀의 gradient를 계산할 경우 x축(가로)을 기준으로 하는 gradient가 나오게 됩니다. y sobel 연산자를 사용할 경우는 y축(세로)을 기준으로 하는 gradient가 나오게 됩니다. 

x sobel 연산자를 사용 할 경우는 x축 gradient가 나오고, y sobel 연산자를 사용 할 경우는 y축 gradient가 나오므로 각 수치의 제곱 합의 제곱근을 구하면 해당 픽셀의 gradient값을 구할 수 있습니다.

$$
\begin{aligned}
  gradient = \sqrt{gradient^2_{x}+gradient^2_{y}}
\end{aligned}
$$

픽셀의 gradient 값을 구한 후는 gradient의 임계치를 구하여 가장자리를 검출 할 수 있습니다.

## HSV, HSL

앞서 말한 것과 같이 차선은 흰색만 있는 것이 아닙니다. 또한 날씨에 따라 차선의 RGB값은 일정하지 않습니다. 이를 보안하기 위해 HSV(색상, 채도, 명도)나 HSL(색상, 채도, 밝기)를 사용하여 색상을 추출할 수도 있습니다. udacity에서는 주로 HSL을 사용하여 색상을 처리 하고 있습니다. 

HSL은 색상(Hue), 채도(Saturation), 밝기(Lightness)라고 하였습니다. 채도가 낮을수록 무채색이 되며, 밝기가 낮을수록 검은색, 높을수록 흰색에 가까워 집니다. HSV의 경우는 밝기(Lightness)를 명도(value)로 바꾼 것 입니다.다음 그림과 같이 HSL과 HSV를 표현할 수 있습니다.

![ros1](/computervision/config/HSL.png)

## 차선 인식

이러한 기능들을 이용하는 궁극적인 목적은 다양한 상황에서의 차선을 잘 인식하기 위함입니다. sobel을 통한 가장자리 검출과 HSL을 이용한 차선 검출을 적절히 섞어서 사용하면 차선이 인식되기 힘든 상황에서도 적절히 검출할 수 있게 됩니다. 물론 적절히 검출하기위한 임계치를 얻기 위해선 적절한 튜닝을 통해 값을 얻어야 합니다.
