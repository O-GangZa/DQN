# Repository
* data/factory_order_test.csv : test 데이터
* data/factory_order_train.csv : train 데이터
* data/obstacles.csv : 장애물 위치 좌표
* data/PQ_box.csv : 시작위치 T가 포함된 목적지 좌표
* data/Q_Finder.csv : 시작위치 T가 포함된 목적지 목록
* result_gif : DQN 결과물 디렉토리
* Final_Baseline_DDQN_v2.2.ipynb : DQN의 실행 노트북 파일

## DQN 이란?
![image](https://user-images.githubusercontent.com/96896665/172666327-cadda250-083d-4b52-9607-e5f572e965ae.png)

기본적인 DQN의 아이디어는 행동가치 테이블(Q-Table)을 뉴럴 네트워크 함수인 Qθ(s, a)을 이용해서 근사하는 것입니다.
여기서 Q함수는 상태와 행동을 입력으로 주면 기대값을 출력해주는 함수입니다. 다르게 말하면, 특정상태에서의 액션 가치를 추정하는 것이라고 할 수 있습니다.

<br?<br?

DQN의 메커니즘은 다음과 같이 비유할 수 있습니다.

<br>

게임을 할 때 Q라는 사람을 옆에 두고 매번 Q의 의견을 물어서 플레이에 참고하는 것입니다. 학습을 처음 시작 할 때는 Q의 실력은 인간 이하입니다. 강화학습을 한다는 것은 Q의 의견대로 게임을 한 뒤 졌으면 혼내고, 이기면 칭찬해서 Q의 판단력을 프로게이머 수준으로 키웁니다.

<br><br>

DQN을 학습할 때 필요한 요소는 다음 두가지 입니다. <br>
Replay Buffer : 특정 상태의 액션과 리워드, 특정 상태의 다음 상태를 기억했다 랜덤하게 불러와 학습에 사용하게 되는데, 해당 정보를 저장하는 메모리입니다.<br>
Target Network : 안정적으로 loss를 계산해서 파라미터 θ를 업데이트하기 위해서, 원본 네트워크의 사본을 만들고
                 일정 주기(sync preq)마다 업데이트 해줍니다.

뉴럴 네트워크는 중국인 방마냥

![image](https://user-images.githubusercontent.com/96896665/172724632-6513e9f0-4e23-4f9e-baee-af8c8613ee4c.png)

안에서 뭔가... 뭔가 일어나고 있지만 어떤 건지 알기는 쉽지 않습니다.<br>
오히려 이 블랙박스를 긍정적인 방향으로만 생각했기 때문에 우리들은 초반에 막연하게 시작좌표와 목적좌표만 넣어주면
알아서 뿅하고 정확한 Q-value를 내줄 것을 기대했지만
다들 여기저기서 곡소리를 내기 시작했습니다.
(퍼실님 포함)<br>

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
