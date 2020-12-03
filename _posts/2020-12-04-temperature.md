---
category: study
layout: post
title: Temperature와 softmax
---
Temperature는 신경망 모델의 하이퍼 파라미터 중 하나인데, softmax를 적용하기 전 logit 값들을 scaling해서 예측의 무작위성을 조절하는 역할을 한다.

![01](https://gityunjae.github.io/images/tempSoft2.png)

temperature 값이 1이면 이전 레이어에서 받은 logit값들을 그대로 사용해서 softmax를 계산하고, temperature 값이 0.6이면 logit값들을 0.6으로 나눠준 값들로 softmax를 계산하는데, 이 경우 logit 값들이 더 커진다.

softmax를 더 큰 값들에 대해 수행하면 더 confident하지만 더 보수적으로 학습이 된다. 즉 확률이 낮은 후보들은 확실히 제껴버린다. 앞에서 보았듯 temperature 값이 더 작아지면 softmax 입력값들이 더 커진다.

그러면 temperature 값이 더 커지면 더 부드러운 확률분포가 나오는데 더 다양한 결과가 나오지만 그만큼 정확도도 조금 낮아질 수 있다.

softmax 함수는 출력값을 0과 1 사이로 유지하기 위해 각 iteration에서 후보들을 정규화 하는 과정을 거치는데, temperature는 여기서 확률이 낮은 후보들의 민감도를 올리거나 낮추는 역할을 한다.

더 자세한 내용은 위키나 Hinton의 Distilling the knowledge in a neural network 논문, 그리고 다른 블로그들을 참고하도록 하자.
<br><br>
출처: <a href="https://medium.com/@majid.ghafouri/why-should-we-use-temperature-in-softmax-3709f4e0161">Why should we use Temperature in softmax?</a>
