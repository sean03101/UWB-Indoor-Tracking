# TDoA 확률화 이미지를 활용한 딥러닝 기반 타겟 추적 모델

### 
[논문링크](https://www.dbpia.co.kr/pdf/pdfView.do?nodeId=NODE11466181&googleIPSandBox=false&mark=0&ipRange=false&b2cLoginYN=false&aiChatView=A&readTime=10-15&isPDFSizeAllowed=true&accessgl=Y&language=ko_KR&hasTopBanner=true)

### Abstract
UWB(Ultra Wide Band)기반 실내 측위 방식 중 TDoA(Time Difference of Arrival)를 활용한 방식은 범용적으로 사용되는 방식이지만, AWGN(Additive White Gaussian Noise)이 많은 환경일 때 위치 추적 성능이 현저히 떨어진다. TDoA의 AWGN을 제거하는 방법이 많이 연구되어 왔지만 해당 방식은 별도의 측위방법을 또 필요로 하며 다양한 환경에 대한 강건성(Robustness)이 부족하다. 본 논문에서는 AWGN이 포함된 TDoA를 그대로 사용해 물체의 위치를 강건하게 추정하는 ‘TDoA probabilistic image based target tracking(TPITT)’ 방법을 제안한다. TPITT는 TDoA를 통해 각 영역에 물체가 존재할 확률을 이미지화하고, 'Convolution-LSTM' 모델을 통해 물체의 좌표를 추정한다. 실험을 통해 제안하는 방법이 다양한 환경에서 강건하고 낮은 예측 오차를 보임을 입증하였다. 특히, AWGN이 많은 환경에서 사전 연구 ‘TDoA image based target tracking(TITT)’에 비해 제안 방법이 더 효과적임을 확인하였다.


### Introduction

- 소위 GPS라 불리는 글로벌 네비게이션 위성 시스템(Global Navigation Satellite System)은 인공위성을 사용해 위치를 추적하는 기술

- 하지만 거리 오차 단위가 크며(수십 미터), 실내 혹은 지하 공간에서는 사용이 불가

- 이러한 단점 때문에 실내에서 물체의 위치를 파악하기 위해 UWB, Wi-Fi, Bluetooth 등의 대체 기술들이 개발

그 중 다음과 같은 장점 때문에 최근 UWB 기술(짧은 시간의 낮은 출력의 펄스 신호를 사용하여 500MHz 이상의 넓은 주파수 대역으로 데이터를 송수신하는 100m 이내의 무선통신 기술)이 주로 사용됨
  1) 수십 cm 이내의 측위 정확성
  2) Multipath 등의 간섭에 대한 저항성
  3) PHY layer와 난수를 활용한 보안성

![image](https://github.com/sean03101/UWB-Indoor-Tracking/assets/59594037/fb29184b-9450-4e6e-8cef-6aaf9e220ae5)

- 하지만 매점, 헬스장 같이 장애물이 많은 공간에서는 UWB 신호가 Additive White Gaussian Noise(AWGN)에 의해 변형되어 위치 추적 성능이 급격히 감소

- AWGN에 대해 강건한 위치 추적 모델을 만들기 위해 딥러닝과 머신 러닝 기법을 활용한 연구 진행

- 사전 연구들은 신호를 분해하여 AWGN이 제거된 깨끗한 신호를 얻고자 하지만, 많은 보정 프로세스를 거쳐 실시간으로 사용하기 힘들며, 실험 환경의 변화에 따라 성능이 강건하지 못한 단점을 가짐

**연구 목적** 
  - TDOA 이미지 변환으로 통한 다양한 공간에서 발생하는 AWGN에 대해 범용적으로 사용 가능한 강건한 모델 개발
  - 측정한 TDOA를 그대로 사용하는 간단한 프로세스의 end to end 모델 개발
  - 이동하는 물체를 실시간으로 추적하는 시계열 모델 개발

### 사전 연구
  - UWB를 활용해 위치를 추정하는 방식은 time of arrival(ToA / 송신기에서 빛을 쏘아 수신기에서 반사되어 돌아오는 시간을 측정한 뒤, 빛의 속도를 곱하여 거리를 구하는 방식) 방법 존재
  - ToA 단점을 보완한 TDOA(Time Difference of Arrival / 측정된 신호 시간의 차이를 이용)
  - TDOA를 사용한 위치 추정 시스템의 순서는 다음과 같음
    1) UWB 태그가 메시지를 송신
    2) 주변의 UWB 앵커가 메시지를 수신
    3) 중앙 컴퓨팅 유닛(CCU)은 전체 UWB 앵커들의 데이터를 수집하여 각 앵커들 간 신호 도달시간 차이(TDOA)를 계산
    4) TDOA를 이용해 쌍곡선 방정식을 도출
    5) 방정식들의 해를 계산하여 UWB 태그의 위치를 추정


### 제안 모델
![image](https://github.com/sean03101/UWB-Indoor-Tracking/assets/59594037/f6861350-7f5f-48c6-9321-abddc45894e1)
  
  - 3개 이상의 UWB 앵커와 1개의 태그를 사용해 얻은 TDOA들을 입력 값으로 설정
  - TPITT(TDOA Probabilistic Image based moving-Target Tracking / **측정된 TDOA들을 ‘TDOA 이미지 시계열 데이터' 변환 뒤, conv-lstm을 통한 물체의 위치 추적**)를 통해 물체의 공간 좌표 벡터 추정

![image](https://github.com/sean03101/UWB-Indoor-Tracking/assets/59594037/2cae4c0c-0fc5-4986-9fcf-8f43ccc9b1eb)

  - 이미지 변환 인풋 : 이미지 변환 과정을 위해 앵커 2개 와 거기서 얻은 TDOA
  - 이미지 변환시 필요한 하이퍼파라미터 : 공간 좌표 이미지 크기, 격자 크기, 최소 거리 차이 상수(ε / TDOA와 앵커와 격자 중점의 거리 차이 유사 비교 상수), temperature(T / sigmoid 함수의 출력 값을 보정하는 양수)
  - 변환 과정
      1) 행렬의 원소가 격자의 중점과 기준 앵커(anchor_ref)의 거리와 격자의 중점과 나머지 앵커(anchor_n)의 거리 차이의 절대값인 행렬 M 생성
      2) sigmoid 함수를 이용해 행렬 M의 원소 smoothing 진행(이때, temperature 상수 T를 활용해 smoothing 정도를 조절 (𝐌_(𝐢,𝐣)=𝟏∕〖𝟏+𝐞^((|(𝐂_(𝐢,𝐣)−𝐀)−(𝐂_(𝐢,𝐣)−𝐀^′ )−𝐬|−𝛆)/𝐓) 〗)
  - 해당 과정을 통해 **실제 쌍곡선 방정식과 유사한, 이미지 내의 격점에 쌍곡선이 지나갈 확률로 표시된 TDOA 이미지 생성**


  - Convolutional-LSTM 모델을 사용하여 물체의 공간 좌표에 대해 regression
  - Loss 함수 : Mean Square Error(MSE=1/𝑁 ∑1_(𝑖=1)^𝑁▒〖(𝑦_𝑖−𝑦 ̂_𝑖)〗^2   (N : Dataset의 크기, y : 물체의 실제 공간 좌표, ŷ : 예측한 물체의 공간 좌표))를 사용


### 실험
- 실험 세팅
    - 제안한 모델을 검증하기 위해 컴퓨터 시뮬레이션을 진행
    - 단일 물체에 대해 위치를 추정하기 위해 크기가 10m * 10m의 정사각형 공간을 설정
    - 각 꼭지점에 4개의 센서가 위치하고 물체의 움직임을 추적하고 있으며, 그 중 하나가 기준 센서로 선택된다고 가정
    - 채널 모델은 IEEE 802.15.4a의 사무실 내 환경을 가정하며 “Bitcraze” 사의 “loco positioning system” 모듈을 이용해 가상 수신 신호 생성
    - 각 센서는 TOA를 기반으로, TDOA는 기준 센서와 나머지 센서 간의 TOA 차이로 중앙 컴퓨팅 유닛(CCU)에서 계산
    - TDOA 측정은 millisecond 단위로 이루어지며 각 센서와 CCU 사이의 통신 지연은 없다고 가정
    - **다양한 환경을 가정**하기 위해 **환경에 따라 각각 다른 측정 AWGN을 적용**
        1) AWGN이 극심한 환경(Noise extreme) : 평균 0, 표준 편차 2
        2) AWGN이 높은 환경(Noise high) : 평균 0, 표준 편차 1
        3) AWGN이 적은 환경(Noise low) : 평균 0, 표준 편차 0.2
    - 다양한 물체의 움직임에 대해 검증하기 위해 검증용 물체가 전체 공간 혹은 기준 앵커 주변의 좁은 공간(기준 앵커 반경 2m)에서만 다음과 같이 움직인다 가정
        1) 지그재그(zigzag)
        2) 랜덤(random walk)
        3) 회전 운동(Rotational motion)


- 실험 결과
![image](https://github.com/sean03101/UWB-Indoor-Tracking/assets/59594037/0a3c7549-882d-4507-a2a8-8b0feb27fa97)

  - Convolutional –LSTM 기반 모델이 **모든 상황에서 평균적으로 가장 좋은 예측**을 하는 결과를 보임
  - 랜덤으로 움직이는 물체를 학습하면서 **다른 움직임을 보이는 물체 또한 운동성을 파악**하여 물체의 움직임을 잘 예측할 수 있었던 것으로 추정

![image](https://github.com/sean03101/UWB-Indoor-Tracking/assets/59594037/b44f249c-9db8-44a3-b4c0-9076d6696e50)
  
  - Convolutional–LSTM과 FCNNs 모델이 예측한 결과를 좌표평면에 시각화
  - 전체 공간에서 물체가 운동(그림 우측)에 비해 앵커 주변의 공간에서 운동 (그림 좌측)하는 상황일 경우 **FCNNs 모델의 성능 저하가 심함**
  - 이미지 기반의 모델은 숫자 기반의 모델과 다르게 **보편적인 상황만 학습을 진행**하여도 다양한 상황에 대해 **섬세하고 강건한 예측 결과**를 보임

![image](https://github.com/sean03101/UWB-Indoor-Tracking/assets/59594037/9ce0ecbf-5ec4-44df-93d2-f7a73461ca89)
   
  - AWGN의 변화와 모델 종류에 따른 실험 결과의 평균 변화 그래프
  - **모든 상황에서 환경의 AWGN가 증가할수록** TITT와 TPITT의 **성능 차이가 커지는** 경향을 보임
  - TPITT의 확률화 과정은 이미지 변환 방식의 주요 목적인 **AWGN를 제거하는 역할을 더 잘 수행**


## 결론
- 연구 요약 : AWGN이 많은 다양한 환경, 상황, 물체에 대해 보편적으로 사용할 수 있는 위치 추적 방법 제안
- 연구 의의
    - 왜 **이미지 기반 모델**인가 : TDOA를 그대로 사용하는 모델보다 일반적이지 않는 상황에서 강건한 성능을 보임
    - 왜 **TPITT** 방법인가 : AWGN이 많은 환경일수록 다른 방법들 보다 AWGN을 효과적으로 제거하여 예측 오차가 작음
    - 왜 **Convolutional-LSTM**인가 : 물체의 운동성을 학습하여 상황, 환경 상관 없이 움직이는 물체를 추적할 때 좋은 성능을 보임


### 참고 프로그램
TDOA 측정 컴퓨터 시뮬레이션 프로그램 소스
- https://github.com/umdlife/roadrunner_mavlink
- https://github.com/AlexisTM/MultilaterationTDOA

### 수상
![image](/ppt & 논문/KakaoTalk_20240417_124511583.jpg)
