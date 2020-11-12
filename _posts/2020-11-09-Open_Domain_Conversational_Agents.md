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


## 1. Continually Learning
continually learning을 하는데에는 크게 세 가지 요소가 존재하는데, 각각에 대해 살펴보도록 하자.

### continuous online training of underlying models
static dataset을 가지고 학습을 하는 것은 reproducibility나 model comparison에는 좋지만 학습시킬 당시와 배포할 시기 사이의 차이가 있을 수 있다. 그리고 한 도메인에서 학습된 모델을 목적에 맞게 finetune하면 좋은 결과를 얻을 수 있지만 여전히 새로운 topic으로 적용하기에는 한계가 있다.

특정 도메인 관련해서는 다양한 도메인의 데이터로 학습을 시키는 것이 fine-tuning할 때 제일 좋지만 interaction data의 경우 특별히 더 학습시키기에 좋은 데이터를 특정하지 못했고, 실제로 dialog corpora로 학습하는것보다 그냥 바로 사용할 수 있는 non-interactive corpora를 사용해서 학습하는게 더 효과적이라는 발표도 있었다.

대화중에 화제를 유지하거나 과거의 다른 topic으로 바꾸기 위해서는 강화학습을 사용할 수 있는데, 이 때 보상을 어떻게 줘야 하는지는 현재도 연구되고 있는 분야이다. 이를 위해 대화를 하는 과정에서 배우는 방법이 있다.

### extracting useful learning signals from interaction
이 방법은 대화 상대로부터 직접 feedback을 받는 방법인데, 예를 들어 self feeding chatbot의 경우 대화 상대방의 만족도를 예측하여 본인이 실수를 했다고 생각하면 feedback을 요청하는 방식으로 reward를 받는 방식이다. 

이 카테고리에서 해결해야 할 문제는 상대방의 unsatisfaction이 conversation 밖의 문제일 경우 공감을 해줘야 하는데, 이를 conversation의 문제라고 생각하고 고치려고 한다거나, sentiment를 사용해서 학습을 하는 경우 offensive comment는 피하고 positive sentiment에만 집중하다가 결국 negative한 것들을 모두 배척하고 긍정적인 주제에만 집중해서 대화의 품질이 떨어질 수 있다는 것이다.

### updating relevant sources of knowledge
좋은 conversational agent는 트렌드를 따라갈 수 있어야 하는데, 지식을 외부 소스에서 가져오는 경우 단순히 소스를 업데이트해주거나, 위키같은 다이나믹 소스를 사용하는 경우 updated 버전을 읽어오면 이를 해결할 수 있다.

Pure retrieval model의 경우 모델 생성시에 지정해준 값들에서만 읽어올 수 있고, generative model은 발화를 생성하기 때문에 좀 더 자유로운 답변을 생성할 수 있다. 초기에는 retrieval model의 성능이 더 좋았는데 최근 large pre-training dataset, 그리고 improved decoding choices를 적용하면서 generative model의 성능이 비약적으로 좋아졌다.

이 카테고리에서 해결해야 할 문제는 benchmark(기준)를 고안해내기 어렵다는 것인데, 기존의 static benchmark에서는 주제가 바뀌는데에 대해 얼마나 잘 적응하는지를 평가할 수 없다. dynamically evolving benchmark을 위한 protocol이 제안된 적이 있는데, 이것과 비슷하게 사용자가 agent와 논의하기를 기대하는 topic을 찾아서 agent를 이에 맞추어 update하는 방식으로 활용할 수 있다. Dynamic benchmark를 conversational chatbot을 사용해본 사람들에게 본인이 관심있는 주제에 대해 대화할 수 있었는지 rate하는 방식으로 설정할 수 있다.

### interacting with human users
다양한 사람들과 대화를 하기 위해 conversation system은 탄력이 있어야 한다.

이 논문의 연구자들은 conversational agent를 사람 발화 수집 및 계속적인 평가 수집을 목적으로 하는 voting game의 형태로 배포했는데, 이 게임에서는 사람 둘이 짝을 지어 한 명은 response를 쓰고, 다른 한명은 앞선 사람이 쓴 response와 model이 생성한 response중 하나를 선택한다.

이 카테고리에서 해결해야 할 문제는 여전히 성능이 좋은 모델은 용량도 크고 연산량도 많은 transformer 기반의 모델이라는 것인데, memory와 computation을 줄이면 on-device model과 소통을 할 수 있게 될 것이라고 주장한다. 최근에는 knowledge distillation, adaptive span, pruning등 더 compact하고 성능 좋은 모델을 만드는 연구가 진행되고 있다고 한다.

## 2. Engaging Content

Agent가 지속적으로 화제를 던져줘야 대화가 계속 이어진다.

### Expert & Knowledgeable

general conversationalist는 얕고 넓은 주제에 대해서, 그리 specialist는 자신의 분야 관련 지식에 대해 깊게 아는 것이 중요하다.
Agent의 경우에는 knowledge와 fact를 정리해서 답변에 잘 녹여내는 능력이 중요하다.

기존의 goal-oriented dialogue의 경우에는 정형화된 지식, 슬롯 채우기 방식, 라벨링, 강화학습 등을 사용하고, QA의 경우에는 대형의 정형, 비정형 데이터를 가지고 답변을 생성하는 방식을 사용한다.
QA에서 사용하는 데이터셋의 경우 conversation 앞쪽에 나왔던 내용을 참조해서 질의응답을 하는 사례도 있다.
하지만 이러한 goal-oriented 또는 QA 방식 모두 open-domain agent의 할 일을 모두 다루지는 못한다.

결론적으로 목표는 open domain knowledge conversation, qa, task completion을 통해 engaging하고 knowledgeable한 skillful bot을 만드는 것이다.

이 카테고리에서 생길 수 있는 문제는, 전문적이고 박식하기 위해서는 기억과 추론 모두를 잘 해야 하는데 그 중에서도 상식적인 추론을 잘 해야 한다는 것이다. 그 외에도 새로운 task로의 transfer가 잘 이뤄져야 하는데, 이러한 부분은 아래와 같은 두 가지를 개선하면 해결될 것이라 생각한다 한다.
1. architectures and learning mechanisms that better incorporate compositionality
2. continual learning that updates knowledge and expertise in those tasks

### Expressiveness and Flow
Agent는 대화에서의 균형을 잘 유지하는 것도 매우 중요한데, 이 때 균형이라고 함은 simplicity vs detail, staying on topic vs changing the topic, ask vs answer 등을 말한다.
생성모델을 사용할 경우 짧고 의미 없는 대답을 생성하거나 자주 나오는 단어는 지나치게 많이 사용학 거의 안나오는 단어들은 점점 덜 사용하는 한계가 존재한다.

담화에서는 전체 dialogue flow의 예측이 어렵다는 문제도 존재하는데, 이 문제는 perplexity를 단순히 최적화한다고 해결되지도 않는다.

Generic response problem을 해결하기 위해서는 controllable neural text generation을 사용하는 방법이 있다. controllable neural text generation의 예시로는 conditional training 또는 weighted decoding 방식이 있는데, 기본적으로 rare word의 사용을 늘려 less generic한 발화를 생성하는 방식이다.
<del>generic을 not specific으로 간주해도 되는건가</del>

아무튼 그 외의 요소들도 조절해서 학습이 가능하고, 조절 결과 human quality judgement 결과가 더 좋았다고 한다.

또다른 방법으로는 unlikelihood training을 하는 방법이 있는데, 이는 성능을 저하시키는 요인에 패널티를 주는 방식이다. 이 방식은 language modeling에서는 좋은 성능을 보였고, dialogue 분야에서도 괜찮은 결과를 낸 사례가 있다고 한다. minimal length restraint만 설정해줘도 human rating이 크게 올라간다고 한다.

이 카테고리의 문제는 적절한 decoding 방식을 사용하더라도 주제와 연관되고 말이 되는 답변을 하지만 여전히 다양한 표현을 사용하지는 못한다는 것이다. 또한, 각각의 발화 이상으로 대화의 흐름을 최적화 하는 것도 연구해야 하는 부분이다.

### Consistency



## 3. Well-Behaved
