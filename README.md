# UWB-Indoor-Tracking

### 
[논문주소](https://www.dbpia.co.kr/pdf/pdfView.do?nodeId=NODE11466181&googleIPSandBox=false&mark=0&ipRange=false&b2cLoginYN=false&aiChatView=A&readTime=10-15&isPDFSizeAllowed=true&accessgl=Y&language=ko_KR&hasTopBanner=true)

### Abstract
UWB(Ultra Wide Band)기반 실내 측위 방식 중 TDoA(Time Difference of Arrival)를 활용한방식은범용적으로 사용되는 방식이지만, AWGN(Additive White Gaussian Noise)이 많은 환경일 때 위치 추적성능이현저히떨어진다. TDoA의 AWGN을 제거하는 방법이 많이 연구되어 왔지만 해당 방식은 별도의 측위방법을또필요로하며 다양한 환경에 대한 강건성(Robustness)이 부족하다. 본 논문에서는 AWGN이 포함된 TDoA를그대로사용해 물체의 위치를 강건하게 추정하는 ‘TDoA probabilistic image based target tracking(TPITT)’ 방법을제안한다. TPITT는 TDoA를 통해 각 영역에 물체가 존재할 확률을 이미지화하고, 'Convolution-LSTM' 모델을통해물체의좌표를 추정한다. 실험을 통해 제안하는 방법이 다양한 환경에서 강건하고 낮은 예측 오차를보임을입증하였다. 특히, AWGN이 많은 환경에서 사전 연구 ‘TDoA image based target tracking(TITT)’에 비해제안방법이더효과적임을 확인하였다














### 참고 프로그램
TDOA 측정 컴퓨터 시뮬레이션 프로그램 소스
- https://github.com/umdlife/roadrunner_mavlink
- https://github.com/AlexisTM/MultilaterationTDOA
