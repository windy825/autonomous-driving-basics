# Sensors

사람으로 치면 눈 역할을 한다.



#### 인지

- **Camera**

  시각적으로 보이는 정보를 찍기에 다양한 정보 취득

  문자인식 가능 (교통 표지판, 차선, 신호등)

  ```
  차선의 색 추출
  이진화
  차선에 해당하는 부분만 크롭
  차선의 곡률 계산을 위해 이미지 왜곡
  (마치 하늘에서 내려다본 것처럼)
  곡률 계산하여 매 위치마다의 조항각 구하기
  ```

  - 낮은 단가로 자율주행 차량을 구성하기에 부담이 적다.

    카메라 센서를 이용하는 것은 가격적 측면이 큼

  - 단독으로는 물체의 정확한 거리 측정 힘듬

  - 사람의 눈과 같이 가시광선을 이용하기 때문에. 

    밤이나 악천후 등 기상상태에 영향을 많이 받는다.

  <br>

  - **종류**

    - Pinhole 

    - Fisheye 어안렌즈

      물 밑에서 수면위를 보는 경우, 물의 굴절로 인해 물위의

      풍경이 원형으로 보임

      광각이 가능하나, 끝 지점은 왜곡이 발생

    - Ground Truth 

      지상 실측 정보로 해석됨.

      인공위성과 같이 지구 멀리서 봤을때, 넓게 볼수 있지만, 

      디테일한 모습은 왜곡될 수 있음.

      머신러닝 관점에서

      GT는 학습하고자 하는 데이터의 원본 혹은 실제 값 표현

      `RGB (기본 카메라) `

      `Semantic (같은 객체끼리 구분, Seen을 나타낼때 주로 사용, 차량이 주행 할 영역 판단 가능)`

      `Instance (Instance based segmentation, `

      `객체를 식별하는 기능, 각 객체 바운딩 박스, `

      `객체의 위치와 속도 정보를 가지고 있음)`

      시그멘틱을 커버할 순 있겠지만, 제한된 요소에서는

      두개를 잘 사용해야 한다.

      - 만약 이미지 처리 알고리즘을 검증한다면, GT들과 

        비교해보는 것이 하나의 방법이 된다. 

  <br>

  - **Parameter Settings**

    - intrinsic 내부 카메라 정보

      Focal Length, Principal Point, Image Sensor Size 등

    - extrinsic 카메라 외부 정보

      장착점, 장착위치, 자세 등

  <br>

  - **Communication interface**
    - ROS protocol

<br>

<br>

- **LIDAR**

  - 레이저 펄스를 쏘아서, 반사되어 돌아오는 시간으로 거리 인지
  - 작은 물체도 감지 할 정도로 정밀도 높다.
  - 형태 인식 가능 (3D 이미지 제공 가능)
  - 높은 에너지 소비, 높은 가격대, 큰 외형
  - 악천후에 약함 (펄스가 눈이난 비에 반사됨)

  <br>

  - Simulator
    - Intensity(GT) - 펄스가 물체에 반사되어 돌아오는 각도
  - 파라메터
    - 카메라와 같음
    - 실제와 비슷한 환경을 만들기 위해선, 노이즈 모델을 사용
      - `Gaussian Noise`사용
  - Communication interface
    - ROS protocol

<br>

<br>

- **Radar**
  - 전파를 매개체로 사물간 거리와 형태 파악
  - 라이다 보다 정밀도가 떨어질 수 있음
  - 악천후에 강함
  - 소형화 가능
  - 사이즈가 곧 성능
  - 작은 물체 식별 어려움
  - 정확한 이미지 제공이 어려움 

<br>

- CAMERA, LIDAR 는 감지 범위를 삼각함수로 구함
- 라이다는 RESOLUTION 채널수가 중요하다. (감지범위 관련)

<br>

- **GPS**

  31개 인공위성 기반으로 위치정보 획득

  가성비가 좋음

  최소 4개의 인공위성과 연결되어야 위치 계산이 가능

  숲이나 건물 터널등의 장애물에 영향을 많이 받음

  신호에 노이즈가 많기 때문에 필터링 필요

  사용을 위해선 GPS가 주는 3차원 좌표를 2D로 받아야 함

  - `WGS84(3D, 타원체에서 위도 경도의 3차원 좌표) -> UTM(2D)`

    경로계획을 위해선 위의 작업이 필요하다.

  <br>

  시뮬레이터

  - WGS84 모델 기반 정보 획득
  - 가우시안 모델 (노이즈도 있음)
  - 파라메터
    - 위치와 좌표
    - 수신 빈도
    - 통신방도 등

<br>

- **IMU (Inertia Measurement Unit)**

  관성 측정 장비

  3측에 대한 각속도 측정하는 자이로 센서

  3측의 가속도를 측정하는 가속도 센서

  우리는 중력가속도를 받기 때문에,   측정 가능

  ```
  x축 : Roll
  y축 : Pitch
  z축 : Yaw
  ```

  자동차 뿐만 아니라, 3차원 좌표계를 사용 하는 모든 곳에 사용됨.

  <br>



#### localization 센서 (GPS, IMU)

##### (정확히 분류할 순 없지만, 인지에 도움을 주므로 인지 센서라고도 할 수 있음)