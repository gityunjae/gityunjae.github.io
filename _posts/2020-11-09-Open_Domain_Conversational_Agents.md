---
category: papers
layout: post
title: Open-Domain Conversational Agents
---
제목: Open-Domain Conversational Agents: Current Progress, Open Problems and Future Directions

저자: Facebook AI Research

발행년도: 2020



기존의 task-oriented dialogue system은 slot filling, structured module을 사용한다.

open domain dialogue를 위해서는 RNN을 사용한 end-to-end 접근방식이 새로운 도메인에도 적용이 되고 가장 활발히 연구되고 있지만 아직 부족한 부분들이 있다. 하지만 end-to-end 방식으로 학습된 deep architecture들의 경우 음성인식, 비전, 기계번역 분야에서 성공적인 사례들이 많기 때문에 dialogue 분야에서 또한 deep architecture을 적용하려는 시도들이 많이 이루어지고 있다.

Open domain chatbot을 평가할 때는 사람과 같은지 아닌지를 구분하기 보다 대화상대로 적합한지를 판단해야 한다. 이러한 특성을 위해 agent(open domain chatbot)이 갖춰야 할 요소들은 다음과 같다.
1. continually-learning
2. provide engaging content during conversations
3. well-behaved
