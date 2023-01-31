---
description: '- 대화를 위한 최적화된 언어 모델'
---

# CHATGPT Details

* Prompt의 지시를 따르고 자세한 응답을 제공하도록 훈련된 InstructGPT의 형제 모델&#x20;

### 행동 양식

* InstructGPT와 동일한 방법을 사용하지만 데이터 수집 설정이 약간 다른 RLHF(Reinforcement Learning from Human Feedback)을 사용해 이 모델을 교육
* Supervised Fine-tuning을 사용해 초기 모델을 교육&#x20;
* 인간 AI 트레이너가 대화를 제공하여 사용자와 AI 비서 양쪽을 플레이함&#x20;
* 강사들에게 모델로 작성된 제안에 대한 액세스 권한을 부여해 응답을 작성하는 데 도움을 줌&#x20;
* 이 새로운 대화 데이터 세트를 대화 형식으로 변환한 InstructGPT 데이터 세트와 혼합



* 강화학습을 위한 보상 모델을 만들려면 품질 별로 순위가 매겨진 두 개 이상의 모델 응답으로 구성된 비교 데이터를 수집해야 했음&#x20;
  * 이 데이터를 수집하기 위해 AI 트레이너가 챗봇과 나눈 대화를 취함&#x20;
  * 모델 작성 메시지를 무작위로 선택하고 몇 가지 대체 완료를 샘플링했으며, AI 트레이너가 순위를 매김&#x20;
* 이러한 보상 모델을 사용 & Proximal Policy Optimization을 사용해 모델을 미세 조정할 수 있음&#x20;
  * 이 프로세스를 여러 번 반복&#x20;
  *





* [https://openai.com/blog/chatgpt/](https://openai.com/blog/chatgpt/)

