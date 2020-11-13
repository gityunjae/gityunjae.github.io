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
생성 모델의 경우 얼핏봐서 엄청 사람같고 토큰 단위로 보면 정확해 보이지만 길게 문맥상으로 보면 흠이 많다.
또한, 대화하던 주제는 잘 유지하지만 실제로 이해하고 대화를 하는 것은 아니기 때문에 스스로 모순적인 말을 할 수 있다.
이 문제는 특히 Natural Language Inference 분야에서 더 활발하게 다뤄지는 내용이라고 한다.

이 카테고리에서 해결해야 할 문제는 현재는 post-processing 과정에서 classifier를 추가하여 특정 도메인에 대한 consistency를 높여줄 수 있지만, 이와 유사하게 이해를 할 수 있는 모델을 개발해서 더 범용적으로 적용을 할 수 있게 해야 한다는 것이다.

### Memory
최근 연구는 메모리 관련 이슈를 다루지 않는데 아무래도 트랜스포머 등 입력을 그렇게 많이 필요로 하지 않는 모델 구조와 크라우드소싱을 통해 얻는 등의 데이터 수집 절차 때문이다. 

long-term knowledge를 사용하는 접근법은 주로 graph representation이나 unstructured text retrieval을 사용하여 memory network architecture를 적용하는 방식이다.
이러한 접근은 long-term fact에 대한 QA나 어떤 주제에 대해 깊게 논의한다거나 long-term personal memory를 회상하는 task에서 효과적이다.

이 카테고리에서 해결해야 할 점은 새로운 memory가 계속해서 생성된다는 점과 특정한 short-term goal에 관련된 정보만 수집하고 달리 정제하지 않을 경우 일반화와 학습에 제한이 생길 수 있다는 점이다. 

머신러닝에서 read, write memory 구조가 연구된 적이 있었으나 실제 대규모 dialogue task에서는 성공적인 결과가 아직 발표된 바가 없다. BERT로 위키피디아 정보를 학습한 결과 knowledge base들을 가중치에 성공적으로 압축하는 연구가 있었지만, 이것이 실제로 문장을 읽고 거기에 담긴 지식을 배우는것과 동일하다고 생각하지 않는다.

깊은 추론을 하기 위해서는 모델들이 각각의 단계에서 하는 추론 과정을 저장해서 이를 활용할 수 있게 해야된다. 이것은 memory를 continual learning을 연결해서 생각해볼 수 있다.

### Commonsense & Reasoning
최근 연구에서는 reasoning에 대한 내용을 직접적으로 서술하지 않는대신 task를 얼마나 잘 수행하는지를 보여준다.
task-oriented dialogue의 경우 task는 사용자의 발화를 이해하는것부터 db에 검색해서 정답을 찾는 등의 task가 있고, QA에서는 유사하지만 single turn task로 reading comprehension이나 retrieval을 하는 task등이 있다.
답변을 생성하기 위해서는 어떤 task든 어느 정도의 reasoning은 해야 하지만 대부분의 경우 강력한 기준선을 제공하는 것으로 문제를 해결하는 경우가 많은데, 이 때 data가 특정 도메인 관련이면 실제로 성공적으로 task를 수행할 수 있다고 한다.

NLP 연구자들은 reasoning을 더 명시적으로 거론하고자 했는데, 이를 위해 제한된 reasoning을 포함한 인공적인 task를 주거나 더 큰 crowdsourced benchmark를 만드는 시도들이 있었다.

이러한 경우 true generalization이 얼마나 이뤄지고 있는지를 어떻게 측정할지 또한 연구되고 있는 분야인데 최근에 사람들이 모델의 결함을 직접 찾아보는 연구들도 있었다고 한다.

이 카테고리에서 해결해야 할 점은 이러한 NLP 분야에서의 reasoning에 대한 연구가 아직 dialogue나 language generation 분야에는 거의 적용되지 않았다는 것이다. 한 가지 가능성이 있어보이는 연구로 dialogue generation에 대해 likelihood unlikelihood training을 통해 correct reasoning에는 보상을 주고 incorrect reasoning에는 패널티를 부과하는 연구가 있었다.

### Multimodality and Grounding
언어는 우리가 살고 있는 세상과 관련된 개념들을 표현하는데에 사용된다. 사람은 이러한 개념들을 언어 뿐만이 아니라 시각이나 청각, 그리고 그 외 감각들도 사용해서 인지한다. 언어를 다른 modality(a person’s attitude toward the world)로 grounding하는 것은 언어를 배워 실제 사용으로 연결하는 데에 도움이 될 것이다. 

예를 들어 engaging conversational agent라면 다른 감각들에 대해 논의할 수 있어야 할 것이다. 예를 들면 image captioning이라던지 video captioning, visual QA나 visual dialogue task를 수행할 수 있을 것이다. 언어를 사용하는 Embodied agents(구현되어있는, 가상의 아바타도 있는 그런..) 또한 연구되고 있다.

Open domain conversation에 관련해서는 가장 연관성 있는 visual task는 Image-chat, Image Grounded Conversations 등 이미지를 기반한 자연스러운 대화가 있다. 사람들이 어떤것을 보면서 관점을 표현하는 task인데, 성공적인 모델로 TransResNet이 있다. 이는 image, personality 그리고 caption을 ResNet과 Transformer 모델을 사용해서 같은 공간에 투영한다.
nonconversational multimodal data와 conversational multimodal data를 결합해서 높은 성능을 보이는 모델에 대한 연구도 있었다.

이 카테고리에서 해결해야 할 점은 아무래도 하나의 modality 안에서만 연구하는것보다 여러 modality를 같이 고려하는 것이 더 유용하고, 이러한 modality를 추가함으로써 정말 engaging한 agent를 만들 수 있을 것이라고 생각한다.

### Personality
agent의 다른 merit들과 독립적으로 personality가 녹아있는 언어를 사용하는 경우 사용자들의 호감을 크게 살 수 있다.
초기에는 dialogue data를 주고 personality를 학습하게 했는데, 그 결과 다양한 personality가 섞여버리는 결과가 나왔다. 이를 해결하기 위해서 speaker id별 personality를 가중치에 포함시켜서 학습을 시키거나 training data에 personality 정보를 명시해서 학습을 시키는 시도도 있었고, 후자의 경우가 Persona-Chat dataset에 해당한다.

personality를 personality trait을 정해주고 이를 기준으로 생각하는 연구도 있었는데, 215개의 trait을 주고 학습을 시킨 결과 trait들을 잘 따라하는 결과가 나왔고, 이는 user engagement를 크게 상승시켰다.

personality를 하나로 고정해놓고 bot을 만드는 경우도 있는데, 다양한 personality를 적용할 수 있는 bot이 낫다고 생각한다. 일단 더 다양한 환경을 제공할 수 있고, 사용자들 각각이 생각하고 원하는 이상적인 대화 상대가 다를 수 있기 때문이다.

이 카테고리에서 해결할 점 중 consistency의 경우 기존의 consistency 이슈와 동일하다. 그리고 각각의 user에게 어떤 personality를 매칭 해줘야 할지도 더 연구해봐야 할 주제이다.

### Being Personal
personalize라고도 할 수 있고 customize 이라고도 할 수 있을 것 같은데 개인화를 잘 시키면 좋다.  보통 대화를 시작할때 본인의 정보에 대한 질문을 주고받는데, 이 질문들에 대한 정답이 보통 남은 대화를 이끌어간다.

이 카테고리에서 해결해야 할 점은 대화를 하면서 점점 사용자들은 agent가 내 정보를 어느정도 기억하고 있기를 기대하지만 사실상 end-to-end chatbot들은 기억을 못한다는 것이다. 이는 memory 문제로도 이어진다. dialogue 연구에 개인화된 추천 시스템을 적용해보는 것도 매우 가능성 있어 보인다.

### Putting It All Together
결과적으로 위에서 언급한 모든 요소들은 해결해야 할 점들이 있고, 이들을 개선하는 것은 중요하다. 그런데 더 중요한 것은 이 요소들을 모두 고려하여 하나로 모으는 작업이다.

실제로 다양한 dialogue task를 모두 학습시키려는 시도가 있었는데, knowledge, personality, multimodality, 그리고 여기서도 언급한 몇몇 요소들을 모두 다루었고, 한 agent가 이런 모든 스킬을 다룰 수 있게 하는것이 목적이었다. 이를 확장해서 모델을 향상시켜서 이러한 다양한 측면을 하나로 합쳐주는 연구가 시도되고 있는데, 예를 들어 Blended Skill Talk라는 데이터셋과 retrieval model을 사용해서 knowledgeability, personality, empathy 등 다양한 행동과 스킬을 한 대화에서 부드럽게 사용할 수 있는 연구가 있었다. 또다른 BlenderBot을 만드는 연구에서는 앞의 연구와 유사하게 social media 데이터로 기학습 시킨 후 대규모의 생성모델을 파인튜닝하는 방식을 사용하였다.

전반적으로 이러한 engagingness에 대한 연구는 사람들이 conversational agent와 대화하고 싶게 만드는 매우 중요한 요소이지만, engagingness만으로는 충분하지 않고, 그 대표적인 예시로 챗봇 Tay가 있다. 

## 3. Well-Behaved
conversational agent가 지녀야 할 quality 중 하나는 사람들이 원하는대로 대해줘야 한다는 것이다.

### Offensive and Toxic Content
engaging함을 유지하면서 사람들에게 어떠한 공격도 되지 않으면서 논쟁의 여부가 있을 표현, 주제, 의견 등을 제외한 표현들로만 대화를 이어가는것은 정말 어렵다. 

crowdsourcing을 통해 여러 사람들이 여러번 정제를 하는 과정을 통하면 toxic content detection task의 metric이 향상되는 결과를 얻을 수 있다. 또한 문맥이 발화를 공격적인지 아닌지에 영향을 미칠 수 있다는 것이다. 다른 연구들에서는 모델의 toxicity를 조절하기 위해서 학습 데이터 또는 학습 목표에서 offensive content를 제거하기도 하였다. social media data로 기학습시키는 것보다 crowdworker들이 생성한 toxic하지 않은 데이터로 파인튜닝하는 것이 덜 toxic한 모델을 생성한다는 연구도 있었다.

이 카테고리에서 해결해야 할 점은 offensive의 기준에 대한 깊은 이해가 없다는 점과 사람들이 즐겁게 느끼는 것과 offensive하게 느끼는 것, engaging하게 느끼는것과 부적절한 은어의 경계를 구분짓는 것이다. 기존의 제한된 생성모델을 사용해서 모델을 좀 더 개개인의 취향에 맞게 수정할 수도 있겠지만 제한된 생성모델이 그렇게까지 적용하기에는 아직 조금 부족하다. 또 다른 방법은 강화학습을 사용하는 방식인데, 이 또한 자칫하면 날씨나 재미 없는 주제들에 대해서만 얘기하는 등의 undesirable한 결과가 나올 수 있기 때문에 보상을 줄 objective를 신중하게 정해야 한다.

### Empathy and Compassion
공감의 요소를 통해 상호작용시 더 긍정적인 결과를 얻을 수가 있다. 예를 들어 약간의 감정을 비치면 화자는 상대방이 잘 듣고있구나 라고 판단을 하게 된다. 이러한 요소는 open-domain conversation에서는 특히나 더 중요한데 대화가 주로 어떤 감정을 느끼게 했던 경험을 위주로 이루어지기 때문이다. 또한 사람들이 기계를 social하게 접근하려고 하기 때문에 공감능력을 가지고 대응하는 것이 매우 중요하다.

이 카테고리에서 해결해야 할 점은 empathy와 informing, entertaining 등 다른 요소와 균형을 맞추는 것이 중요하다. 또한 사람마다 원하는 공감의 정도에 차이가 있을 수 있는데 이를 사용자와 대화 문맥을 통해 잘 설정하는 것이 중요하다.

### Privacy
배포된 conversational agent에게 가장 중요한 측면은 privacy를 보장하는 것이다. 해당 논문의 연구자들의 경우 게임 참가자들에게 persona를 부여해서 익명성을 보장하였고, 다른 연구들에서도 role-playing을 사용하거나 trait나 movie preferences를 주거나 아예 가상의 세계관을 만들어서 privacy를 보장해주었다.

이 카테고리에서 해결해야 할 일은 role-playing 	방식에 의존하다 보면 데이터에 mismatch가 생길 수 있다는 것이다. 예를 들어 사람들이 실제로 일상에서 일대일 private 대화를 할 때와 동일한 방식으로 얘기하는지 보장할 수 없다는 것이다. 다른 방법은 privacy-preserving 라이브러리를 사용하고 decentralized 접근방식을 사용하여 각 사용자의 개인 정보는 각자의 기기에만 저장이 되는 방식으로 개인화를 시키는 것이다.

이러한 방법은 현재의 모델을 디바이스 상에서 돌아갈 수 있을 정도로 down-sizing해야 가능하다. 현재 더 효율적인 distil 관련 연구가 많이 이루어지고 있다.

----
## Measuring Success
Natural Language Generation을 evaluate하는 것도 아직 갈 길이 먼 부분이다. multi-turn으로 sequence를 생성하는 task를 evaluate한다는 것 자체가 어렵기 때문이다.

그 중 dialogue system을 evaluate하려고 했던 시도들과 각각의 상대적 장점, 그리고 해결해야 할 부분들에 대해 정리하였다.

### Human Evaluations
Goal-oriented dialogue system는 상대적으로 좀 더 명확한 평가 척도를 가지고 있지만 대화 태스크처럼 open ended 분야에서는 자동화해서 평가할 수 있는 명확한 goal이 없다. 실제로도 자동적인 평가 방법이 사람이 평가한 결과와 뚜렷한 상관관계를 보이지도 않는다. 이를 보면 현재의 dialogue 연구를 평가하는 데에는 아무래도 사람의 손이 거쳐가야 한다는 것이다.

사람이 평가하는 것중 가장 흔히 사용되는 것 두가지는 single-turn pairwise evaluation과 multi-turn Likert evaluation이다.

single-turn pairwise evaluation은 평가자(사람)에게 전체적인 맥락을 주고 가능한 두가지 답변중에서 더 괜찮아 보이는 것을 고르게 시키는 방식이다. 이는 간단해서 좋지만, multi-turn 측면을 고려하지 못한다는 단점이 있다.

multi-turn Likert evaluation은 평가자(사람)가 agent와 몇분동안 대화를 한 후 Likert(1-5) 척도로 성능을 평가하는 것이다. 이러한 방식을 모델이 더 긴 대화를 잘 이끌어갈 수 있는지를 평가하기 좋고 out-of-distribution등의 상황을 다루는 능력을 평가하기에 유용하기 때문에 single-turn pairwise evaluation보다 더 선호될수도 있다. 하지만 이 방식은 single-turn pairwise evaluation 방식보다 더 많은 노동력이 필요하고 사람이 주관적으로 평가하기 때문에 완전히 믿을 수 없다. 그리고 모델간 다른 점을 찾을 만큼 강력하지 못하다. 게다가 annotator의 분포가 달라질 수 있기 때문에 기준을 re-evaluate해야 한다. 또한 첫 시스템을 평가한 결과가 이후의 평가에 영향을 줄 수 있다는 문제점도 있다.

single-turn 방식과 multi-turn 방식을 둘 다 활용하는 hybrid 방식을 제안한 연구들도 있었는데 single turn에서 연속적인 scale로 평가를 하거나 multi turn에서 binary good/bad 평가를 한 연구도 있었다. 최근에는 ACUTE-Eval이라고 complete dialogue에 대한 complete pairwise evaluation을 하기도 하였다. 이러한 방식은 single-turn과 multi-turn 두 경우 모두보다 좋은 성능을 보였다. 하지만 이 방식을 사용하면 model과 비교를 하는 사람들과 이를 pairwise로 선택하는 제3자가 있어야 하기 때문에 비교해야 할 시스템이 많으면 필요한 자원이 늘어난다. 하지만 동일한 대화 자료를 pairwise comparison으로 여러번 사용할 수 있다. 

하지만 연구 결과 이러한 ACUTE-Eval이 self-chat 모드에도 적용할 수 있다는 것을 알 수 있는데, 이는 대화를 하는 인력이 없어도 되기 때문에 필요한 자원의 양이 크게 줄어든다. 또한 self-chat 데이터를 사용한 결과와 사람이 bot과 대화하는 데이터를 사용한 결과에 상관관계가 있다는 것을 확인할 수 있다.

### Automatic metrics


## Discussion

## Recommendations to the Community

## Conclusion
