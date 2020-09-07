---
category: study
layout: post
title: TRANSFORMER에 대해 알아보자
---

## Transformer란?
2017년 NIPS에 게재된 "Attention is All You Need"라는 논문에서 처음 제안된 모델로, attention을 사용한 encoder-decoder 모델의 일종이다.

### encoder-decoder 모델
encoder-decoder 모델은 sequence-to-sequence(seq2seq) 학습모델 중 대표적인 모델인데, seq2seq는 이름에서 알 수 있듯, 입력 시퀀스를 통해 다른 도메인의 시퀀스를 출력하는 모델이다[1]. encoder에서는 입력받은 소스 데이터를 압축하고 decoder에서는 encoder가 압축하여 보내준 정보를 타겟 데이터로 변환하여 출력을 해준다[2]. Encoder-decoder 모델이 사용되는 예시로는 한 언어에서 다른 언어로 문장을 번역하는 translation task나 text summarization, image captioning, conversational modeling 등이 있다[3].

이제 기존의 encoder-decoder 모델에서는 RNN, LSTM, GRU등을 사용을 해왔는데 LSTM과 GRU도 사실상 RNN의 일종이다. 
RNN 모델에는 약간의 한계점들이 존재하는데,
* 문장의 길이가 길어질수록 닾의 단어가 뒤의 단어에 주는 영향이 작아진다. 즉, 단어간 거리가 멀어질수록 문맥정보를 학습하기 어렵다.
* RNN은 일방향으로만 학습이 이루어지기 때문에 뒤에 나온 단어가 앞에 나온 단어에 영향을 미치지 못한다.

이러한 한계점을 해결하기 위해 양방향으로 학습이 이루어지는 bidirectional RNN 모델도 등장하였다. 

### Attention






출처: 
[1] 딥 러닝을 이용한 자연어처리 입문 ( https://wikidocs.net/24996 )
[2] ratsgo님 블로그 ( https://ratsgo.github.io/natural%20language%20processing/2017/03/12/s2s/ )
[3] nlp-bert-transformer ( https://medium.com/@jonathan_hui/nlp-bert-transformer-7f0ac397f524 )
