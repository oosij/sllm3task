개인 스터디로, LLM FT 방향으로 스터디 중, 다음 조건하에 모델 학습을 하고, 이를 평가, 정량/정성 평가를 목표로 진행하였습니다.
정량 평가는, 대화/요약의 경우 G-Eval 방식[1]으로, 특히 대화 경우 [2]의 방식을 채택하였습니다.  
분류의 경우, 대표적인 Confusion matrix를 사용했습니다. 
평가를 위해, 일부 데이터를 분리해서 사용하였습니다. (비율상으로 하려했으나, 데이터 분포 및 GPT 비용/시간상, 임의로 잘라냈습니다.)

조건 :
1) 싱글/멀티 대화턴 학습
2) 대화시, 마지막에 질문을 해서 대화를 계속 유도
3) 이전 대화를 기억 (문맥 파악)
4) 요약, 분류 등 그동안 했던 Task에 대해 프롬포트 가능 필요

이 4가지를 필수로써, 자료 조사 및 선행 연구를 찾아보았고, 1-3)은 저와 같이 고민을 하고 실제 프로젝트를 진행한 분들이 있기에 
원활히 할 수 있었습니다.[2] 

다만, Task 적인 프롬포트 경우, 어려워서 따로 요약 데이터 [3], 분류 데이터(감정 3가지) [4] 를 추가하여서 프롬포트 기반 데이터를 재구성하였고,
이를 기반으로 학습 결과 상당부분 결과가 나올수 있었습니다. (정보 공유 감사드립니다.)

코드 부분은 정리가 많이 필요한데, 추후 시간이 날 경우, 따로 정리해둘 예정입니다. 

학습한 최종 모델 (평가 모델 (비율 조정)기반으로 분리한 테스트 데이터셋까지 학습한 QLora 가중치 모델)은 허깅페이스에 업로드하였습니다. (상세한 정보는 추후 기약)
=> https://huggingface.co/oosij/llama2-ko-13b-3task/tree/main


달성한 점 :
- 조건 1-4) 달성 
- 3 task 수행 가능 (대화/요약/분류)
- 평가지표 상 괜찮은 점수 달성 (분류 기준 약 85%)
- 모델 및 데이터셋 구성에 따른 이해도

개선방안 : 
- 더 큰 backborn 모델 사용 필요
- 프롬포트 데이터셋 보강 및 최적화 (DPO 사용)
- 3 task -> 5 task 이상 진행 예정 (대화/요약/분류 add 글생성, 번역 등)
- 한글 데이터 및 기반 모델 부족으로 다양한 모델 비교 분석이 어려웠던 점 -> 구글 gemma 7b 등 ko 모델 예정
- sLLM 으로 13b 이하의 모델로 도메인 Task 최적화 모델 개선 진행 (우선 7b로 진행, 단일 GPU 3090/4090 inference 서버 가정하에 진행 )
- 그외 시간/비용/에러/도전/실패 등으로 시도하지 못한 다양한 기법 및 Approach.. 


Sample :
- 정성 평가에 사용되서 7b, 13b 각각 파리미터 조정 중 13b 조정버전을 선택 (30b, 70b은 시간/비용 상 추후 기약)

![image](https://github.com/oosij/sllm3task/assets/94098546/0d17bfea-c6db-41fd-b87d-3a69badfbe3b)



Model performance :
- 대화 (싱글/멀티2/멀티3) : 100 개씩 300개
- 요약 : 200개
- 분류 (긍정/중립/부정) : 100개씩 300개
- 총 800개로 평가


![image](https://github.com/oosij/sllm3task/assets/94098546/a9d1e220-4210-453c-82d4-c8cdbaa5700e)


![image](https://github.com/oosij/sllm3task/assets/94098546/a010365d-5f68-470b-a305-a206bdc129e3)


![image](https://github.com/oosij/sllm3task/assets/94098546/9cf93acc-7e7d-4493-b955-24a865ec1e14)





reference :

[1]  https://arxiv.org/abs/2303.16634

[2]  https://github.com/boostcampaitech5/level3_nlp_finalproject-nlp-12

[3]  https://aihub.or.kr/aihubdata/data/view.do?currMenu=&topMenu=&aihubDataSe=data&dataSetSn=582

[4]  https://github.com/ukairia777/finance_sentiment_corpus
