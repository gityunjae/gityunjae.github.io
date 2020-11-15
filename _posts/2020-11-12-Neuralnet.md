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

활성화 함수에는 여러가지 종류가 있는데, 그 중 퍼셉트론은 임계값을 경계로 출력이 바뀌는 계단함수를 사용한다. (사실 퍼셉트론과 신경망의 주된 차이점은 활성화 함수의 차이이다.)

다른 활성화 함수로는 먼저 시그모이드(sigmoid) 함수가 있다. 시그모이드와 계단함수의 차이는 시그모이드는 부드러운 곡선이고 출력값으로 실수값도 나올 수 있지만 계단함수는 임계값을 기준으로 값이 확 바뀌고 출력이 0 또는 1로만 나온다는 점이다. 하지만 큰 틀에서는 입력이 작을때는 출력이 0에 가깝고 입력이 클 때에는 출력이 1에 가까워지는 구조이고, 둘 다 비선형 함수(선형이 아니다)라는 점에서 유사한 점도 있다.

선형 함수의 경우 신경망을 여러 층으로 쌓는 의미가 없기 때문에 활성화 함수로는 비선형 함수를 사용해야 한다.

또 다른 활성화 함수로는 ReLU 함수가 있는데, ReLU함수는 입력이 0 이하이면 0을 출력하고 0보다 크면 입력값을 그대로 출력하는 함수이다.

아래는 순서대로 계단함수, 시그모이드 함수, 그리고 ReLU함수이다.
<div style="float: left; width: 33%">
  <img src="https://gityunjae.github.io/images/step.png">
</div>
<div style="float: left; width: 33%">
  <img src="https://gityunjae.github.io/images/sigmoid.png"> 
</div>
<div style="float: left; width: 33%">
  <img src="https://gityunjae.github.io/images/ReLU.png">
</div>

------------

이제 3계층 신경망에서 어떤 방식으로 순방향 학습이 이루어지는지를 코드와 함께 보도록 하자.

먼저 우리가 코드로 구현할 3계층 신경망은 다음과 같다.

<사진>



코드는 다음과 같다.
``` python
def init_network():
  network = {}
  network['W1'] = np.array([[0.1, 0.3, 0.5], [0.2, 0.4, 0.6]])
  network['b1'] = np.array([0.1, 0.2, 0.3])
  network['W2'] = np.array([[0.1, 0.4],[0.2, 0.5], [0.3, 0.6]])
  network['b2'] = np.array([0.1, 0.2])
  network['W3'] = np.array([[0.1, 0.3], [0.2, 0.4]])
  network['b3'] = np.array([0.1, 0.2])

  return network
  
def forward(network, x):
  W1, W2, W3 = network['W1'], network['W2'], network['W3']
  b1, b2, b3 = network['b1'], network['b2'], network['b3']

  a1 = np.dot(x, W1) + b1
  z1 = sigmoid(a1)
  a2 = np.dot(z1, W2) + b2
  z2 = sigmoid(a2)
  a3 = np.dot(z2, W3) + b3 
  y = identity_function(a3)

  return y
  
def identity_function(x):
  return x
  
def sigmoid(x):
  return 1/(1+np.exp(-x))
  
import numpy as np
network = init_network()
x = np.array([1.0, 0.5])
y = forward(network, x)
print(y) #[0.31682708 0.69627909]
```