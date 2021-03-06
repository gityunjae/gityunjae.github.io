---
category: DLfS
layout: post
title: 2. 퍼셉트론
---

<b>퍼셉트론은 가중치와 편향을 사용한다. </b>

아래와 같이 두 개의 입력을 받아서 하나의 신호를 출력한다고 할 때,각각의 입력값에 가중치를 곱해주면 w1*x1 + w2*x2라고 표현할 수 있다. 이 값이 Θ(임계값)보다 크면 1, 작거나 같으면 0을 출력하게 된다.

![perceptron](https://gityunjae.github.io/images/Perc.png)

여기서 임계값 Θ을 편향값 b로 표현하면 w1*x1 + w2*x2 + b 처럼 표현할 수 있는데, 이 값이 0보다 크면 1, 작거나 같으면 0을 출력한다.
예를 들어 b, w1, w2가 각각 -0.7, 0.5, 0.5인 경우 AND를 표현할 수 있다.

<b>단층 퍼셉트론으로는 선형 영역만, 다층 퍼셉트론으로는 비선형 영역도 표현할 수 있다. </b>

AND나 OR처럼 직선으로 영역을 나누는 문제는 단층 퍼셉트론으로 해결할 수 있지만 XOR의 경우 아래와 같이 선형으로 나눌 수 없다.

![xor](https://gityunjae.github.io/images/XOR.jpeg)

이런 경우 비선형으로 영역을 나누어 해결할 수 있는데, 이를 위해서 다층 퍼셉트론을 사용한다.

![xorPerceptron](https://gityunjae.github.io/images/xorPerc.png)

위와 같이 NAND, OR, AND를 2층으로 쌓아서 XOR를 해결할 수 있다.
