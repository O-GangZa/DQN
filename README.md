# Repository
* data/factory_order_test.csv : test 데이터
* data/factory_order_train.csv : train 데이터
* data/obstacles.csv : 장애물 위치 좌표
* data/PQ_box.csv : 시작위치 T가 포함된 목적지 좌표
* data/Q_Finder.csv : 시작위치 T가 포함된 목적지 목록
* result_gif : DQN 결과물 디렉토리
* Final_Baseline_DDQN_v2.2.ipynb : DQN의 실행 노트북 파일

## DQN 아이디어 도식화
![image](https://user-images.githubusercontent.com/96896665/172666327-cadda250-083d-4b52-9607-e5f572e965ae.png)

기본적인 DQN의 아이디어는 행동가치 테이블(Q-Table)을 뉴럴 네트워크 함수인 Qθ(s, a)을 이용해서 근사하는 것입니다.
다시 말해 특정 상태에서 액션의 행동가치를 추정하는 것이라고 할 수 있습니다.

DQN의 핵심 아이디어은 다음과 같습니다.
Replay Buffer : 특정 상태의 액션과 리워드, 특정 상태의 다음 상태를 기억했다 랜덤하게 불러와 학습에 사용함
Target Network : 안정적으로 loss를 계산해서 파라미터 θ를 업데이트하기 위해서, 원본 네트워크의 사본을 만들고
                 일정 주기(sync preq)마다 업데이트 해줌
또한 정상적으로 구현 하였습니다.

뉴럴 네트워크는 중국인 방마냥

![image](https://user-images.githubusercontent.com/96896665/172724632-6513e9f0-4e23-4f9e-baee-af8c8613ee4c.png)

안에서 뭔가... 뭔가 일어나고 있지만 어떤 건지 알기는 쉽지 않다.
때문에 우리들은 초반에 막연하게 시작좌표와 목적좌표만 넣어주면
알아서 뿅하고 정확한 Q-value를 내줄 것을 기대했지만
다들 여기저기서 곡소리를 내기 시작했다.
(퍼실님 포함)

이전에 테이블 기반 Q러닝에서 사용했던 방법을 적용해보기도하고
다양한 아이디어를 접목시키면서 DQN을 성공적으로 해내기 위해 노력하고자 했다.


## 모델 개선사항
![image](https://user-images.githubusercontent.com/96896665/172663529-1c8414f5-5439-43ae-b02e-6ffc17c58d1f.png)


![image](https://user-images.githubusercontent.com/96896665/172663971-572babd6-37fb-46a4-a8bd-a640b7938ef9.png)


![image](https://user-images.githubusercontent.com/96896665/172664491-ad48516d-aded-48f2-bc2a-0af518e35a4b.png)


![image](https://user-images.githubusercontent.com/96896665/172665792-fba90ae3-ceae-431d-96fb-96b6edce2e54.png)

![image](https://user-images.githubusercontent.com/96896665/172664834-02788c86-37d1-4836-b1ab-b31a8f056b3b.png)

![image](https://user-images.githubusercontent.com/96896665/172500258-a5c05c51-3200-49b9-9f2e-e806a3e33170.png)

![image](https://user-images.githubusercontent.com/96896665/172665261-3409a3d4-1839-4973-8e1d-f4319b01dbce.png)

## 문제 해결 접근방법
![image](https://user-images.githubusercontent.com/96896665/172665424-fb3418a9-df56-4027-998d-16953390b65d.png)


## 결과 GIF
![KakaoTalk_20220608_105933650](https://user-images.githubusercontent.com/96896665/172556914-95015cc3-0349-46a6-bd27-5c9363cac509.gif)
