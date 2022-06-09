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

<br>

DQN의 메커니즘은 다음과 같이 이해할 수 있습니다.

> 게임을 할 때 Q라는 사람을 옆에 두고 매번 Q의 의견을 물어서 플레이에 참고하는 것입니다. 학습을 처음 시작 할 때는 Q의 실력은 인간 이하입니다. 강화학습을 한다는 것은 Q의 의견대로 게임을 한 뒤 졌으면 혼내고, 이기면 칭찬해서 Q의 판단력을 프로게이머 수준으로 키웁니다.

<br>

DQN을 학습할 때 필요한 요소는 다음 두가지 입니다. <br>
Replay Buffer : 특정 상태의 액션과 리워드, 특정 상태의 다음 상태를 기억했다 랜덤하게 불러와 학습에 사용하게 되는데, 해당 정보를 저장하는 메모리입니다.<br>
Target Network : 안정적으로 loss를 계산해서 파라미터 θ를 업데이트하기 위해서, 원본 네트워크의 사본을 만들고
                 일정 주기(sync preq)마다 업데이트 해줍니다.

뉴럴 네트워크는 중국인 방마냥 안에서 뭔가... 뭔가 일어나고 있지만 어떤 건지 알기는 쉽지 않습니다.

![image](https://user-images.githubusercontent.com/96896665/172724632-6513e9f0-4e23-4f9e-baee-af8c8613ee4c.png)

저희는 처음에 이 블랙박스를 긍정적인 방향으로만 생각했기 때문에 우리들은 초반에 막연하게 시작좌표와 목적좌표만 넣어주면
알아서 뿅하고 정확한 Q-value를 내줄 것을 기대했지만
다들 여기저기서 곡소리를 내기 시작했습니다.
(퍼실님 포함)<br>

이전에 테이블 기반 Q러닝에서 사용했던 방법을 적용해보기도하고
다양한 아이디어를 접목시키면서 DQN을 성공적으로 해내기 위해 노력하고자 했다.


## 모델 개선사항
![image](https://user-images.githubusercontent.com/96896665/172663529-1c8414f5-5439-43ae-b02e-6ffc17c58d1f.png)

기존의 그리드 월드는 빈선반 100, 목적지 -100, 장애물 0, 현위치 -5, 이동할 수있는 길은 1로 표시되어 있었습니다. 기준이 없이 그리드 월드가 배치 되어 있었기 때문에, 에이전트가 그리드 값의 값만으로는 어떤 것이 좋은지, 어떤 의미인 지 알 수 없을 것 같았습니다. 

그래서 빈선반 -1, 목적지 10, 9, 8 ..(목적지 값 차등화), 장애물 -10, 현위치 1, 이동할 수 있는 길 0으로 표시해 주면서 그리드 값 간의 편차를 감소 시켜주었습니다.

![image](https://user-images.githubusercontent.com/96896665/172663971-572babd6-37fb-46a4-a8bd-a640b7938ef9.png)

초반에 시도한 DQN 모델은  input 길이가 4개 뿐이었습니다. 기업 멘토링을 통해 이러한 정보는 강자가 어떤 행동을 판단할 지에 대한 정보가 너무 부족하다는 평을 들었습니다.

그래서 input 정보를 그리드 월드로 바꾸어주게 되었고, 보다 더 풍부하게 해주기 위해 연속된 history 정보 3장을 더 넣어 4장을 넣어주기 시작하였습니다.

![image](https://user-images.githubusercontent.com/96896665/172664491-ad48516d-aded-48f2-bc2a-0af518e35a4b.png)

기존의 보상체계는 이동보상 +0.1 장애물에 닿으면 +0.1 골인 보상 10이었습니다. 이렇게 되면 어디로 가든 + 보상을 받기 때문에 강자는 아이템을 찾으려하지도 골인을 하려는 노력조차 하지않습니다.

어딜 가든 칭찬을 받고 있기 때문이죠! ~~칭찬만으로 아이를 키우면 안되는 이유,,~~

최종 보상체계는 아이템으로 다가가는 방향이면 칭찬, 멀어지면 맴매
그리고 아이템으로 가까이갈 수록 받을 수 있는 보상의 총합이 크고, 멀어질수록 작도록 구현되어 있습니다.

이로써, 강자는 아이템으로 가야겠다는 목적이 생기게 되고, 실제로 아이템을 먹으러 향하게 됩니다.

의도대로 되고 있군. 😎


![image](https://user-images.githubusercontent.com/96896665/172665792-fba90ae3-ceae-431d-96fb-96b6edce2e54.png)

![image](https://user-images.githubusercontent.com/96896665/172664834-02788c86-37d1-4836-b1ab-b31a8f056b3b.png)

<!-- ![image](https://user-images.githubusercontent.com/96896665/172500258-a5c05c51-3200-49b9-9f2e-e806a3e33170.png)
 -->
<img src="https://user-images.githubusercontent.com/80737049/172749375-3d4eac9e-2536-4167-a81f-cb5a8fc9e368.png" width="30%" height="70%">

Trial & error를 이용하여 하이퍼 파라미터를 찾았습니다. 위와 같이 한가지 요소만 변경해보며, 어떤 하이퍼파라미터가 가장 큰 영향을 주는지 알아보았습니다.
그 실험했던 로그들은 아래 페이지에서 확인해 보실 수 있습니다 🤗🤗

https://padlet.com/tmsk0711/ogangza_parametertuning

## 문제 해결 접근방법
![image](https://user-images.githubusercontent.com/96896665/172665424-fb3418a9-df56-4027-998d-16953390b65d.png)


## 결과 GIF
![KakaoTalk_20220608_105933650](https://user-images.githubusercontent.com/96896665/172556914-95015cc3-0349-46a6-bd27-5c9363cac509.gif)
