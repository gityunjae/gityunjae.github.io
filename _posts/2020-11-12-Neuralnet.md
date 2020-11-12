---
category: DLfS
layout: post
title: 3. 신경망
---
신경망은 기본적으로 다음과 같이 생겼다.

![neuralnet](https://gityunjae.github.io/images/neuralnet.png)

여기서 은닉층은 실제로는 보이지 않는다.

신경망에 대해 알아보기 전에 이전 단원에서 본 퍼셉트론을 떠올려 보자.

퍼셉트론에서는 b + w1*x1 + w2*x2가 0보다 작거나 같으면 0, 0보다 크면 1을 출력되게 했었는데, 이를 함수로 만들면 다음과 같이 표현할 수 있다.

> y = h(b+w1*x1+w2*x2), <br>
> h(x) = 0 (x<=0), 1 (x>0)

이렇듯 입력 신호의 총 합을 출력신호로 변환해주는 함수를 일반적으로 활성화 함수(activation function)라고 부른다.
