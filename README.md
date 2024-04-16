![image](https://github.com/sean03101/UWB-Indoor-Tracking/assets/59594037/4aef63b7-9708-4c42-9e33-93e7b3201cfd)# UWB-Indoor-Tracking

### 
[논문링크](https://www.dbpia.co.kr/pdf/pdfView.do?nodeId=NODE11466181&googleIPSandBox=false&mark=0&ipRange=false&b2cLoginYN=false&aiChatView=A&readTime=10-15&isPDFSizeAllowed=true&accessgl=Y&language=ko_KR&hasTopBanner=true)

### Abstract
UWB(Ultra Wide Band)기반 실내 측위 방식 중 TDoA(Time Difference of Arrival)를 활용한방식은범용적으로 사용되는 방식이지만, AWGN(Additive White Gaussian Noise)이 많은 환경일 때 위치 추적성능이현저히떨어진다. TDoA의 AWGN을 제거하는 방법이 많이 연구되어 왔지만 해당 방식은 별도의 측위방법을또필요로하며 다양한 환경에 대한 강건성(Robustness)이 부족하다. 본 논문에서는 AWGN이 포함된 TDoA를그대로사용해 물체의 위치를 강건하게 추정하는 ‘TDoA probabilistic image based target tracking(TPITT)’ 방법을제안한다. TPITT는 TDoA를 통해 각 영역에 물체가 존재할 확률을 이미지화하고, 'Convolution-LSTM' 모델을통해물체의좌표를 추정한다. 실험을 통해 제안하는 방법이 다양한 환경에서 강건하고 낮은 예측 오차를보임을입증하였다. 특히, AWGN이 많은 환경에서 사전 연구 ‘TDoA image based target tracking(TITT)’에 비해제안방법이더효과적임을 확인하였다


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
  - 다양한 공간에서 발생하는 AWGN에 대해 범용적으로 사용 가능한 강건한 모델 개발
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

    
![image](https://github.com/sean03101/UWB-Indoor-Tracking/assets/59594037/a3b9565c-1e63-42d3-80ba-184e28d93e9e)



### 참고 프로그램
TDOA 측정 컴퓨터 시뮬레이션 프로그램 소스
- https://github.com/umdlife/roadrunner_mavlink
- https://github.com/AlexisTM/MultilaterationTDOA
