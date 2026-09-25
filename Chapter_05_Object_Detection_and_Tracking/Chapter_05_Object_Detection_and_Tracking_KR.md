**Volume 12. Autonomous Driving Software**

# Chapter 05. Object Detection and Tracking

## 05.01. 3D Object Detection Architecture BEV Pillar Voxel [w/Code]

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

3차원 객체 검출(3D Object Detection)은 자율주행 시스템(Autonomous Driving System)과 실외 자율이동로봇(Outdoor AMR)의 핵심 인지(Perception) 기능이다. 주행 계획(Planning)을 위해서는 단순히 객체가 존재한다는 사실을 인식하는 것만으로는 충분하지 않다. 인지 스택(Perception Stack)은 3차원 공간에서 객체의 위치(Position), 크기(Dimensions), 방향(Orientation), 클래스(Class), 신뢰도(Confidence)를 추정해야 한다. 제공된 구조에서 이 주제는 객체 검출 및 추적(Object Detection and Tracking) 장의 시작 부분에 배치되며, 이후 라이다(LiDAR), 카메라(Camera), 센서 융합(Sensor Fusion), 추적(Tracking) 기법으로 확장된다.

일반적인 3차원 검출기(3D Detector)는 원시 센서 관측값(Raw Sensor Observation)을 중간 공간 표현(Intermediate Spatial Representation)으로 변환한 후 검출 네트워크(Detection Network)를 적용한다. 라이다(LiDAR)의 입력은 좌표(Coordinates)와 반사 강도(Intensity) 등의 속성을 포함하는 비정렬 포인트 집합(Unordered Point Set)이다. 이러한 포인트를 직접 처리할 수도 있지만, 자율주행 시스템에서는 공간 구조를 명확하게 표현하고 효율적인 합성곱(Convolution) 또는 트랜스포머(Transformer) 기반 특징 추출을 위해 BEV, 필러(Pillar), 복셀(Voxel) 표현으로 구성하는 방식이 널리 사용된다.

조감도(Bird\'s-Eye View, BEV)는 환경을 위에서 내려다보는 시점(Top-Down Perspective)으로 표현한다. 센서 정보는 지면과 정렬된 2차원 평면에 투영되거나 인코딩(Encoding)되며, 수평축은 일반적으로 종방향(Longitudinal) 및 횡방향(Lateral) 위치를 나타낸다. 높이(Height), 반사 강도(Intensity), 밀도(Density), 의미 정보(Semantic Information), 학습된 특징(Learned Features) 등은 채널(Channel) 형태로 저장할 수 있다. 이러한 표현은 위치 추정(Localization), 예측(Prediction), 점유 추정(Occupancy Estimation), 모션 계획(Motion Planning)에서 사용하는 좌표계와 자연스럽게 연결된다.

BEV의 주요 장점은 객체들이 지면 평면(Ground Plane)에서 서로 간의 공간적 관계(Spatial Relationship)를 유지한다는 것이다. 따라서 차량(Vehicle), 보행자(Pedestrian), 자전거(Bicycle), 방호물(Barrier) 및 기타 장애물을 검출하면서 자차 또는 로봇 플랫폼(Ego Platform)과의 미터 단위 관계를 유지할 수 있다. 원근 카메라 영상(Perspective Camera Image)과 달리 BEV에서는 원근 효과 때문에 먼 객체가 본질적으로 작아지는 현상이 발생하지 않는다. 이러한 특성은 인지 결과를 경로 계획(Trajectory Planning)과 충돌 회피(Collision Avoidance)에 직접 전달해야 하는 시스템에서 특히 유용하다.

BEV는 반드시 특정 센서에 종속된 표현은 아니다. 라이다 포인트(LiDAR Point)는 BEV로 투영할 수 있으며, 다중 카메라(Multi-Camera)의 특징은 영상 좌표(Image Coordinates)에서 3차원 공간으로 리프트(Lift)한 뒤 공통 BEV 특징 맵(Common BEV Feature Map)으로 변환할 수 있다. 레이더(Radar) 측정값 역시 통합할 수 있다. 따라서 현대적인 아키텍처(Architecture)에서는 서로 다른 센서가 기하학적 정보(Geometric Information), 의미 정보(Semantic Information), 속도(Velocity), 신뢰도(Confidence)를 상호 보완적으로 제공하는 공통 공간 인터페이스(Common Spatial Interface)로 BEV를 활용하는 경우가 증가하고 있다.

필러 기반 아키텍처(Pillar-Based Architecture)는 불규칙한 포인트 클라우드(Point Cloud)를 구조화된 텐서(Structured Tensor)로 변환하는 효율적인 방법을 제공한다. 수평 환경을 필러(Pillar)라고 하는 수직 기둥 형태의 영역으로 분할하며, 높이 방향은 여러 개의 복셀 계층(Voxel Layer)으로 세분화하지 않는다. 각 필러에 포함된 포인트는 좌표, 오프셋(Offset), 반사 강도 및 학습된 포인트 특징(Learned Point Feature)을 이용해 압축된 특징 벡터(Feature Vector)로 인코딩된다. 이렇게 생성된 의사 영상(Pseudo-Image)은 일반적인 2차원 합성곱 신경망(2D Convolutional Neural Network)을 이용하여 효율적으로 처리할 수 있다.

필러 방식(Pillar Approach)은 높이 방향에 대한 조밀한 이산화(Dense Discretization)를 피하기 때문에 계산 복잡도(Computational Complexity)를 줄일 수 있다. 이러한 특성은 GPU 메모리(GPU Memory), 추론 지연시간(Inference Latency), 전력 소비(Power Consumption), 열 제한(Thermal Limit)이 중요한 임베디드 자율주행 시스템(Embedded Autonomous System)에 유리하다. 특히 도로 차량, 물류 야드(Logistics Yard), 캠퍼스(Campus), 항만(Port), 다양한 실외 AMR 운용 영역처럼 대부분의 주요 객체가 지면 중심 환경(Ground-Oriented Environment)에 존재하는 경우 필러 표현은 효과적이다.

그러나 높이 방향을 단순화하면 한계도 발생한다. 서로 다른 높이에 존재하는 포인트들이 동일한 필러 안에서 통합되면서 세부적인 3차원 기하 정보(3D Geometry)가 일부 압축될 수 있다. 이러한 문제는 불규칙 지형(Irregular Terrain), 돌출 구조물(Overhanging Structure), 식생(Vegetation), 적재 장비(Loading Equipment), 적층 객체(Stacked Object), 또는 수직 방향의 분리가 중요한 의미 정보를 제공하는 환경에서 중요해질 수 있다. 따라서 필러 해상도(Pillar Resolution)는 기하학적 충실도(Geometric Fidelity)와 계산 효율성(Computational Efficiency) 사이의 실용적인 절충점으로 이해해야 한다.

복셀 기반 아키텍처(Voxel-Based Architecture)는 3차원 구조를 보다 명시적으로 보존한다. 센싱 공간(Sensing Volume)을 x, y, z축 방향의 개별 셀(Cell)로 분할하고, 각 복셀 내부의 포인트를 학습된 특징(Learned Feature)으로 변환한다. 라이다 공간의 대부분은 비어 있기 때문에 희소 3차원 합성곱(Sparse 3D Convolution)이 일반적으로 사용된다. 모든 가능한 셀을 처리하는 대신 희소 연산(Sparse Operation)은 점유된 복셀(Occupied Voxel)에 계산을 집중하고 선택된 공간 위치를 통해 특징을 전달한다.

복셀화(Voxelization)는 수직 방향의 관계를 특징 추출 과정 전체에서 유지할 수 있기 때문에 강력한 기하학적 표현(Geometric Representation)을 제공한다. 세밀한 형상 정보(Fine-Grained Shape Information)는 지면상의 점유 형태는 비슷하지만 3차원 구조가 서로 다른 객체를 구별하는 데 도움을 줄 수 있다. 그러나 단순화된 필러 처리보다 계산량과 메모리 요구량이 증가한다. 작은 복셀은 공간 해상도(Spatial Resolution)를 높이지만 셀의 수를 증가시키며, 큰 복셀은 계산량을 줄이는 대신 기하학적 정밀도(Geometric Precision)를 감소시킨다.

현대적인 검출기(Detector)는 BEV, 필러, 복셀을 서로 배타적인 대안으로 취급하기보다 이러한 개념을 함께 결합하는 경우가 많다. 네트워크는 먼저 포인트 클라우드를 필러 또는 희소 복셀(Sparse Voxel)로 인코딩하고, 3차원 특징 또는 의사 영상 특징(Pseudo-Image Feature)을 추출한 후 이를 BEV로 압축하거나 변환할 수 있다. 이후 BEV 백본(BEV Backbone)이 광범위한 공간 추론(Spatial Reasoning)을 수행하고, 검출 헤드(Detection Head)가 클래스와 3차원 경계 상자(3D Bounding Box)의 매개변수를 추정한다. 따라서 전체 아키텍처는 개별 표현 방식이 아니라 표현이 순차적으로 변환되는 과정으로 이해하는 것이 적절하다.

일반적인 검출 파이프라인(Detection Pipeline)은 센서 입력(Sensor Input), 전처리(Preprocessing), 공간 인코딩(Spatial Encoding), 백본 특징 추출(Backbone Feature Extraction), 다중 스케일 특징 통합(Multi-Scale Feature Aggregation), 예측 헤드(Prediction Head)의 순서로 설명할 수 있다. 최종 헤드는 일반적으로 객체 신뢰도(Object Confidence), 의미 클래스(Semantic Class), 중심 좌표(Center Coordinates), 크기(Dimensions), 방향(Orientation)을 추정하며, 경우에 따라 속도(Velocity)나 추가 속성(Attribute)도 예측한다. 이러한 출력은 이후 추적 및 예측 모듈이 시간에 따라 검출 결과를 일관되게 연관시킬 수 있도록 명확하게 정의된 좌표계(Coordinate Frame)로 표현되어야 한다.

앵커 기반 검출기(Anchor-Based Detector)는 공간 위치마다 사전에 정의된 경계 상자 템플릿(Bounding-Box Template)을 배치하고, 이 템플릿을 기준으로 보정값(Correction)을 예측한다. 반면 앵커 프리 검출기(Anchor-Free Detector)는 객체 중심(Object Center)을 직접 추정하고 후보 위치 주변에서 경계 상자의 속성을 회귀(Regression)한다. 중심 기반 방식(Center-Based Formulation)은 객체 중심이 지면 평면에서 자연스러운 검출 목표가 되기 때문에 BEV 검출에 특히 적합하며, 수동으로 설계한 앵커 구성에 대한 의존도를 줄이고 다양한 크기의 객체 검출을 단순화할 수 있다.

특징 해상도(Feature Resolution)는 아키텍처를 결정하는 핵심 매개변수이다. 거친 BEV 격자(Coarse BEV Grid)는 메모리 소비를 줄이고 추론 속도를 높이지만, 보행자, 기둥, 콘(Cone), 원거리 객체와 같은 작은 대상을 매우 적은 수의 셀로 표현할 수 있다. 반대로 미세한 격자(Fine Grid)는 작은 객체 정보를 더 잘 보존하지만 계산량을 증가시킨다. 실제 시스템에서는 검출 거리(Detection Range), 위치 정확도(Localization Accuracy), 실시간 성능(Real-Time Performance)의 균형을 위해 다중 스케일 백본(Multi-Scale Backbone), 특징 피라미드(Feature Pyramid), 희소 처리(Sparse Processing), 선택적 고해상도 영역(Selective High-Resolution Region)을 활용한다.

검출 범위(Detection Range) 역시 중요한 절충 관계를 만든다. BEV 또는 복셀 격자가 포함하는 물리적 영역을 확장하면 멀리 있는 객체를 더 일찍 인식할 수 있지만, 고정된 텐서 크기에서는 공간 해상도가 감소한다. 검출 범위와 해상도를 동시에 증가시키면 텐서 크기가 크게 증가한다. 따라서 실외 AMR은 단순히 검출 범위를 최대화하기보다 운용 속도(Operational Speed), 정지 거리(Stopping Distance), 센서 범위(Sensor Range), 환경 구조(Environment Structure), 사용 가능한 엣지 컴퓨팅(Edge Computing) 자원을 고려하여 인지 영역(Perception Volume)을 설정해야 한다.

필러 인코더(Pillar Encoder)와 복셀 인코더(Voxel Encoder)의 선택 역시 운용 영역(Operating Domain)에 따라 달라진다. 객체 높이가 비교적 예측 가능한 구조화 도로(Structured Road)에서는 효율적인 필러 처리가 유리할 수 있지만, 복잡한 산업 환경이나 오프로드 환경(Off-Road Environment)에서는 보다 풍부한 복셀 기하 정보가 유용할 수 있다. 불규칙한 지면, 건설 장비, 식생, 경사면, 연석(Curb), 비정형 장애물은 수직 구조 정보의 중요성을 높인다. 따라서 공간 표현 방식은 시스템의 운용 설계 영역(Operational Design Domain, ODD)에 정의된 기하학적 특성과 위험 요소를 반영해야 한다.

실외 AMR에서 3차원 객체 검출은 독립적인 인지 기능으로 동작하기보다 주행 가능 영역 추정(Drivable-Area Estimation)과 밀접하게 연결된다. 앞선 장의 구조에는 라이다 기반 자유 공간 검출(LiDAR-Based Free-Space Detection), BEV 도로 분할(BEV Road Segmentation), 점유 격자 지도(Occupancy Grid Map), 오프로드 주행 가능성 분석(Off-Road Traversability Analysis), 음의 장애물 검출(Negative-Obstacle Detection)이 포함되어 있다. 강건한 자율주행 스택(Robust Autonomy Stack)은 이러한 환경 표현과 객체 검출 결과를 결합하여 주행 가능한 지형과 동적 또는 정적 위험 요소를 구분할 수 있다.

시간 정보(Temporal Information)는 검출 성능을 더욱 강화한다. 단일 포인트 클라우드는 특히 원거리에서 희소한 관측값(Sparse Observation), 부분 가림(Partial Occlusion), 측정 잡음(Measurement Noise)을 포함할 수 있다. 여러 시점의 스윕(Sweep)을 자차 움직임 보상(Ego-Motion Compensation)을 통해 공통 좌표계로 변환하고 누적하면 포인트 밀도를 증가시킬 수 있다. 그러나 움직이는 객체는 그 움직임을 적절하게 보상하지 않을 경우 시간적 번짐(Temporal Smearing)을 발생시킬 수 있다. 따라서 검출 아키텍처는 공간 표현뿐 아니라 타임스탬프 정렬(Timestamp Alignment) 및 운동 추정(Motion Estimation)과도 조화를 이루어야 한다.

검출 결과가 생성되면 다중 객체 추적(Multi-Object Tracking, MOT)이 프레임 간에 지속적인 객체 식별자(Persistent Identity)를 설정하고 시간에 따른 상태(Temporal State)를 추정한다. 이러한 이유로 제공된 소프트웨어 구조에서는 객체 검출과 추적이 하나의 장으로 구성되어 있으며, 3차원 객체 검출 이후 센서별 검출, 다중 센서 융합(Multi-Sensor Fusion), 다중 객체 추적, 속도 추정(Velocity Estimation), 운동 예측(Motion Prediction)으로 이어진다. 검출 품질(Detection Quality)은 데이터 연관(Data Association)의 안정성, 궤적 추정(Trajectory Estimation), 이후 행동 결정(Behavioral Decision)에 직접적인 영향을 미친다.

실시간 배포(Real-Time Deployment)에서는 신경망 정확도만 평가하는 것이 아니라 전체 파이프라인을 평가해야 한다. 포인트 클라우드 전송(Point-Cloud Transfer), 전처리, 복셀화 또는 필러화(Pillarization), GPU 실행(GPU Execution), 후처리(Post-Processing), 좌표 변환(Coordinate Transformation), 미들웨어 통신(Middleware Communication), 추적 과정 모두가 종단간 지연시간(End-to-End Latency)에 영향을 준다. 벤치마크 정확도가 뛰어난 검출기라도 최악 조건 지연시간(Worst-Case Latency)이 제어 주기(Control Cycle)를 초과하거나 메모리 사용량이 위치 추정, 계획 및 다른 GPU 작업과 충돌한다면 실제 시스템에는 적합하지 않을 수 있다.

따라서 최종적인 아키텍처 설계는 다차원 최적화 문제(Multidimensional Optimization Problem)로 이해해야 한다. BEV는 주행 계획에 적합한 공간 추상화(Spatial Abstraction)를 제공하고, 필러는 계산 효율성에 중점을 두며, 복셀은 더욱 풍부한 3차원 기하 구조를 보존한다. 실제 자율주행 시스템에서는 희소한 센서 측정값을 점차 작업 지향적인 특징(Task-Oriented Feature)으로 변환하는 계층적 파이프라인(Hierarchical Pipeline) 안에서 이들을 함께 사용하는 경우가 많다. 최적의 구성은 센서 특성, 환경 복잡도, 검출 범위, 객체 크기, 지연시간 예산(Latency Budget), 하드웨어 처리 능력에 의해 결정된다.

실제 운용되는 실외 AMR에서 3차원 검출기는 궁극적으로 연속적인 인지-행동 파이프라인(Perception-to-Action Pipeline)의 일부로 다루어야 한다. 그 출력은 단순한 경계 상자들의 집합이 아니라 주변 행위자(Actor)와 장애물에 대한 구조화된 표현(Structured Representation)이며, 추적(Tracking), 예측(Prediction), 행동 계획(Behavior Planning), 궤적 생성(Trajectory Generation), 안전 모니터링(Safety Monitoring)의 입력으로 사용된다. 이러한 아키텍처 관점은 이후 다루게 될 포인트필러스(PointPillars), 센터포인트(CenterPoint), 카메라 기반 BEV(Camera-Based BEV), 다중 센서 융합(Multi-Sensor Fusion) 구현을 이해하기 위한 기반을 제공한다.

## 05.02. LiDAR 3D Object Detection PointPillars CenterPoint [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

LiDAR 기반 3차원 객체 검출(LiDAR-Based 3D Object Detection)은 희소한 3차원 측정값을 자율주행 소프트웨어와 실외 AMR에서 활용할 수 있는 구조화된 객체 정보로 변환한다. 객체 검출 및 추적(Object Detection and Tracking) 장에서 PointPillars와 CenterPoint는 LiDAR 관측값을 효율적인 공간 특징(Spatial Feature)과 3차원 검출 결과로 변환하는 대표적인 접근법이다. 두 아키텍처의 차이는 포인트 클라우드 표현(Point-Cloud Representation), 특징 추출(Feature Extraction), 객체 위치 추정(Object Localization), 예측 헤드(Prediction Head)를 실시간 인지(Real-Time Perception)를 위해 어떻게 구성할 수 있는지를 보여준다.

LiDAR 센서는 일반적으로 3차원 좌표와 반사 강도(Intensity), 리턴 정보(Return Information) 또는 기타 속성을 포함하는 포인트 클라우드(Point Cloud)를 생성한다. 영상과 달리 포인트들은 불규칙하게 분포하며 자연스럽게 조밀한 직사각형 텐서(Dense Rectangular Tensor)를 형성하지 않는다. 따라서 검출 네트워크는 이러한 희소 표현(Sparse Representation)을 신경망 처리에 적합한 특징(Feature)으로 변환하는 인코딩 단계(Encoding Stage)를 필요로 한다. PointPillars는 수평 위치에 따라 포인트를 그룹화하고 환경을 수직 필러(Vertical Pillar)로 표현함으로써 이러한 변환을 수행한다.

PointPillars에서는 검출 공간(Detection Space)을 지면 평면(Ground Plane)의 규칙적인 2차원 격자(2D Grid)로 나눈다. 동일한 격자 셀(Grid Cell)에 들어오는 모든 포인트는 하나의 필러(Pillar)에 할당되며, 높이 차원(Height Dimension)은 조밀한 복셀(Voxel) 시퀀스로 분할하지 않고 암묵적으로 유지된다. 포인트 수준 특징(Point-Level Feature)에는 원래 좌표(Original Coordinates), 필러 중심(Pillar Center)에 대한 오프셋(Offset), 포인트 평균 위치(Point Mean Location)에 대한 오프셋, 반사 강도 등이 포함될 수 있다. 이후 포인트 단위 네트워크(Point-Wise Network)가 이러한 측정값을 학습된 특징으로 변환하고 필러 내부에서 이를 집계한다.

생성된 필러 특징(Pillar Feature)은 BEV 좌표계(BEV Coordinate System)에 따라 의사 영상(Pseudo-Image) 형태로 배열할 수 있다. 이는 성숙한 2차원 합성곱 신경망(2D Convolutional Neural Network)을 특징 추출에 사용할 수 있다는 중요한 아키텍처적 장점이다. 따라서 PointPillars 파이프라인은 포인트 수준 인코딩(Point-Level Encoding)과 효율적인 2차원 공간 처리를 결합한다. 이 방식은 조밀한 3차원 합성곱(Dense 3D Convolution)의 계산 비용을 피하면서도 차량(Vehicle), 보행자(Pedestrian), 방호물(Barrier), 기타 객체를 검출하는 데 필요한 유용한 수평 기하 정보(Horizontal Geometry)를 보존한다.

백본(Backbone)은 의사 영상에 대해 점차 추상화되는 공간 특징(Spatial Feature)을 생성한다. 초기 계층은 국부적인 기하 패턴(Local Geometric Pattern)을 포착하고, 더 깊은 계층은 더 넓은 수용 영역(Receptive Field)과 더 많은 문맥 정보(Contextual Information)를 제공한다. 객체가 서로 다른 거리와 크기로 나타날 수 있기 때문에 다중 스케일 특징 추출(Multi-Scale Feature Extraction)이 중요하다. 가까운 차량은 많은 BEV 셀을 차지할 수 있는 반면 먼 보행자는 매우 작은 영역만 차지할 수 있다. 특징 피라미드(Feature Pyramid) 또는 다중 해상도 구조(Multi-Resolution Structure)는 서로 다른 공간 스케일의 정보를 결합할 수 있도록 한다.

검출 헤드(Detection Head)는 학습된 BEV 특징을 객체 예측(Object Prediction)으로 변환한다. 일반적인 출력에는 객체 분류(Object Classification), 중심 위치(Center Location), 크기(Dimensions), 방향(Orientation)이 포함된다. 구현에 따라 속도(Velocity)와 기타 객체 속성(Object Attribute)도 추정할 수 있다. 예측된 3차원 경계 상자(3D Bounding Box)는 자차 좌표계(Ego Coordinate Frame)로 표현할 수 있으며, 이를 통해 이후의 추적(Tracking), 예측(Prediction), 행동 계획(Behavior Planning), 충돌 회피(Collision Avoidance) 모듈이 객체와 로봇 또는 차량 사이의 공간적 관계를 직접 처리할 수 있다.

CenterPoint는 이와 다르면서도 상호 보완적인 설계 철학을 따른다. 사전에 정의된 앵커 박스(Anchor Box)에 주로 의존하기보다는 객체 중심(Object Center)을 중심으로 검출을 구성한다. 네트워크는 BEV 표현을 생성하고 공간 특징 맵(Spatial Feature Map)에서 객체 중심의 위치를 예측한다. 추가 회귀 분기(Regression Branch)는 높이(Height), 크기(Dimensions), 방향(Orientation), 속도(Velocity) 등의 속성을 추정한다. 이러한 중심 기반(Center-Based) 방식은 객체 위치를 직접적으로 표현하며 이후 추적 과정과 자연스럽게 연결될 수 있다.

중심 표현(Center Representation)은 객체의 중심이 데이터 연관(Data Association)과 운동 추론(Motion Reasoning)을 위한 안정적인 공간 기준점(Spatial Reference)을 제공하기 때문에 특히 유용하다. 후보 중심(Candidate Center)이 식별되면 네트워크는 해당 3차원 경계 상자의 매개변수(3D Bounding-Box Parameter)를 복원할 수 있다. 자율주행 시스템에서는 방향 추정(Orientation Estimation)이 중요하다. 크기와 위치가 비슷한 두 객체라도 방향이 다르면 실제 점유 영역(Occupied Region)이 크게 달라질 수 있기 때문이다. 따라서 정확한 중심 및 방향 추정은 충돌 검사(Collision Checking)와 궤적 계획(Trajectory Planning)에 직접 기여한다.

속도 예측(Velocity Prediction)은 CenterPoint에 시간적 인지(Temporal Perception)와의 추가적인 연결성을 제공한다. 객체 속도를 추정하는 검출기는 단일 프레임의 경계 상자보다 더 많은 정보를 제공할 수 있다. 추정된 운동 상태(Motion State)는 이후 프레임의 관측값과 결합하여 추적과 예측을 지원할 수 있다. 그러나 실제 시스템에서 속도 추정값은 타임스탬프 품질(Timestamp Quality), 자차 움직임 보상(Ego-Motion Compensation), 센서 잡음(Sensor Noise), 객체 가림(Object Occlusion)과 함께 해석되어야 한다. 따라서 검출과 추적은 독립적인 기능이라기보다 서로 협력하는 단계로 설계하는 것이 적절하다.

PointPillars와 CenterPoint는 시간적 처리(Temporal Processing)를 지원하는 출력 구조에서도 차이가 있다. PointPillars는 효율적인 3차원 검출 결과를 제공하며 이를 별도의 다중 객체 추적기(Multi-Object Tracker)로 전달할 수 있다. CenterPoint의 중심 기반 표현과 속도 추정은 추적 단계로 보다 직접적으로 연결될 수 있다. 따라서 전체 소프트웨어 아키텍처에서는 추론 속도(Inference Speed), 위치 정확도(Localization Quality), 객체 크기(Object Scale), 시간 정보(Temporal Information), 사용 가능한 계산 자원(Computational Resource) 사이의 필요한 균형에 따라 검출기를 선택할 수 있다.

BEV 격자 해상도(BEV Grid Resolution)는 두 접근법 모두에서 가장 중요한 구성 매개변수(Configuration Parameter) 중 하나이다. 더 작은 셀 크기(Cell Size)는 더 세밀한 공간 정보를 제공하여 작은 객체의 위치 추정을 향상시킬 수 있지만, 특징 텐서(Feature Tensor)의 크기를 증가시키고 계산 부하를 높인다. 더 큰 셀 크기는 메모리와 처리 요구량을 감소시키지만 보행자, 좁은 장애물, 원거리 객체에 중요한 기하학적 세부 정보를 통합해 버릴 수 있다. 따라서 선택된 해상도는 센서 특성과 운용 설계 영역(Operational Design Domain, ODD)에 연결하여 결정해야 한다.

검출 범위(Detection Range)는 또 다른 엔지니어링 절충 관계(Engineering Trade-Off)를 만든다. 실외 AMR은 주행 속도가 증가할수록 정지 거리가 길어지고 장애물이 운용상 중요한 위치에 도달하기 전에 이를 식별해야 하기 때문에 비교적 넓은 인지 영역(Perception Area)이 필요할 수 있다. BEV 범위를 확장하면서 높은 공간 해상도를 유지하면 계산량이 크게 증가할 수 있다. 따라서 실제 구현에서는 LiDAR 격자 크기(LiDAR Grid Dimension)를 결정하기 전에 차량 또는 로봇의 속도, 제동 능력(Braking Capability), 위치 정확도, 장애물 크기, 계획 요구사항을 기반으로 필요한 검출 거리를 설정해야 한다.

PointPillars의 장점은 임베디드 추론 효율성(Embedded Inference Efficiency)이 중요한 환경에서 특히 의미가 있다. 포인트 클라우드를 의사 영상으로 변환하면 기존의 2차원 합성곱 아키텍처를 이용하여 효율적으로 GPU 처리를 수행할 수 있다. CenterPoint는 객체 중심을 자연스럽게 표현하고 속도 추정을 통합할 수 있는 중심 기반 구조를 제공한다. 두 방식 중 하나를 보편적으로 우월한 방식으로 취급하기보다는 특정 센서 구성, 대상 클래스(Target Class), 요구 지연시간(Required Latency), 검출 범위, 이후 자율주행 아키텍처에 따라 평가해야 한다.

실외 AMR에서는 LiDAR 검출이 표준화된 도로 장면과 다른 조건에서도 동작해야 한다. 산업 야드(Industrial Yard), 항만(Port), 캠퍼스(Campus), 건설 현장(Construction Area), 오프로드 환경에는 불규칙한 지형, 특수 장비, 컨테이너(Container), 임시 구조물, 식생, 연석, 부분적으로 가려진 객체 등이 존재할 수 있다. 이러한 조건에서는 일반적인 도로 데이터셋에서 학습된 가정의 효과가 감소할 수 있다. 따라서 학습 및 검증 데이터는 객체 밀도(Object Density), 조명에 상대적으로 독립적인 LiDAR 기하 정보(LiDAR Geometry), 날씨, 노면 상태, 센서 장착 구성(Sensor Mounting Configuration) 등의 변화를 포함하여 실제 운용 환경을 대표해야 한다.

최종적인 제품용 파이프라인(Production Pipeline)에서는 신경망 정확도(Neural-Network Accuracy) 이상의 요소를 고려해야 한다. 포인트 클라우드 획득(Point-Cloud Acquisition), 타임스탬프 동기화(Timestamp Synchronization), 좌표 변환(Coordinate Transformation), 필터링(Filtering), 필러화(Pillarization), GPU 추론, 디코딩(Decoding), 비최대 억제(Non-Maximum Suppression) 또는 중심 디코딩(Center Decoding), 객체 추적, 미들웨어 전송(Middleware Transmission)이 모두 종단간 지연시간(End-to-End Latency)에 영향을 준다. 따라서 검출기는 인지 성능 지표뿐만 아니라 추론 시간(Inference Time), 메모리 사용량, 처리량(Throughput), 검출 범위, 위치 오차(Localization Error), 오탐률(False-Positive Rate), 시간적 안정성(Temporal Stability)과 같은 시스템 수준의 측정값으로 평가해야 한다.

PointPillars와 CenterPoint는 LiDAR 3차원 객체 검출의 발전을 이해하기 위한 유용한 기준 아키텍처(Reference Architecture)를 제공한다. PointPillars는 불규칙한 포인트 클라우드를 효율적인 필러 기반 BEV 특징으로 변환하는 방법을 보여주며, CenterPoint는 객체 중심을 검출과 시간적 추론(Temporal Reasoning)을 위한 간결한 표현으로 강조한다. 자율주행 AMR 소프트웨어 스택에서는 이러한 출력값을 추적, 속도 추정, 운동 예측, 행동 계획, 궤적 생성, 안전 기능으로 전달되는 구조화된 인지 상태(Structured Perception State)로 취급해야 한다. 이를 통해 LiDAR 검출과 이후에 정의된 객체 추적 및 센서 융합 모듈 사이의 실질적인 연결 관계를 구성할 수 있다.

## 05.03. Camera Based 3D Detection BEVFusion PETR [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

카메라 기반 3차원 객체 검출(Camera-Based 3D Object Detection)은 카메라 영상을 이용하여 3차원 객체를 추정하고, 자율주행 시스템(Autonomous Driving System)과 실외 AMR에서 활용할 수 있는 기하학적 정보를 제공한다. LiDAR와 달리 카메라는 주로 색상(Color), 질감(Texture), 에지(Edge), 의미적 외관(Semantic Appearance)을 원근 영상 좌표(Perspective Image Coordinate)에서 관측한다. 따라서 핵심적인 과제는 영상 기반 특징(Image-Based Feature)을 3차원 위치 추정에 적합한 공간 표현(Spatial Representation)으로 변환하는 것이다. 객체 검출 및 추적(Object Detection and Tracking) 장에서 BEVFusion과 PETR은 카메라 인지(Camera Perception)를 3차원 장면 이해(3D Scene Understanding) 및 BEV 기반 검출(BEV-Based Detection)과 연결하는 중요한 접근법이다.

일반적인 카메라는 하나 이상의 원근 영상(Perspective Image)을 생성하며, 영상에서 객체가 보이는 크기는 카메라와의 거리에 크게 영향을 받는다. 이러한 표현은 의미적 인식(Semantic Recognition)에 매우 유용하지만 직접적인 미터 단위 깊이(Metric Depth) 정보를 제공하지는 않는다. 따라서 3차원 검출 아키텍처(3D Detection Architecture)는 시각적 증거(Visual Evidence)로부터 깊이 또는 기하학적 관계(Geometric Relationship)를 추론해야 한다. 일반적으로 먼저 영상 특징(Image Feature)을 추출한 다음, 이를 변환하거나 리프트(Lift)하거나 쿼리(Query)하여 정보가 3차원 좌표계(3D Coordinate System)에서 표현될 수 있도록 한다.

첫 번째 단계는 일반적으로 카메라 보정(Camera Calibration)과 영상 전처리(Image Preprocessing)로 구성된다. 내부 파라미터(Intrinsic Parameter)는 초점거리(Focal Length), 주점(Principal Point), 렌즈 특성을 나타내며, 외부 파라미터(Extrinsic Parameter)는 카메라와 차량 또는 로봇 좌표계 사이의 변환(Transformation)을 정의한다. 정확한 보정(Calibration)은 매우 중요하다. 작은 기하학적 오차도 투영된 특징(Projected Feature)을 잘못된 공간 위치에 배치할 수 있기 때문이다. 영상 크기 조정(Resizing), 정규화(Normalization), 왜곡 보정(Distortion Correction), 동기화된 영상 획득(Synchronized Capture) 역시 중요하며, 시각적 표현의 품질이 이후 3차원 위치 추정에 직접적인 영향을 준다.

카메라 기반 검출기(Camera-Based Detector)는 일반적으로 2차원 영상 백본(2D Image Backbone)에서 시작한다. 합성곱 신경망(Convolutional Neural Network) 또는 비전 트랜스포머(Vision Transformer)는 RGB 영상으로부터 계층적 특징 맵(Hierarchical Feature Map)을 생성하며, 점차 추상화된 시각 정보를 포함한다. 초기 특징은 에지, 질감, 국부 패턴(Local Pattern)을 보존하고, 더 깊은 특징은 의미적 구조(Semantic Structure)와 객체 수준 문맥(Object-Level Context)을 포착한다. 다중 스케일 특징(Multi-Scale Feature)은 특히 중요하다. 가까운 객체는 영상에서 넓은 영역을 차지할 수 있지만, 원거리의 보행자, 차량, 장애물은 매우 적은 수의 픽셀만 차지할 수 있기 때문이다.

핵심적인 아키텍처 문제는 원근 영상 특징(Perspective Image Feature)을 공간 표현으로 변환하는 것이다. 원근 영상은 영상 좌표(Image Coordinate)로 구성되는 반면, 주행 계획(Planning)과 충돌 판단(Collision Reasoning)은 일반적으로 차량 중심 좌표(Ego-Centric Coordinate)에서 수행된다. BEV 표현은 자차(Ego Vehicle)를 기준으로 객체와 환경 특징을 표현함으로써 공통 공간 인터페이스(Common Spatial Interface)를 제공한다. 따라서 카메라에서 BEV로의 변환(Camera-to-BEV Transformation)은 원근 영상 공간과 자율주행 시스템이 요구하는 지면 중심 표현(Ground-Oriented Representation) 사이의 기하학적이고 학습 기반의 매핑으로 이해할 수 있다.

깊이 추정(Depth Estimation)은 이러한 변환의 핵심이다. 카메라는 LiDAR와 같은 방식으로 직접 거리를 측정하지 않기 때문에 네트워크는 단안(Monocular) 또는 다중 카메라(Multi-Camera) 시각 단서로부터 깊이를 추론해야 한다. 이러한 단서에는 원근(Perspective), 객체 크기(Object Scale), 질감 변화(Texture Gradient), 양안 관계(Binocular Relationship), 시간적 일관성(Temporal Consistency), 학습된 장면 사전정보(Scene Prior) 등이 포함될 수 있다. 모든 픽셀에 대해 깊이를 완벽하게 추정할 필요는 없지만, 체계적인 깊이 오차(Systematic Depth Error)는 영상 특징이 3차원 또는 BEV 공간으로 투영될 때 상당한 위치 오차를 발생시킬 수 있다.

다중 카메라 시스템(Multi-Camera System)은 단일 카메라의 일부 한계를 감소시킨다. 여러 카메라는 자율 플랫폼 주변을 둘러싸며 서로 겹치는 시야(Field of View)를 제공할 수 있다. 각 카메라는 자체 특징 표현을 생성하고, 이러한 특징은 공통 좌표계(Common Coordinate System)로 변환될 수 있다. 이를 통해 객체가 서로 다른 관측 각도에서도 지속적으로 표현될 수 있으며 사각 영역(Blind Region)을 줄일 수 있다. 실외 AMR의 경우 카메라 배치는 요구되는 검출 범위, 장착 높이(Mounting Height), 시야각(Field of View), 플랫폼 크기, 운용 환경을 함께 고려하여 설계해야 한다.

BEV 기반 카메라 검출(BEV-Based Camera Detection)은 명시적인 깊이 리프팅(Explicit Depth Lifting) 또는 학습된 특징 변환(Learned Feature Transformation)을 사용할 수 있다. 명시적인 방식에서는 영상 특징을 추정된 깊이 분포(Depth Distribution)와 연결하고, 이를 3차원 프러스텀(3D Frustum)으로 투영한 후 BEV 셀(BEV Cell)에 누적한다. 학습 기반 방식에서는 어텐션(Attention), 위치 인코딩(Positional Encoding) 또는 기타 기하학적 메커니즘을 통해 시각 특징과 공간 위치의 대응 관계를 네트워크가 학습한다. 두 방식 모두 근본적으로 원근 영상의 시각적 증거를 의미 있는 공간 관계를 갖는 표현으로 변환하는 동일한 문제를 해결한다.

BEVFusion은 이러한 개념을 확장하여 여러 센서 모달리티(Sensor Modality)를 결합할 수 있는 공통 BEV 공간(Common BEV Space)을 제공한다. 카메라 특징은 BEV로 변환할 수 있으며, LiDAR 특징 역시 호환 가능한 공간 표현으로 인코딩할 수 있다. 이후 검출 전에 이러한 특징을 융합할 수 있다. 이러한 구조는 카메라가 강력한 의미 및 외관 정보를 제공하고 LiDAR가 직접적인 기하 정보를 제공한다는 점에서 중요하다. 두 센서의 상호 보완적인 특성을 적절하게 결합하면 장면 이해 성능을 향상시킬 수 있다.

BEVFusion의 가치는 단순히 특징 텐서(Feature Tensor)를 연결하는 것에만 있지 않다. 효과적인 융합을 위해서는 서로 다른 센서의 관측값이 동일한 물리적 위치와 거의 동일한 시간에 대응해야 한다. 카메라 영상과 LiDAR 스캔은 서로 다른 획득 방식, 샘플링 속도, 지연시간(Latency), 좌표계를 가질 수 있다. 따라서 보정, 타임스탬프 정렬(Timestamp Alignment), 자차 움직임 보상(Ego-Motion Compensation), 공간 변환(Spatial Transformation)은 단순한 구현 세부사항이 아니라 인지 아키텍처의 일부가 된다.

여러 카메라에서 생성된 BEV 특징 맵(BEV Feature Map)은 서로 다른 관점의 정보를 공통 지면 중심 좌표계(Common Ground-Oriented Coordinate System)에서 표현할 수 있다. 여러 카메라에 의해 관측되는 영역에는 상호 보완적인 증거가 포함될 수 있으며, 하나의 카메라에서만 관측되는 영역은 해당 카메라의 개별 관측 정보를 유지한다. 융합 메커니즘(Fusion Mechanism)은 학습된 관련성(Learned Relevance)에 따라 이러한 특징을 결합할 수 있다. 어텐션 기반 아키텍처(Attention-Based Architecture)는 특정 객체나 장면 구조에 대해 더 강한 증거를 제공하는 공간 영역과 모달리티를 네트워크가 강조하도록 할 수 있다.

PETR은 위치 정보를 인식하는 표현(Position-Aware Representation)과 트랜스포머 기반 추론(Transformer-Based Reasoning)을 통해 카메라 기반 3차원 인지에 접근한다. 영상 특징을 순수한 2차원 정보로만 취급하는 대신 PETR은 3차원 위치 정보를 시각 특징 표현에 통합한다. 이후 네트워크는 영상 특징이 3차원 공간의 어느 위치에 대응할 수 있는지 추론할 수 있다. 이를 통해 원근 시각 관측과 3차원 객체 쿼리(3D Object Query) 및 공간적으로 구조화된 예측(Spatially Structured Prediction)을 연결할 수 있다.

위치 인코딩(Positional Encoding)은 특히 중요하다. 트랜스포머 어텐션(Transformer Attention)은 픽셀 위치의 물리적 의미를 본질적으로 알지 못하기 때문이다. 동일한 영상 좌표의 특징이라도 카메라 보정, 관측 기하(Observation Geometry), 추정 깊이에 따라 전혀 다른 실제 공간 위치에 대응할 수 있다. 공간 정보를 표현에 임베딩(Embedding)하면 네트워크가 시각적 증거를 3차원 좌표 체계와 연결할 수 있다. 이를 통해 검출 과정에서 객체 위치, 방향, 공간 관계를 보다 직접적으로 추론할 수 있다.

트랜스포머 기반 검출(Transformer-Based Detection)은 객체 특징을 수집하는 방식도 변화시킨다. 단순히 조밀한 합성곱 처리(Dense Convolutional Processing)에만 의존하는 대신 객체 쿼리(Object Query)가 관련 영상 특징에 어텐션을 적용하여 특정 객체를 추정하는 데 필요한 정보를 집계할 수 있다. 이는 객체가 부분적으로 가려졌거나 외관 정보가 영상의 여러 영역에 분산되어 있을 때 유용하다. 어텐션은 문맥 정보를 결합하면서도 시각적 증거와 대상 3차원 예측 사이의 연결을 유지할 수 있다.

최종 검출 헤드(Final Detection Head)는 객체 클래스, 3차원 중심(3D Center), 크기, 방향, 신뢰도 등을 추정한다. 아키텍처에 따라 속도와 같은 추가 속성도 예측할 수 있다. 생성된 3차원 경계 상자(3D Bounding Box)는 명확하게 정의된 좌표계로 표현되어야 하며, 이를 통해 이후의 추적 및 계획 모듈이 일관되게 사용할 수 있다. 따라서 좌표 변환의 품질은 신경망 자체의 예측 품질만큼 중요하다.

카메라 기반 3차원 검출은 LiDAR 기반 검출과 다른 오류 특성(Error Profile)을 가진다. 시각적 인식은 깊이가 불확실한 경우에도 강하게 유지될 수 있지만, 원거리 또는 시각적으로 모호한 객체에서는 미터 단위 위치 추정 성능이 저하될 수 있다. 가는 객체(Thin Object), 질감이 없는 표면(Textureless Surface), 강한 반사(Strong Reflection), 반복 패턴(Repeated Pattern), 심한 가림(Occlusion)은 깊이 추론을 어렵게 만들 수 있다. 따라서 신뢰도(Confidence)는 절대적인 기하학적 정확도 척도로 취급하기보다 객체 거리, 영상 품질, 관측 각도, 시간적 일관성과 함께 해석해야 한다.

환경 조건(Environmental Condition)은 추가적인 어려움을 만든다. 카메라 인지는 조명, 그림자, 눈부심(Glare), 비, 안개, 눈, 오염된 렌즈, 야간 운용, 급격한 노출 변화(Exposure Change)의 영향을 받을 수 있다. 실외 AMR은 인공 조명이 크게 달라지는 환경과 다양한 표면 외관을 가진 장소에서도 운용될 수 있다. 따라서 강건한 시스템은 악천후, 계절 변화, 비정형 객체, 부분 가림, 카메라 오염을 포함하여 실제 운용 설계 영역(Operational Design Domain)을 대표하는 학습 및 검증 데이터를 필요로 한다.

실외 AMR에서는 카메라 기반 검출을 지형 및 주행 가능 영역 추정(Terrain and Drivable-Area Estimation)과 함께 고려해야 한다. 앞선 구조에는 BEV 도로 분할(BEV Road Segmentation), 다중 센서 주행 가능 영역 융합(Multi-Sensor Drivable-Area Fusion), 점유 격자 구축(Occupancy-Grid Construction), 지형 주행 가능성 분석(Terrain Traversability Analysis), 음의 장애물 검출(Negative-Obstacle Detection)이 포함되어 있다. 카메라에서 생성된 BEV 특징은 이러한 표현에 의미 정보를 제공할 수 있으며, 기하 센서는 거리와 지면 구조에 대한 상호 보완적인 정보를 제공할 수 있다.

BEVFusion은 이미 LiDAR와 카메라 센서를 모두 사용하는 시스템에서 특히 중요하다. 카메라 특징은 객체 클래스, 시각적 외관, 차선 또는 도로 의미 정보, 문맥 정보를 제공할 수 있으며, LiDAR 특징은 정확한 공간 기하 정보를 제공한다. 센서 융합을 통해 검출기는 어느 하나의 센서만으로 생성할 수 있는 것보다 풍부한 표현을 사용할 수 있다. 그러나 이러한 아키텍처는 보정 오차, 센서 고장, 계산 부하, 동기화 문제에도 더욱 민감해진다.

따라서 센서 고장(Sensor Failure)을 아키텍처에서 고려해야 한다. 카메라가 오염, 통신 오류, 노출 문제 또는 하드웨어 고장으로 사용할 수 없게 되는 경우 인지 시스템은 정의된 성능 저하 운용 모드(Degraded Operating Mode)를 가져야 한다. 마찬가지로 LiDAR 고장이 발생하더라도 충분한 시각 정보가 남아 있다면 시스템이 즉시 통제되지 않는 행동을 수행해서는 안 된다. 따라서 센서 융합 아키텍처는 정상적인 융합(Normal Fusion), 감소된 모달리티 운용(Reduced-Modality Operation), 신뢰도 저하(Confidence Degradation), 안전 기반 대체 동작(Safety-Triggered Fallback Behavior)을 구분해야 한다.

실시간 성능(Real-Time Performance) 역시 중요한 고려사항이다. 카메라 기반 3차원 검출은 여러 개의 고해상도 영상, 대규모 비전 백본, 깊이 추정, 트랜스포머 어텐션, BEV 변환, 다중 카메라 융합을 필요로 할 수 있다. 이러한 연산은 상당한 GPU 메모리와 계산 대역폭(Computational Bandwidth)을 사용할 수 있다. 따라서 임베디드 AMR에서는 벤치마크 검출 정확도만이 아니라 종단간 지연시간, 영상 해상도, 카메라 수, 프레임률(Frame Rate), 메모리 사용량, GPU 사용률, 전력 소비를 기준으로 모델을 평가해야 한다.

BEV 표현의 공간 해상도(Spatial Resolution) 역시 정확도와 효율성 사이의 균형을 결정한다. 미세한 BEV 셀은 작은 객체에 대한 세부 정보를 보존하고 위치 추정 정확도를 향상시킬 수 있지만 특징 맵의 크기와 계산량을 증가시킨다. 거친 셀은 처리 요구량을 줄이지만 보행자, 좁은 장애물, 연석, 원거리 객체에 대한 정보를 잃을 수 있다. 실제 시스템에서는 객체 크기, 검출 범위, 플랫폼 속도, 계획 요구사항, 사용 가능한 엣지 컴퓨팅 자원에 따라 BEV 해상도를 결정해야 한다.

카메라 기반 3차원 검출은 시간 정보(Temporal Information)를 활용하여 더욱 향상될 수 있다. 연속된 프레임은 약간씩 다른 관점에서 동일한 객체를 여러 번 관측할 수 있게 한다. 자차 움직임과 타임스탬프를 올바르게 처리한다면 시간적 융합(Temporal Fusion)을 통해 깊이 추정, 객체 안정성, 속도 추론을 향상시킬 수 있다. 그러나 움직이는 객체, 카메라 움직임, 동기화 오류를 무시하면 시간적 누적에서 오류가 발생할 수도 있다. 따라서 시간 처리는 단순히 영상 특징을 누적하기보다 움직임 보상과 추적 과정에 통합해야 한다.

검출과 추적의 관계는 자율주행 운용에서 특히 중요하다. 카메라 기반 검출기는 순간적인 3차원 객체 가설(3D Object Hypothesis)을 생성하는 반면, 추적기는 객체 식별자를 유지하고 시간적 상태를 추정한다. 안정적인 검출 중심, 방향, 크기, 신뢰도는 데이터 연관(Data Association)의 품질을 향상시킨다. 반대로 추적 정보는 불확실한 검출을 안정화하고 객체가 부분적으로 가려졌을 때 시간적 문맥을 제공할 수 있다. 따라서 검출기와 추적기는 결합된 인지 서브시스템(Perception Subsystem)으로 평가하는 것이 적절하다.

모델 학습 및 검증은 의도된 배포 영역(Intended Deployment Domain)을 반영해야 한다. 범용 자율주행 데이터셋(General-Purpose Autonomous-Driving Dataset)은 유용한 사례를 제공하지만, 실외 AMR은 항만, 캠퍼스, 산업 야드, 건설 현장, 물류 시설, 오프로드 환경 등에서 운용될 수 있으며 이러한 환경에는 기존 데이터셋에 충분히 표현되지 않은 객체 종류와 공간 구성이 존재할 수 있다. 따라서 도메인 특화 데이터(Domain-Specific Data), 합성 증강(Synthetic Augmentation), 악조건 데이터(Adverse-Condition Sample), 롱테일 시나리오(Long-Tail Scenario)는 학습 파이프라인의 중요한 구성요소가 될 수 있다.

평가는 일반적인 검출 지표와 시스템 수준 측정값을 모두 포함해야 한다. 평균 정밀도(Average Precision)는 검출 품질을 측정할 수 있으며, 3차원 위치 오차(3D Localization Error), 방향 오차(Orientation Error), 거리별 성능(Range-Dependent Performance), 소형 객체 재현율(Small-Object Recall), 오탐률(False-Positive Rate), 시간적 안정성(Temporal Stability)은 추가 정보를 제공한다. 실제 AMR에서는 인지 지연시간, 최악 조건 처리시간(Worst-Case Processing Time), 자원 사용률(Resource Utilization), 센서 동기화 오차(Sensor Synchronization Error), 성능 저하 모드의 동작도 측정해야 한다. 이러한 요소들이 계획과 안전에 직접적인 영향을 주기 때문이다.

BEVFusion과 PETR은 카메라 기반 3차원 검출의 서로 보완적인 두 방향을 보여준다. BEVFusion은 카메라와 다른 센서 정보를 통합할 수 있는 공통 BEV 표현을 구축하는 데 중점을 두며, PETR은 카메라 특징에 대한 3차원 위치 정보와 트랜스포머 기반 추론을 강조한다. 두 접근법 모두 원근 시각 관측을 공간적으로 의미 있는 3차원 표현으로 변환한다는 근본적인 문제를 해결한다.

전체 자율 AMR 소프트웨어 아키텍처에서 카메라 기반 3차원 검출은 독립적인 영상 분류(Image Classification) 작업이 아니라 공간 인지 계층(Spatial Perception Layer)으로 보아야 한다. 카메라 영상은 보정, 특징 추출, 깊이 또는 위치 추론, BEV 투영, 검출 과정을 거쳐 구조화된 3차원 객체 상태(Structured 3D Object State)로 변환된다. 이러한 상태는 LiDAR 및 레이더 정보와 결합될 수 있으며, 다중 객체 추적과 속도 추정으로 전달된 후 궁극적으로 운동 예측, 행동 계획, 궤적 생성, 안전 기능에서 사용된다. 이는 이 절의 카메라 기반 방법을 객체 검출 및 추적 장에서 다음에 정의된 다중 센서 융합 및 추적 모듈과 직접 연결한다.

## 05.04. Multi Sensor Fusion Detection LiDAR Camera Radar [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

다중 센서 융합 검출(Multi-Sensor Fusion Detection)은 LiDAR, 카메라(Camera), 레이더(Radar)의 관측 정보를 결합하여 주변 환경에 대한 보다 완전한 표현을 구성한다. 각 센서는 서로 다른 정보를 제공하고 서로 다른 측정 특성을 가지므로, 센서 융합(Fusion)은 하나의 센서에 의존하기보다 각 센서의 상호 보완적인 장점을 활용하는 것을 목표로 한다. 객체 검출 및 추적(Object Detection and Tracking) 장에서 이 주제는 LiDAR 기반 및 카메라 기반 3차원 검출 이후에 배치되며, 다중 객체 추적(Multi-Object Tracking)과 운동 예측(Motion Prediction)으로 연결되는 기반을 구성한다.

LiDAR는 3차원 공간에서 정확한 기하학적 측정값을 제공하며 신뢰성 높은 거리 및 객체 형상 정보를 생성할 수 있다. 카메라 센서는 객체의 외관, 색상, 질감, 장면의 문맥 정보 등을 포함하는 풍부한 시각 및 의미 정보를 제공한다. 레이더는 거리와 상대 운동(Relative Motion)에 관련된 측정값을 제공하며, 시각 센서의 성능이 저하되는 환경에서도 유용할 수 있다. 따라서 이러한 모달리티를 결합하면 기하학적 정보와 의미 정보를 모두 포함하는 인지 결과를 구성할 수 있다.

효과적인 융합을 위한 첫 번째 요구사항은 공통 좌표 체계(Common Coordinate Framework)이다. LiDAR, 카메라, 레이더는 일반적으로 서로 다른 물리적 위치와 방향으로 장착되므로 처음부터 동일한 좌표계를 공유하지 않는다. 외부 보정(Extrinsic Calibration)은 센서 사이의 공간 변환(Spatial Transformation)을 정의하고, 내부 보정(Intrinsic Calibration)은 각 센서의 고유한 파라미터를 정의한다. 정확한 보정은 매우 중요하다. 작은 공간 변환 오차도 서로 다른 센서의 측정값이 잘못된 실제 객체에 연결되도록 만들 수 있기 때문이다.

시간 동기화(Temporal Synchronization) 역시 동일하게 중요하다. LiDAR 스캔, 카메라 프레임, 레이더 측정값은 정확히 동일한 순간에 획득되지 않을 수 있다. 따라서 움직이는 객체는 각각의 센서 관측에서 서로 다른 위치에 나타날 수 있다. 융합 소프트웨어는 관측값을 정렬할 때 타임스탬프(Timestamp), 센서 지연시간(Sensor Latency), 획득 주기(Acquisition Period), 자차 운동(Ego-Motion)을 고려해야 한다. 하드웨어 타임스탬프(Hardware Timestamping)와 결정론적 동기화(Deterministic Synchronization)는 이 과정을 향상시킬 수 있으며, 소프트웨어 보간(Software Interpolation)이나 운동 보상(Motion Compensation)은 잔여 시간 정렬 오차를 줄일 수 있다.

센서 전처리(Sensor Preprocessing)는 일반적으로 융합 이전에 수행되어야 한다. LiDAR 데이터는 필터링(Filtering), 지면 제거(Ground Removal), 좌표 변환(Coordinate Transformation), 포인트 클라우드 특징 추출(Point-Cloud Feature Extraction)이 필요할 수 있다. 카메라 데이터는 영상 크기 조정(Resizing), 정규화(Normalization), 왜곡 보정(Distortion Correction), 시각 특징 추출(Visual Feature Extraction)이 필요할 수 있다. 레이더 측정값은 검출 필터링(Detection Filtering), 클러터 억제(Clutter Suppression), 좌표 변환, 속도 관련 처리(Velocity-Related Processing)가 필요할 수 있다. 목표는 서로 다른 원시 측정값을 효율적으로 연관시키고 결합할 수 있는 표현으로 변환하는 것이다.

융합은 인지 파이프라인(Perception Pipeline)의 서로 다른 수준에서 구성할 수 있다. 초기 융합(Early Fusion)은 비교적 낮은 수준의 측정값이나 특징을 상당한 모달리티별 처리 이전에 결합한다. 중간 융합(Intermediate Fusion)은 먼저 각 센서에서 독립적으로 특징을 추출한 후 공통 표현에서 특징을 결합한다. 후기 융합(Late Fusion)은 센서별로 독립적인 검출을 수행한 후 생성된 객체 가설(Object Hypothesis)을 결합한다. 적절한 융합 수준은 계산 자원, 보정 품질, 센서 특성, 센서 간 상호작용에 요구되는 정도에 따라 결정된다.

공통 중간 표현(Common Intermediate Representation)으로는 BEV(Bird\'s-Eye View)가 널리 사용될 수 있다. BEV는 여러 센서의 정보를 수용할 수 있는 지면 중심 좌표계(Ground-Oriented Coordinate System)를 제공하기 때문이다. LiDAR 특징은 BEV로 투영하거나 인코딩할 수 있으며, 카메라 특징은 원근 영상(Perspective Image)에서 BEV로 변환할 수 있고, 레이더 관측값도 동일한 공간 좌표계에 매핑할 수 있다. 표현이 정렬되면 융합 네트워크(Fusion Network)는 기하학적 증거, 시각적 증거, 운동 관련 증거 사이의 관계를 학습할 수 있다.

LiDAR-카메라 융합(LiDAR-Camera Fusion)은 두 모달리티가 상호 보완적인 특성을 가지기 때문에 특히 유용하다. LiDAR는 정확한 미터 단위 기하 정보(Metric Geometry)를 제공할 수 있지만 의미적 외관 정보는 상대적으로 제한적이다. 카메라는 강력한 의미 인식과 상세한 시각적 문맥을 제공하지만 깊이를 추론해야 한다. 두 센서의 특징을 정확하게 정렬하면 시각 정보가 LiDAR 기반 구조의 분류 또는 해석을 지원할 수 있으며, LiDAR 기하 정보는 카메라 관측의 공간적 해석을 제한하고 보완할 수 있다.

레이더는 다른 유형의 정보를 제공한다. 레이더 측정값은 거리와 상대 속도(Relative Velocity) 정보를 제공할 수 있으며, 시각적 외관이 모호한 경우에도 움직이는 객체를 검출할 수 있다. 따라서 레이더 관측은 동적 객체 인지(Dynamic-Object Perception)에서 카메라와 LiDAR를 보완할 수 있다. 그러나 레이더 리턴(Radar Return)은 희소하고 잡음이 많을 수 있으며 다중경로(Multipath) 또는 반사(Reflection)의 영향을 받을 수 있다. 따라서 융합 과정에서는 모든 레이더 리턴을 정확한 객체 관측값으로 취급하기보다 레이더의 신뢰도(Confidence)와 측정 불확실성(Measurement Uncertainty)을 표현해야 한다.

객체 연관(Object Association)은 다중 센서 융합의 핵심 문제이다. 서로 다른 센서의 측정값이 동일한 실제 객체에 해당하는지를 판단해야 한다. 공간적 근접성(Spatial Proximity)은 초기 연관 단서를 제공할 수 있지만, 객체들이 서로 가까이 있을 경우 위치 정보만으로는 충분하지 않을 수 있다. 객체 클래스, 크기, 방향, 속도, 타임스탬프, 신뢰도 등의 추가 정보를 사용하면 연관 성능을 향상시킬 수 있다. 또한 모든 센서가 모든 객체를 관측하는 것은 아니므로 융합 시스템은 매칭되지 않은 관측값(Unmatched Observation)도 표현할 수 있어야 한다.

신뢰도 관리(Confidence Management)는 운용 조건에 따라 센서의 신뢰성이 변하기 때문에 중요하다. 카메라의 신뢰도는 어두운 환경, 눈부심, 안개, 비, 렌즈 오염 등으로 감소할 수 있다. LiDAR 성능은 거리, 반사율(Reflectivity), 가림, 날씨, 센서 오염 등의 영향을 받을 수 있다. 레이더는 반사와 클러터로 인해 불확실하거나 모호한 리턴을 생성할 수 있다. 따라서 융합 시스템은 모든 모달리티가 항상 동일한 수준의 정확도를 가진다고 가정하기보다 각 센서의 신뢰성에 따라 측정값을 결합해야 한다.

다중 센서 융합은 특징 연결(Feature Concatenation), 학습된 가중치(Learned Weighting), 어텐션 메커니즘(Attention Mechanism), 확률적 결합(Probabilistic Combination), 객체 수준 가설 융합(Object-Level Hypothesis Fusion) 등을 이용하여 구현할 수 있다. 신경망은 센서 특징 사이의 관계를 학습할 수 있으며, 확률적 방법은 불확실성을 명시적으로 표현할 수 있다. 하이브리드 아키텍처(Hybrid Architecture)는 학습 기반 인지와 규칙 기반 일관성 검사(Rule-Based Consistency Check)를 결합할 수도 있다. 선택된 방법은 정확도, 계산 비용, 해석 가능성 요구사항, 안전 요구사항을 기준으로 평가해야 한다.

카메라 기반 BEV 시스템에서 다중 센서 융합은 시각적 깊이 추정(Visual Depth Estimation)과 관련된 불확실성을 줄이는 방법을 제공할 수 있다. 카메라 특징은 의미 정보를 제공하고, LiDAR 또는 레이더 측정값은 추가적인 기하학적 제약(Geometric Constraint)을 제공할 수 있다. 반대로 카메라 정보는 희소한 기하 측정값을 해석하는 데 도움을 줄 수 있다. 융합 아키텍처는 모든 센서 정보를 지나치게 이른 단계에서 동일한 표현으로 강제하기보다 각 모달리티의 장점을 보존해야 한다.

공통 표현의 공간 해상도(Spatial Resolution)는 융합 품질에 영향을 준다. 거친 BEV 격자(Coarse BEV Grid)는 메모리 소비와 계산량을 줄이지만 서로 가까운 객체의 측정값이 구분되지 않을 수 있다. 미세한 격자(Fine Grid)는 더 많은 공간 세부 정보를 보존하지만 처리 요구량을 증가시킨다. 따라서 해상도는 센서 정확도, 객체 크기, 검출 범위, 플랫폼 속도, 대상 시스템의 계산 능력을 고려하여 결정해야 한다.

모든 센서 모달리티에서 검출 범위(Detection Range) 역시 고려해야 한다. LiDAR, 카메라, 레이더는 서로 다른 유효 검출 범위와 장거리에서 서로 다른 검출 특성을 가질 수 있다. 융합 시스템은 인지 영역 전체에서 모든 센서가 동일한 정보를 제공한다고 가정해서는 안 된다. 대신 각 공간 영역에서 가장 강한 증거를 활용하고 관측값이 희소하거나 신뢰성이 낮아지는 영역에서는 불확실성을 표현할 수 있어야 한다.

실외 AMR은 구조화된 도로 장면과 상당히 다른 환경에서 운용될 수 있기 때문에 추가적인 융합 요구사항을 만든다. 산업 야드(Industrial Yard), 항만(Port), 캠퍼스(Campus), 건설 현장(Construction Site), 오프로드 영역(Off-Road Area)에는 불규칙한 지형, 임시 구조물, 컨테이너, 기계 장비, 식생, 보행자, 비정형 차량 등이 존재할 수 있다. 따라서 다중 센서 검출기는 실제 운용 환경을 대상으로 학습 및 검증되어야 한다. 또한 센서 장착 높이와 방향도 중요하다. 센서가 관측하는 기하 구조는 로봇 플랫폼의 구성에 직접적으로 영향을 받기 때문이다.

융합은 인지 스택의 나머지 부분과 연속적으로 동작해야 한다. 앞선 구조에는 주행 가능 영역 추정(Drivable-Area Estimation), 점유 격자 구축(Occupancy-Grid Construction), 지형 주행 가능성 분석(Terrain Traversability Analysis), 음의 장애물 검출(Negative-Obstacle Detection)이 포함되며, 이후 구조에는 다중 객체 추적과 속도 추정(Velocity Estimation)이 포함된다. 따라서 융합된 검출 결과는 정적 환경 구조와 동적 장애물을 동시에 이해하기 위해 사용되는 여러 상호 보완적 표현 중 하나가 될 수 있다.

시간적 융합(Temporal Fusion)은 강건성을 향상시키는 또 다른 방법이다. 연속된 센서 관측값을 누적하거나 서로 연관시켜 더욱 완전한 객체 정보를 제공할 수 있다. 그러나 이동 객체와 자차 운동은 측정값을 결합하기 전에 보상해야 한다. 그렇지 않으면 타임스탬프 오류로 인해 객체가 중복되거나 위치가 이동하거나 흐려진 표현이 생성될 수 있다. 따라서 시간적 융합은 단순한 누적 방식으로 처리하기보다 운동 보상과 추적에 통합해야 한다.

센서 중복성(Sensor Redundancy) 역시 운용 강건성(Operational Robustness)에 중요하다. 하나의 모달리티를 사용할 수 없게 되었을 때 인지 아키텍처는 정의된 성능 저하 모드(Degraded Mode)를 가져야 한다. 예를 들어 카메라가 고장 나더라도 LiDAR와 레이더가 정상적으로 동작한다면 모든 인지 기능이 중단되어서는 안 되며, 반대로 LiDAR가 고장 나더라도 시스템은 남아 있는 카메라와 레이더 정보를 각 센서의 한계 내에서 활용할 수 있어야 한다. 이러한 성능 저하 운용에는 융합이 자동으로 모든 센서 고장을 보상한다고 가정하기보다 명시적인 신뢰도 관리와 안전 정책(Safety Policy)이 필요하다.

계산 아키텍처(Computational Architecture)는 모든 센서 모달리티의 결합된 작업량을 지원해야 한다. 여러 카메라는 상당한 영상 처리 자원을 요구할 수 있고, LiDAR는 포인트 클라우드 처리를 필요로 하며, 레이더는 자체 신호 처리 파이프라인(Signal-Processing Pipeline)을 필요로 한다. 이후 특징 추출과 융합 과정에서 GPU 메모리 사용량과 통신 대역폭(Communication Bandwidth)이 증가할 수 있다. 따라서 실시간 배포에서는 전체 파이프라인 지연시간, 처리량(Throughput), 메모리 사용량, GPU 사용률, 센서 전송 오버헤드(Sensor-Transfer Overhead), 최악 조건 처리시간(Worst-Case Processing Time)을 측정해야 한다.

융합 품질(Fusion Quality)은 센서 수준과 시스템 수준에서 모두 평가해야 한다. 정밀도(Precision), 재현율(Recall), 평균 정밀도(Average Precision)와 같은 일반적인 검출 지표는 객체 검출 성능을 측정할 수 있다. 추가적으로 3차원 위치 오차(3D Localization Error), 방향 정확도(Orientation Accuracy), 속도 추정, 거리별 성능(Range-Dependent Performance), 오탐(False Positive), 미검출(Missed Detection), 시간적 안정성(Temporal Stability)을 평가해야 한다. 시스템 수준의 시험에서는 센서 성능 저하, 비동기 측정(Asynchronous Measurement), 보정 오차, 악조건 환경에서의 동작도 평가해야 한다.

실용적인 융합 아키텍처는 중요한 정보가 어느 센서에서 비롯되었는지에 대한 추적성(Traceability)을 유지해야 한다. 예를 들어 검출된 객체의 경우 위치는 LiDAR 기하 정보에 의해 강하게 뒷받침되고, 클래스는 주로 카메라 정보에 의해 결정되며, 속도는 레이더에 의해 강하게 뒷받침될 수 있다. 이러한 정보의 출처(Provenance)는 진단(Diagnostics), 신뢰도 추정, 고장 분석(Failure Analysis), 안전 모니터링(Safety Monitoring)을 지원할 수 있다. 또한 엔지니어가 융합 오류가 센싱, 보정, 연관, 학습된 검출 모델 중 어디에서 발생했는지를 파악하는 데 도움을 준다.

자율주행 AMR에서 최종 융합 검출 결과는 위치, 크기, 방향, 클래스, 가능한 경우 속도, 신뢰도, 타임스탬프, 좌표계, 그리고 잠재적으로 센서 출처를 포함하는 구조화된 객체 상태(Structured Object State)로 표현해야 한다. 이 상태는 다중 객체 추적기로 전달되어 시간에 따른 지속적인 객체 식별자(Persistent Object Identity)를 유지할 수 있다. 이후 추적은 보다 안정적인 속도와 운동 상태를 제공하여 다음 단계의 예측과 계획을 지원할 수 있다.

다중 센서 융합 검출은 개별 센서 인지와 통합된 자율행동 사이를 연결하는 아키텍처적 가교(Architectural Bridge)를 구성한다. LiDAR는 3차원 기하 정보를 제공하고, 카메라는 의미 및 시각적 문맥을 제공하며, 레이더는 거리와 운동 관련 정보를 제공한다. 이러한 정보를 결합하려면 정확한 보정, 시간 정렬, 표현 변환, 데이터 연관, 불확실성 관리, 실시간 계산이 필요하다. 전체 객체 검출 및 추적 아키텍처에서 이러한 기능은 이후의 추적, 속도 추정, 운동 예측 단계로 전달될 일관된 다중 모달 인지 상태(Multi-Modal Perception State)를 구성한다.

## 05.05. Multi Object Tracking MOT Kalman ByteTrack [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

다중 객체 추적(Multi-Object Tracking, MOT)은 연속적인 인지 프레임(Perception Frame)에 걸쳐 여러 객체의 식별자(Identity)와 시간적 상태(Temporal State)를 유지한다. 객체 검출(Object Detection)이 각 프레임에서 객체를 독립적으로 추정하는 반면, 추적(Tracking)은 어떤 검출 결과가 시간에 따라 동일한 실제 객체에 해당하는지를 결정한다. 객체 검출 및 추적(Object Detection and Tracking) 장에서 MOT는 LiDAR, 카메라, 다중 센서 검출 이후에 배치되며 속도 추정(Velocity Estimation)과 운동 예측(Motion Prediction)을 위한 시간적 기반을 제공한다.

추적 시스템(Tracking System)은 일반적으로 위치(Position), 크기(Dimensions), 방향(Orientation), 클래스(Class), 신뢰도(Confidence), 타임스탬프(Timestamp) 정보를 포함하는 검출 객체 시퀀스(Sequence of Detected Objects)를 입력으로 받는다. 추적기는 현재 검출 결과를 기존에 생성된 추적 상태(Track)와 연관시키고 해당 상태를 갱신한다. 객체가 가림(Occlusion)이나 센서의 한계로 인해 일시적으로 보이지 않는 경우, 추적기는 예측된 상태(Predicted State)를 사용하여 제한된 시간 동안 해당 추적 상태를 유지할 수 있다. 이러한 시간적 연속성(Temporal Continuity)은 자율 시스템에서 중요하다. 계획(Planning)은 프레임마다 독립적으로 생성되는 검출 결과보다 안정적인 객체 정보를 필요로 하기 때문이다.

MOT의 핵심 문제는 데이터 연관(Data Association)이다. 매 프레임마다 추적기는 현재 검출 결과 중 어떤 것이 기존 추적 상태에 해당하는지를 결정해야 한다. 공간적 거리(Spatial Distance)는 중요한 연관 단서이지만 여러 객체가 서로 가까이 있을 때는 충분하지 않다. 객체 클래스, 경계 상자 크기(Bounding-Box Dimensions), 방향, 속도(Velocity), 검출 신뢰도, 외관 특징(Appearance Feature)과 같은 추가 정보를 사용하면 연관 성능을 향상시킬 수 있다. 강건한 추적기는 매칭되지 않은 검출(Unmatched Detection)과 매칭되지 않은 추적 상태(Unmatched Track)도 처리해야 한다. 객체가 센싱 영역에 새롭게 진입하거나 영역을 벗어날 수 있기 때문이다.

칼만 필터(Kalman Filter)는 잡음이 포함된 측정값으로부터 객체의 숨겨진 상태(Hidden State)를 추정하기 위한 고전적인 프레임워크를 제공한다. 상태 벡터(State Vector)는 위치와 속도를 포함할 수 있으며, 더욱 발전된 모델에서는 가속도(Acceleration) 또는 기타 운동 변수를 추가로 표현할 수 있다. 필터는 운동 모델(Motion Model)을 사용하여 다음 상태를 예측하고, 새로운 측정값이 들어오면 해당 예측을 보정한다. 이러한 예측(Prediction)과 보정(Correction)의 분리는 칼만 필터를 잡음이 많고 간헐적인 관측값을 처리해야 하는 실시간 추적 시스템에 적합하게 만든다.

일반적인 추적 주기(Tracking Cycle)에서는 칼만 필터가 먼저 현재 타임스탬프에서 모든 활성 추적 상태(Active Track)의 예상 상태를 예측한다. 이후 검출기가 새로운 측정값을 제공하고, 연관 알고리즘(Association Algorithm)이 예측값과 검출값 사이의 대응 관계를 계산한다. 매칭된 검출값은 해당 필터 상태를 갱신하는 데 사용된다. 측정값이 없는 추적 상태는 일정 기간 동안 예측만을 사용하여 임시로 유지할 수 있으며, 새롭게 관측된 객체는 새로운 추적 상태로 초기화할 수 있다. 일정 시간 이상 매칭되지 않은 추적 상태는 최종적으로 종료된다.

운동 모델의 품질(Motion Model Quality)은 추적 성능에 큰 영향을 준다. 일정 속도 모델(Constant-Velocity Model)은 짧은 시간 동안 객체의 속도가 대략적으로 일정하다고 가정하는 반면, 일정 가속도 모델(Constant-Acceleration Model)은 변화하는 운동을 보다 명시적으로 표현할 수 있다. 차량과 보행자의 움직임은 선회, 제동, 가속, 갑작스러운 회피 동작 중 이러한 가정을 위반할 수 있다. 따라서 추적기는 가정된 모델과 실제 객체 운동 사이의 차이에 적응할 수 있도록 적절한 프로세스 잡음(Process Noise) 파라미터를 사용해야 한다.

ByteTrack은 낮은 신뢰도의 검출 결과도 중요하게 활용하는 실용적인 연관 전략을 사용한다. 기존의 추적 파이프라인에서는 연관 과정 이전에 특정 신뢰도 임계값(Confidence Threshold)보다 낮은 검출 결과를 제거할 수 있다. 그러나 이러한 방식은 부분적으로 가려진 객체의 약한 관측값을 제거하여 기존 추적 상태가 사라지는 문제를 발생시킬 수 있다. ByteTrack은 먼저 높은 신뢰도의 검출 결과를 연관시킨 다음 낮은 신뢰도의 검출 결과를 사용하여 추가적인 연관을 복구하려고 한다. 이러한 방식은 모든 낮은 신뢰도 검출 결과를 새로운 객체로 받아들이지 않으면서 추적 연속성(Track Continuity)을 향상시킬 수 있다.

검출 신뢰도(Detection Confidence)와 추적 신뢰도(Tracking Confidence)의 차이를 이해하는 것이 중요하다. 검출기는 객체가 부분적으로 가려졌거나 멀리 있거나 시각적으로 모호하기 때문에 낮은 신뢰도를 부여할 수 있지만, 해당 추적 상태 자체는 이전 프레임에서 축적된 강한 시간적 증거(Temporal Evidence)를 가지고 있을 수 있다. 반대로 높은 신뢰도의 검출 결과라도 잘못된 분류나 위치 오차로 인해 기존 추적 상태와 일치하지 않을 수 있다. 따라서 추적 시스템은 객체 식별자를 유지할 때 순간적인 검출 증거와 누적된 시간적 증거를 함께 고려해야 한다.

3차원 추적(3D Tracking)은 이러한 개념을 영상 좌표(Image Coordinate)에서 물리적 공간(Physical Space)으로 확장한다. 자율주행 및 실외 AMR에서는 추적 상태에 자차 중심(Ego-Centered) 또는 월드 좌표계(World Coordinate Frame)에서의 x와 y 위치, 높이, 크기, 방향, 속도, 필요에 따라 가속도를 포함할 수 있다. 미터 단위 좌표계(Metric Coordinate System)에서 상태를 유지하면 추적기가 운동 예측(Motion Prediction), 충돌 검사(Collision Checking), 궤적 계획(Trajectory Planning)에서 직접 사용할 수 있는 정보를 제공할 수 있다.

이동하는 플랫폼에서는 좌표 일관성(Coordinate Consistency)이 특히 중요하다. AMR이나 차량 자체가 움직이면 정지해 있는 객체도 센서 좌표계에서는 움직이는 것처럼 보일 수 있다. 자차 운동 보상(Ego-Motion Compensation)을 사용하면 데이터 연관과 상태 추정 이전에 관측값을 공통 기준 좌표계(Common Reference Frame)로 변환할 수 있다. 따라서 정확한 위치 추정(Localization), 타임스탬프, 좌표 변환은 신뢰성 높은 다중 객체 추적의 필수적인 구성요소가 된다.

가림(Occlusion)은 실제 MOT에서 가장 중요한 문제 중 하나이다. 보행자가 차량 뒤로 이동하거나 하나의 차량이 다른 차량을 일시적으로 가릴 수 있으며, 카메라 또는 LiDAR 센서에서도 동일한 문제가 발생할 수 있다. 추적기는 제한된 시간 동안 숨겨진 객체의 상태를 계속 예측할 수 있지만, 관측값이 없는 시간이 길어질수록 불확실성(Uncertainty)은 증가한다. 따라서 추적 관리(Track Management)는 한 프레임에서 객체가 관측되지 않았다고 즉시 삭제하기보다 확정된 추적(Confirmed Track), 일시적으로 손실된 추적(Temporarily Lost Track), 종료된 추적(Terminated Track)을 구분해야 한다.

추적 상태의 초기화와 종료 역시 신중하게 설계해야 한다. 특히 복잡한 환경에서는 하나의 오탐 검출(False-Positive Detection)이 지속적인 객체 식별자를 생성해서는 안 된다. 추적 상태의 확정(Track Confirmation)은 여러 프레임에 걸쳐 일관된 관측을 요구할 수 있다. 마찬가지로 하나의 검출을 놓쳤다고 즉시 추적 상태를 삭제해서도 안 된다. 일시적인 가림과 센서 희소성(Sensor Sparsity)은 실제 환경에서 정상적으로 발생하기 때문이다. 이러한 정책은 잘못된 추적 생성, 추적 단절(Track Fragmentation), 식별자 안정성(Identity Stability)에 영향을 준다.

실외 AMR에서 MOT는 도로, 캠퍼스, 항만, 산업 야드, 건설 현장, 보행자 혼합 영역 등 다양한 환경에서 동작해야 한다. 객체에는 차량, 보행자, 자전거, 기계 장비, 컨테이너, 비정형 장애물 등이 포함될 수 있다. 센서 특성 역시 거리, 지형, 날씨, 가림 정도에 따라 크게 달라질 수 있다. 따라서 추적 파라미터는 기존 도로 데이터셋에만 맞추어 조정하기보다 실제 운용 설계 영역(Operational Design Domain, ODD)을 대상으로 검증해야 한다.

다중 센서 검출(Multi-Sensor Detection)은 추적에 추가적인 기회와 과제를 제공한다. 융합된 검출 결과는 추적기에 입력되기 전에 LiDAR 기하 정보, 카메라 의미 정보, 레이더 속도 정보를 결합할 수 있다. 추적기는 데이터 연관과 상태 추정 과정에서 이러한 속성을 사용할 수 있다. 레이더 속도는 움직이는 객체와 정지 객체를 구분하는 데 특히 유용할 수 있으며, LiDAR는 미터 단위 위치 정보를 제공하고 카메라 정보는 의미적 일관성(Semantic Consistency)을 지원할 수 있다. 그러나 센서의 시간 정보와 보정 오차는 추적 안정성에 직접 영향을 줄 수 있다.

추적 성능은 식별자 관련 지표와 운동 관련 지표를 모두 사용하여 평가해야 한다. 일반적인 MOT 평가 개념에는 식별자 일관성(Identity Consistency), 추적 단절, 잘못된 추적(False Track), 누락된 추적(Missed Track), 연관 정확도(Association Accuracy)가 포함된다. 실외 AMR에서는 위치 오차, 속도 오차, 추적 지연시간(Track Latency), 추적 수명(Track Lifetime), 가림 이후 복구 성능(Recovery After Occlusion), 센서 성능 저하 상황에서의 안정성도 중요하다. 평가는 이후의 예측과 계획에서 요구되는 실제 시간적 동작 특성을 반영해야 한다.

추적기는 궁극적으로 최종 인지 출력이 아니라 중간 상태 추정(State Estimation) 구성요소이다. 구조화된 추적 상태(Structured Track State)는 지속적인 객체 식별자와 함께 위치, 크기, 방향, 속도, 신뢰도, 타임스탬프, 좌표계 정보를 제공해야 한다. 이러한 상태는 이후 속도 추정과 운동 예측으로 전달되어 자율주행 스택이 주변 객체가 앞으로 어느 방향으로 움직일 가능성이 있는지를 추정할 수 있도록 한다. 이러한 관점에서 칼만 필터 기반 추적(Kalman-Based Tracking)과 ByteTrack 방식의 연관(Association)은 프레임 단위 검출과 미래 지향적 자율행동(Future-Oriented Autonomous Behavior)을 연결하는 시간적 가교(Temporal Bridge)를 형성한다.

## 05.06. Velocity Estimation and Motion Prediction [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

속도 추정(Velocity Estimation)과 운동 예측(Motion Prediction)은 객체 검출(Object Detection)과 추적(Tracking)을 현재 장면의 이해에서 미래 상태의 예측으로 확장한다. 검출기는 주변 객체를 식별하고, 추적기는 시간에 따라 객체의 식별자(Identity)를 유지한다. 속도 추정은 객체가 어떻게 움직이고 있는지를 판단하며, 운동 예측은 객체가 다음에 어디로 이동할 가능성이 있는지를 추정한다. 객체 검출 및 추적(Object Detection and Tracking) 장에서 이 단계는 다중 객체 추적(Multi-Object Tracking) 이후에 위치하며, 행동 계획(Behavior Planning), 궤적 생성(Trajectory Generation), 충돌 회피(Collision Avoidance)에 필요한 동적 정보를 제공한다.

속도는 연속적인 관측에서 객체 위치의 변화를 이용하여 추정할 수 있다. 객체 중심(Object Center)이 일관된 미터 단위 좌표계(Metric Coordinate Frame)로 표현된다면 위치 변화량을 경과 시간으로 나누어 기본적인 속도 추정값을 얻을 수 있다. 그러나 실제 환경에서 직접적인 미분(Direct Differentiation)은 위치 추정 잡음(Localization Noise), 타임스탬프 오류(Timestamp Error), 검출 누락(Missed Detection), 연관 오류(Association Error)에 민감하다. 따라서 실제 시스템에서는 독립적인 프레임 간 차이를 계산하기보다 시간적 필터링(Temporal Filtering)을 통해 속도를 추정한다.

추적 필터(Tracking Filter)는 속도 추정을 위한 자연스러운 프레임워크를 제공한다. 예를 들어 칼만 필터(Kalman Filter)는 객체 상태 벡터(Object State Vector) 내부에 위치와 속도를 함께 유지하고 새로운 측정값이 들어올 때마다 두 변수를 갱신할 수 있다. 예측 단계(Prediction Stage)는 이전 운동 상태를 시간적으로 앞으로 전파하고, 보정 단계(Correction Stage)는 예측 상태와 최신 관측값을 결합한다. 이를 통해 보다 부드러운 속도 추정값을 생성하고 짧은 시간 동안 측정값이 누락되더라도 운동 상태를 계속 추정할 수 있다.

속도 추정에 사용되는 좌표계(Coordinate Frame)는 명확하게 정의되어야 한다. 이동하는 AMR은 자체적으로 병진 및 회전하는 센서 플랫폼에서 객체를 관측하므로 센서 좌표계에서 보이는 운동에는 객체 운동과 자차 운동(Ego Motion)이 모두 포함된다. 자차 운동 보상(Ego-Motion Compensation)은 속도를 추정하기 전에 관측값을 안정적인 차량 중심 좌표계(Vehicle-Centric Coordinate Frame) 또는 월드 좌표계(World Coordinate Frame)로 변환한다. 따라서 위치 추정 품질, 좌표 변환, 정확한 타임스탬프는 객체 운동 추정의 신뢰성에 직접적인 영향을 준다.

레이더(Radar)는 도플러 측정(Doppler Measurement)이 직접적인 방사 속도(Radial Velocity) 정보를 포함하기 때문에 특히 유용한 정보를 제공할 수 있다. 이 정보는 LiDAR 또는 카메라 추적에서 추정된 속도를 보완할 수 있다. LiDAR는 정확한 공간 기하 정보를 제공하지만 일반적으로 시간에 따른 위치 변화를 통해 속도를 추정하며, 카메라는 시각적 관측과 기하 변환을 통해 운동을 추론한다. 다중 센서 융합(Multi-Sensor Fusion)은 이러한 서로 다른 정보원을 결합하여, 특히 하나의 모달리티(Modality)가 불확실해질 때 속도 추정 성능을 향상시킬 수 있다.

속도는 단순한 스칼라 속력(Scalar Speed)이 아니라 벡터(Vector)이다. 객체가 로봇을 향해 이동하는 경우와 동일한 속력으로 멀어지는 경우는 서로 다른 위험을 발생시키기 때문에 운동 방향은 속도의 크기만큼 중요하다. 지상 차량과 보행자의 경우 속도는 일반적으로 공통 좌표계에서 종방향(Longitudinal) 및 횡방향(Lateral) 성분으로 표현된다. 방향(Orientation)도 추가적인 정보를 제공할 수 있지만, 선회, 횡방향 이동, 불규칙한 보행자 운동에서는 헤딩(Heading)과 실제 속도 방향(Velocity Direction)이 항상 동일하지는 않다.

시간적 이력(Temporal History)이 충분히 안정적이라면 가속도(Acceleration)도 추정할 수 있다. 가속도를 포함하면 추적기가 제동, 가속 및 기타 비정속 운동(Non-Constant-Velocity Motion)을 표현할 수 있다. 그러나 가속도는 시간에 따른 속도의 변화에 의존하기 때문에 일반적으로 속도보다 잡음에 더 민감하다. 따라서 필터링과 불확실성 관리(Uncertainty Management)가 중요하다. 충분한 시간적 증거 없이 단기적인 측정값 변동을 의미 있는 가속도로 해석해서는 안 된다.

운동 예측은 추정된 동적 상태(Dynamic State)에서 시작하여 예측 구간(Prediction Horizon)에 걸쳐 가능한 미래 객체 상태를 투영한다. 가장 단순한 방식은 일정 속도(Constant Velocity)를 가정하여 현재 위치와 속도로부터 직선 형태의 미래 궤적(Future Trajectory)을 생성하는 것이다. 일정 가속도 모델(Constant-Acceleration Model)은 속도 변화를 표현할 수 있으며, 차량 운동학 모델(Kinematic Vehicle Model)은 헤딩과 선회 동작을 고려할 수 있다. 이러한 모델은 계산 효율성이 높으며 짧은 예측 구간과 안전 중심 충돌 검사에 여전히 유용하다.

더 긴 예측 구간에서는 객체의 운동이 환경 구조와 행동의 영향을 받기 때문에 단순한 운동학(Kinematics)만으로는 충분하지 않다. 차량은 도로를 따라 이동하거나 교차로에서 회전하거나 앞 차량 뒤에서 정지하거나 장애물을 회피할 수 있다. 보행자는 경로를 횡단하거나 정지하거나 방향을 변경하거나 다른 사람과 상호작용할 수 있다. 따라서 운동 예측은 상태 외삽(State Extrapolation)에서 지도 구조(Map Structure), 주행 가능 영역(Drivable Area), 객체 종류, 주변 에이전트(Surrounding Agent), 장면 의미(Scene Semantics)를 포함하는 문맥 인식 추론(Context-Aware Reasoning)으로 발전한다.

예측은 결정론적(Deterministic) 또는 확률적(Probabilistic)으로 수행할 수 있다. 결정론적 예측기는 하나의 미래 궤적을 생성하는 반면, 확률적 예측기는 여러 개의 가능한 궤적이나 공간 확률 분포(Spatial Probability Distribution)를 표현한다. 미래 행동은 본질적으로 불확실하기 때문에 다중 모달 예측(Multi-Modal Prediction)이 유용하다. 교차로에 접근하는 차량은 직진하거나 회전할 수 있으며, 도로 주변의 보행자는 정지 상태를 유지하거나 횡단을 시작할 수 있다. 여러 개의 가능한 미래를 표현하면 계획 시스템이 하나의 가정된 궤적에만 의존하지 않고 불확실성을 고려할 수 있다.

예측 불확실성(Prediction Uncertainty)은 일반적으로 시간이 증가할수록 커진다. 가까운 미래의 객체 위치는 현재 운동 상태의 영향을 강하게 받기 때문에 비교적 정확하게 추정할 수 있다. 그러나 미래로 멀어질수록 속도, 가속도, 의도(Intention), 상호작용의 작은 불확실성이 큰 위치 차이를 발생시킬 수 있다. 따라서 예측 시스템은 예측 궤적과 함께 신뢰도(Confidence) 또는 공분산(Covariance) 정보를 제공하여 이후 계획 모듈이 높은 신뢰도의 단기 예측과 불확실한 장기 예측을 구분할 수 있도록 해야 한다.

지도 및 환경 정보(Map and Environment Information)는 미래 운동을 제한하는 조건으로 활용할 수 있다. 차량은 일반적으로 도로, 차선, 교차로 또는 기타 주행 가능 영역 내에 머무를 것으로 예상되며, 보행자는 보도, 횡단보도, 개방 공간, 공유 공간을 통해 이동할 수 있다. 실외 AMR은 정형화된 차선 구조가 없는 환경에서도 운용될 수 있으므로 점유 지도(Occupancy Map), 주행 가능 영역 추정(Drivable-Area Estimation), 지형 경계(Terrain Boundary), 건물, 울타리, 정적 장애물(Static Obstacle)이 운동 예측을 위한 중요한 문맥적 제약(Contextual Constraint)이 된다.

에이전트 상호작용(Agent Interaction)도 중요한 요소이다. 여러 객체가 동일한 환경을 공유할 때 서로 독립적으로 움직이지 않는다. 후행 차량은 앞 차량의 움직임에 반응하고, 보행자는 접근하는 로봇이나 차량에 반응하며, 여러 에이전트는 공유 공간에서 서로의 움직임을 조정한다. 따라서 상호작용 인식 모델(Interaction-Aware Model)은 주변 객체의 상태를 추가 입력으로 사용할 수 있다. 보다 정교한 예측이 필요한 경우 그래프 기반 네트워크(Graph-Based Network), 어텐션 메커니즘(Attention Mechanism), 순환 모델(Recurrent Model), 트랜스포머 아키텍처(Transformer Architecture)를 사용하여 여러 에이전트 사이의 관계를 표현할 수 있다.

실외 AMR을 위한 예측은 일반적인 고속도로 자율주행과 다른 운용 시나리오를 포함한다. 산업 야드(Industrial Yard), 항만(Port), 캠퍼스(Campus), 물류 시설(Logistics Facility), 건설 현장(Construction Area)에는 지게차(Forklift), 트럭, 작업자, 자전거, 서비스 차량, 기계 장비, 로봇 등이 서로 다른 행동 규칙에 따라 움직일 수 있다. 일부 에이전트는 정해진 경로를 따르는 반면 다른 에이전트는 자유롭게 이동할 수 있다. 따라서 예측 모델과 데이터셋은 실제 운용 설계 영역(Operational Design Domain, ODD)의 객체 종류, 운동 패턴, 속도, 상호작용을 반영해야 한다.

정적 및 동적 분류(Static and Dynamic Classification)는 이후의 추론을 단순화할 수 있다. 추정 속도가 충분한 시간 동안 0에 가까운 객체는 정지 상태로 처리할 수 있으며, 움직이는 객체는 시간적 예측을 필요로 한다. 그러나 임계값은 측정 잡음과 위치 추정 불확실성을 고려해야 한다. 주차된 차량이 작은 추적 변동 때문에 이동 상태와 정지 상태 사이를 반복적으로 전환해서는 안 된다. 시간적 히스테리시스(Temporal Hysteresis) 또는 확률적 상태 분류(Probabilistic State Classification)를 사용하면 안정성을 향상시킬 수 있다.

추적 이력(Track History)은 예측에 중요한 정보를 제공한다. 가장 최근의 위치와 속도만 사용하는 대신 예측기는 과거의 위치, 속도, 방향, 타임스탬프를 포함하는 이전 상태 시퀀스를 입력으로 사용할 수 있다. 이러한 이력을 통해 객체가 가속하고 있었는지, 회전하고 있었는지, 정지하고 있었는지 또는 일정하게 이동하고 있었는지를 파악할 수 있다. 시간 신경망(Temporal Neural Network)과 트랜스포머 기반 예측기(Transformer-Based Predictor)는 이러한 시퀀스에서 운동 패턴을 학습할 수 있으며, 고전적인 필터(Classical Filter)는 추정 상태와 공분산을 통해 이력을 요약할 수 있다.

예측 출력(Prediction Output)은 계획 시스템이 직접 사용할 수 있는 형태로 표현해야 한다. 예측 궤적은 정의된 예측 구간에 대해 미래 위치, 방향, 속도, 타임스탬프, 불확실성을 포함할 수 있다. 다중 모달 예측기는 관련 확률 또는 신뢰도와 함께 여러 개의 후보 궤적(Candidate Trajectory)을 제공할 수 있다. 점유 기반 예측(Occupancy-Based Prediction)은 객체가 시간에 따라 점유할 가능성이 있는 영역을 대신 추정할 수 있다. 적절한 표현 방식은 계획기가 궤적 샘플링(Trajectory Sampling), 최적화(Optimization), 점유 추론(Occupancy Reasoning), 위험 기반 의사결정(Risk-Based Decision Making) 중 어떤 방식을 사용하는지에 따라 달라진다.

충돌 평가(Collision Assessment)는 운동 예측과 자율행동을 연결한다. 계획기는 주변 객체의 예측된 미래 상태와 자차 플랫폼(Ego Platform)의 후보 궤적을 비교할 수 있다. 예측된 점유 영역이 중요한 시간 구간에서 서로 겹치는 경우 시스템은 감속하거나 정지하거나 다른 궤적을 선택할 수 있다. 충돌까지의 시간(Time-to-Collision)과 최근접 접근(Closest Approach) 계산은 유용한 지표를 제공하지만, 강건한 계획 시스템은 하나의 결정론적 예측에만 의존하지 않고 예측 불확실성과 객체의 대안적 행동도 고려해야 한다.

지연시간(Latency)은 속도 추정과 예측 품질에 직접적인 영향을 준다. 센서 획득, 검출, 추적, 통신 과정에서 상당한 지연이 발생하면 계획기가 객체 상태를 전달받는 시점에는 해당 정보가 이미 과거 상태를 나타낼 수 있다. 타임스탬프 인식 상태 전파(Timestamp-Aware State Propagation)를 사용하면 추정 상태를 현재 계획 시점까지 투영하여 이러한 문제를 보상할 수 있다. 따라서 종단간 지연시간(End-to-End Latency)은 단순한 계산 성능 지표로만 취급하기보다 시간적 아키텍처에 통합하여 고려해야 한다.

평가는 속도 정확도와 미래 궤적 품질을 모두 검토해야 한다. 속도 오차(Velocity Error)는 추정된 운동 벡터와 기준 운동 벡터 사이의 차이를 이용하여 측정할 수 있다. 예측 평가는 여러 미래 시점에서의 변위 오차(Displacement Error), 최종 위치 오차(Final-Position Error), 미스율(Miss Rate), 여러 궤적을 생성하는 경우 확률적 보정(Probabilistic Calibration)을 측정할 수 있다. 자율주행 AMR에서는 급정지, 선회, 가림, 객체 재등장, 비정형 에이전트, 센서 성능 저하, 공유 공간에서의 상호작용도 추가적으로 평가해야 한다.

고장 처리(Failure Handling)는 잘못된 운동 추정이 안전 관련 의사결정에 직접적인 영향을 줄 수 있기 때문에 필수적이다. 연관 오류는 한 객체의 속도를 다른 객체에 전달할 수 있으며, 위치 추정 드리프트(Localization Drift)는 실제로 존재하지 않는 운동을 생성할 수 있고, 일시적인 검출 오류는 비현실적인 가속도를 발생시킬 수 있다. 타당성 검사(Plausibility Check)를 통해 객체 종류에 따라 최대 속도, 가속도, 선회율(Turning Rate), 변위(Displacement)를 제한할 수 있다. 또한 기반이 되는 추적 상태나 측정값의 신뢰성이 낮아질 경우 신뢰도 저하(Confidence Degradation)가 이후 모듈에도 전달되어야 한다.

따라서 속도 추정과 운동 예측은 다중 객체 추적과 자율주행 계획 사이의 시간적 추론 계층(Temporal Reasoning Layer)을 형성한다. 추적은 지속적인 객체 식별자와 필터링된 객체 상태를 제공하고, 속도 추정은 현재 운동을 정량화하며, 예측은 이러한 상태를 가능한 미래 궤적으로 확장한다. 이들을 결합하면 인지는 단순히 객체가 현재 어디에 있는지를 설명하는 수준에서 로봇이 미래 위치에 도달할 때 객체가 어디에 있을 가능성이 있는지를 추정하는 수준으로 확장된다.

전체 자율주행 AMR 아키텍처에서 이러한 시간 정보는 검출, 위치 추정, 지도(Map), 주행 가능 영역 추정, 자차 운동과 지속적으로 동기화되어야 한다. 최종적인 동적 객체 표현(Dynamic Object Representation)은 추적 식별자(Track Identity), 위치, 크기, 방향, 속도, 신뢰할 수 있는 경우 가속도, 예측 궤적, 불확실성, 타임스탬프, 좌표계 정보를 포함할 수 있다. 이러한 구조화된 상태(Structured State)는 행동 계획, 궤적 생성, 충돌 회피, 안전 모니터링(Safety Monitoring)의 직접적인 입력이 되며, 객체 검출에서 예측형 인지(Predictive Perception)로 전환되는 과정을 완성한다.

## 05.07. Long Tail Object Detection Rare Class Handling

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

롱테일 객체 검출(Long-Tail Object Detection)은 자율주행 및 실외 AMR 인지에서 발생하는 객체 클래스의 심각한 불균형 분포(Imbalanced Distribution)를 다룬다. 승용차나 보행자와 같은 일반적인 객체는 학습 데이터에 수천 번 등장할 수 있지만, 특수 기계 장비, 임시 방호물, 동물, 파손 차량 또는 특수 장비는 드물게 나타날 수 있다. 이 장의 구조에서는 희귀 클래스 처리(Rare-Class Handling)를 검출, 추적, 운동 예측 이후에 배치하여 자주 관측되는 범주를 넘어 인지 강건성(Perception Robustness)을 향상시키는 역할을 강조한다.

롱테일 분포(Long-Tail Distribution)는 소수의 빈번한 헤드 클래스(Head Class), 중간 수준으로 표현되는 더 큰 클래스 그룹, 그리고 다수의 희귀한 테일 클래스(Tail Class)로 구성된다. 일반적인 지도학습(Supervised Learning)은 학습 샘플을 지배하는 범주에 성능을 최적화하는 경향이 있다. 그 결과 검출기는 전체적으로 높은 정확도를 달성하면서도 드물게 나타나는 객체에서는 낮은 성능을 보일 수 있다. 희귀 객체 역시 중요한 운용 또는 안전 위험을 나타낼 수 있기 때문에 이러한 불균형은 자율 시스템에서 특히 중요하다.

희귀 클래스의 어려움은 단순히 샘플 수만으로 발생하지 않는다. 테일 클래스는 시각적 및 기하학적 다양성(Visual and Geometric Diversity)이 큰 경우가 많으며, 제한된 사례만으로는 전체적인 외관 분포(Appearance Distribution)를 충분히 표현하기 어렵다. 건설 차량, 특수 트레일러, 이동 보조 장치, 비정형 화물 또는 임시 도로 구조물은 매우 다양한 형태로 나타날 수 있다. 크기, 방향, 가림(Occlusion), 날씨, 센서 거리, 환경의 차이는 희귀 범주의 실질적인 복잡성을 더욱 증가시킬 수 있다.

따라서 모델을 수정하기 전에 데이터셋 분석(Dataset Analysis)이 선행되어야 한다. 엔지니어는 클래스별 객체 및 장면의 수, 공간 분포, 거리 분포, 객체 크기, 가림 수준, 센서 가시성(Sensor Visibility), 환경 조건을 측정해야 한다. 장면 수준 빈도(Scene-Level Frequency) 역시 중요하다. 소수의 위치에서 수집된 거의 동일한 수천 개의 사례보다 다양한 환경에서 수집된 더 적은 수의 사례가 더 높은 다양성을 제공할 수 있기 때문이다. 따라서 롱테일 분석은 단순한 인스턴스 수뿐만 아니라 다양성도 측정해야 한다.

클래스 재균형화(Class Rebalancing)는 학습 불균형을 감소시키기 위한 직접적인 전략을 제공한다. 희귀 클래스 샘플을 학습 과정에서 더 자주 제시할 수 있으며, 지나치게 빈번한 샘플은 줄이거나 서로 다른 가중치를 적용할 수 있다. 클래스 인식 샘플링(Class-Aware Sampling)은 과소 표현된 객체를 포함하는 장면을 의도적으로 선택할 수 있다. 그러나 지나친 오버샘플링(Oversampling)은 네트워크가 소수의 희귀 사례를 암기하도록 만들 수 있다. 따라서 재균형화는 거의 동일한 데이터를 반복적으로 제시하기보다 유용한 노출을 증가시키는 방향으로 수행해야 한다.

손실 재가중치(Loss Reweighting)는 개별 학습 사례에 부여되는 중요도를 변경한다. 희귀 클래스에서 발생한 오류에 지배적인 범주의 오류보다 높은 가중치를 부여하여 모델이 테일 객체에 더 많은 학습 능력을 할당하도록 유도할 수 있다. 포컬 방식 목적함수(Focal-Style Objective)는 쉬운 사례의 영향을 줄이고 어려운 검출을 강조할 수 있다. 그러나 희귀 클래스에 지나치게 높은 중요도를 부여하면 보정(Calibration)이 악화되거나 오탐(False Positive)이 증가하거나 빈번하게 등장하는 객체의 성능이 감소할 수 있으므로 가중치 전략을 신중하게 제어해야 한다.

데이터 증강(Data Augmentation)은 희귀 범주의 실질적인 다양성을 확장할 수 있다. 스케일링(Scaling), 회전(Rotation), 크로핑(Cropping), 광도 변환(Photometric Modification), 기하학적 변형(Geometric Perturbation)과 같은 일반적인 변환은 추가 데이터 수집 없이 다양성을 증가시킬 수 있다. 3차원 인지에서는 객체 삽입(Object Insertion), 포인트 클라우드 증강(Point-Cloud Augmentation), 장면 합성(Scene Composition)을 이용하여 희귀 객체를 새로운 공간적 문맥에 배치할 수 있다. 비현실적인 기하 구조, 조명, 크기 또는 센서 특성은 실제 배포 환경에서 존재하지 않는 패턴을 검출기가 학습하도록 만들 수 있으므로 증강 결과는 물리적으로 타당해야 한다.

합성 데이터(Synthetic Data)는 실제 환경에서 수집하기 어렵거나 비용이 많이 드는 희귀 시나리오를 생성하는 또 다른 방법을 제공한다. 시뮬레이션(Simulation)은 객체 자세, 배경, 조명, 날씨, 가림, 밀도, 센서 관점을 체계적으로 변화시킬 수 있다. 희귀 기계 장비 또는 비정상적인 장애물 배치를 대량으로 생성하는 것도 가능하다. 그러나 합성 데이터에는 도메인 격차(Domain Gap)가 존재하므로 렌더링 품질, 센서 모델링(Sensor Modeling), 도메인 무작위화(Domain Randomization), 실제 관측 데이터와의 혼합 전략을 실제 배포 데이터를 기준으로 검증해야 한다.

복사-붙여넣기 증강(Copy-Paste Augmentation)은 희귀 객체가 이미 정확하게 라벨링되어 있는 경우 특히 효과적일 수 있다. 하나의 장면에서 객체 인스턴스를 추출하여 크기, 지면 위치, 방향, 가림을 고려하면서 다른 장면에 삽입할 수 있다. LiDAR 데이터에서는 포인트 수준 삽입(Point-Level Insertion)이 실제적인 포인트 밀도와 가시성을 유지해야 한다. 카메라 영상에서는 원근(Perspective), 조명, 그림자, 경계 품질(Boundary Quality)이 중요하다. 부자연스럽게 합성된 샘플은 실제 환경에서의 일반화 성능(Generalization)을 저하시킬 수 있는 인공적인 단서를 생성할 수 있다.

전이학습(Transfer Learning)은 많은 수의 작업 특화 사례(Task-Specific Example)에 대한 의존성을 감소시킨다. 광범위한 시각 또는 3차원 데이터셋으로 사전학습된 백본(Pretrained Backbone)은 이미 형상, 질감, 기하 구조, 의미 구조에 대한 표현을 포함할 수 있다. 이후 미세조정(Fine-Tuning)을 통해 이러한 특징을 목표 운용 도메인에 적응시킨다. 파운데이션 모델(Foundation Model)과 대규모 사전학습 인코더(Large Pretrained Encoder)는 표현 품질을 더욱 향상시킬 수 있지만, 특수 산업 객체에 대한 성능은 일반적인 벤치마크 결과만으로 가정해서는 안 되며 별도로 검증해야 한다.

계층적 분류(Hierarchical Classification)는 세분화된 희귀 클래스를 구분하기 어려운 경우 도움이 될 수 있다. 많은 세부 범주 중 하나를 즉시 결정하는 대신 시스템은 먼저 차량, 취약 도로 사용자(Vulnerable Road User), 기계 장비, 장애물과 같은 더 넓은 의미 그룹(Semantic Group)을 식별할 수 있다. 이후 충분한 증거가 확보되면 두 번째 단계에서 세부 범주를 결정할 수 있다. 이러한 구조를 사용하면 정확한 희귀 하위 클래스(Rare Subclass)를 높은 신뢰도로 결정할 수 없는 경우에도 인지 시스템이 안전에 필요한 유용한 정보를 유지할 수 있다.

오픈셋 인식(Open-Set Recognition)은 객체가 학습 과정에서 표현된 어떤 클래스에도 속하지 않을 때 중요해진다. 폐쇄형 검출기(Closed-Set Detector)는 익숙하지 않은 객체를 가장 유사한 기존 범주로 잘못 분류할 수 있다. 자율주행에서는 객체의 위치, 크기, 운동, 점유 정보(Occupancy)를 유지하면서 해당 객체를 미지 객체(Unknown Object)로 표현하는 것이 더 유용할 수 있다. 따라서 미지 객체 처리는 낮은 빈도의 알려진 범주와 실제로 처음 접하는 객체를 구분함으로써 롱테일 검출을 보완한다.

객체성(Objectness)과 점유 정보는 의미적 분류(Semantic Classification)가 불확실한 경우에도 안전 정보를 제공할 수 있다. AMR은 장애물을 회피하기 전에 항상 해당 장애물의 정확한 정체를 알아야 하는 것은 아니다. 희귀 객체의 의미적 신뢰도(Semantic Confidence)가 낮더라도 신뢰할 수 있는 기하학적 위치와 점유 체적(Occupied Volume)을 가질 수 있다. 이러한 기하 검출(Geometric Detection)과 의미 분류의 분리를 통해 검출기가 익숙하지 않거나 충분히 학습되지 않은 범주를 만났을 때에도 계획 시스템이 보수적으로 동작할 수 있다.

다중 센서 융합(Multi-Sensor Fusion)은 서로 다른 센서가 익숙하지 않은 객체의 서로 다른 특성을 관측하기 때문에 희귀 객체 처리 성능을 향상시킬 수 있다. LiDAR는 의미적 분류가 불확실하더라도 형상과 거리를 제공할 수 있으며, 카메라는 질감과 시각적 외관을 제공하고, 레이더(Radar)는 운동에 대한 증거를 제공할 수 있다. 이러한 관측값을 결합하면 특정 표현 하나에 대한 의존성을 줄일 수 있다. 융합 과정에서 불확실성이 숨겨지지 않도록 센서 출처(Sensor Provenance)와 신뢰도를 유지해야 한다.

시간 정보(Temporal Information) 역시 희귀 객체 인식을 강화할 수 있다. 하나의 관측은 거리 또는 가림으로 인해 모호할 수 있지만, 여러 프레임은 서로 다른 관점과 점차 풍부해지는 증거를 제공한다. 추적(Tracking)을 통해 동일한 객체에 대한 기하학적, 의미적, 운동 정보를 누적할 수 있다. 따라서 첫 번째 검출 프레임에서 되돌릴 수 없는 결정을 내리기보다 시간에 따라 분류 신뢰도를 점진적으로 개선할 수 있다.

어려운 사례 마이닝(Hard-Example Mining)은 현재 검출기가 처리하기 어려워하는 샘플에 학습을 집중한다. 미검출(False Negative), 오탐, 낮은 신뢰도의 올바른 검출, 클래스 혼동(Classification Confusion)을 검증 또는 실제 운용 데이터에서 수집하여 다시 학습 파이프라인으로 전달할 수 있다. 롱테일 인지에서는 드물지만 중요한 실패 사례가 전체 성능 지표에 거의 영향을 주지 못할 수 있기 때문에 이러한 과정이 특히 중요하다. 마이닝은 단순히 모든 낮은 신뢰도 샘플을 선택하기보다 정보 가치가 높은 실패 패턴을 우선해야 한다.

능동학습(Active Learning)은 불확실성 또는 신규성(Novelty)에 따라 사람이 라벨링할 운용 데이터를 선택함으로써 이러한 원리를 확장한다. 기록된 모든 프레임을 라벨링하는 대신 시스템은 익숙하지 않은 객체, 비정상적인 기하 구조, 희귀한 상호작용 또는 불확실한 예측을 포함하는 장면을 식별할 수 있다. 이후 사람이 정보 가치가 가장 높은 사례를 검토하고 라벨링한다. 이를 통해 불필요한 라벨링 작업을 줄이면서 실제 배포 데이터를 기반으로 롱테일 영역의 커버리지(Coverage)를 점진적으로 개선하는 피드백 루프(Feedback Loop)를 구성할 수 있다.

희귀 클래스 평가(Rare-Class Evaluation)는 전체 검출기 성능과 분리하여 수행해야 한다. 전체 평균 정밀도(Average Precision)는 지배적인 클래스가 대부분의 평가 샘플을 차지하는 경우 심각한 실패를 숨길 수 있다. 따라서 클래스 빈도, 객체 크기, 거리, 가림, 환경, 운용 중요도에 따라 성능을 구분하여 평가해야 한다. 특히 안전과 관련된 테일 객체에서는 재현율(Recall)이 중요하다. 전체적으로 허용 가능한 정밀도(Precision)를 가진 검출기라도 특정 희귀 장애물 범주를 반복적으로 놓칠 수 있기 때문이다.

혼동 분석(Confusion Analysis)은 희귀 클래스가 완전히 검출되지 않는지 또는 지속적으로 다른 범주로 분류되는지를 판단하는 데 도움을 준다. 이 두 가지 실패 유형에는 서로 다른 대응 방법이 필요하다. 객체 자체를 놓치는 경우에는 기하학적 또는 시각적 표현이 부족할 수 있으며, 체계적인 클래스 혼동은 클래스 정의가 중첩되거나 의미 특징(Semantic Feature)이 충분하지 않음을 의미할 수 있다. 신뢰도 분포(Confidence Distribution)와 오탐의 원인을 함께 분석하면 문제가 분류, 위치 추정, 라벨 품질 또는 데이터 다양성 부족 중 어디에서 발생하는지 추가로 파악할 수 있다.

실외 AMR에서 관련되는 롱테일은 운용 설계 영역(Operational Design Domain, ODD)에 크게 의존한다. 항만에는 컨테이너 처리 장비(Container-Handling Equipment)와 특수 트레일러가 존재할 수 있고, 산업 시설에는 지게차(Forklift)와 이동형 기계 장비가 존재할 수 있으며, 캠퍼스에는 자전거와 개인형 이동장치(Personal Mobility Device)가 존재할 수 있다. 건설 지역에는 임시 방호물과 비정형 장비가 나타날 수 있다. 따라서 보편적으로 동일한 클래스 분포를 가정해서는 안 되며, 희귀 클래스 전략은 실제 목표 환경에서 발생하는 객체와 위험 요소를 기준으로 시작해야 한다.

롱테일 처리(Long-Tail Handling)는 안전 중심의 대체 동작(Safety-Oriented Fallback Behavior)과도 연결되어야 한다. 객체가 높은 기하학적 신뢰도(Geometric Confidence)를 가지지만 의미적 식별이 불확실하다면 시스템은 해당 검출 결과를 미지 장애물(Unknown Obstacle)로 유지하고 보수적인 계획 제약(Conservative Planning Constraint)을 적용할 수 있다. 기하 정보와 분류가 모두 불확실하다면 추가적인 센서 증거, 시간적 관측 또는 차량 속도 감소가 적절할 수 있다. 목표는 단순히 분류 정확도를 최대화하는 것이 아니라 의미적 불확실성이 통제되지 않은 물리적 위험으로 이어지는 것을 방지하는 것이다.

배포 모니터링(Deployment Monitoring)은 초기 학습 이후에도 롱테일이 계속 변화하기 때문에 필수적이다. 새로운 장비, 변경된 인프라, 계절성 객체, 비정형 화물, 변화하는 운용 방식은 기존 데이터셋에 포함되지 않은 새로운 분포를 생성할 수 있다. 불확실한 검출, 미지 객체, 추적 실패, 사람의 개입(Human Intervention)을 기록하면 새롭게 발생하는 인지 공백을 파악할 수 있다. 이러한 관측값은 데이터 선택, 라벨링, 재학습(Retraining), 검증, 통제된 모델 배포(Controlled Model Release)로 구성되는 지속적인 데이터 엔진 사이클(Continuous Data-Engine Cycle)에 다시 투입될 수 있다.

따라서 실제 제품 수준의 희귀 클래스 전략(Production Rare-Class Strategy)은 데이터셋 분석, 균형 학습(Balanced Training), 데이터 증강, 합성 데이터, 사전학습 표현(Pretrained Representation), 불확실성 추정(Uncertainty Estimation), 오픈셋 처리(Open-Set Handling), 시간적 증거, 실제 운용 피드백을 결합한다. 하나의 기법만으로 롱테일 문제 전체를 해결할 수는 없다. 대신 아키텍처는 의미적 확실성이 낮은 경우에도 유용한 기하 정보를 유지하면서 실제 운용 환경에서 수집된 데이터를 통해 지속적으로 커버리지를 향상시켜야 한다.

전체 자율주행 AMR 인지 아키텍처에서 롱테일 검출은 기존 객체 검출을 벤치마크 중심 인식(Benchmark-Oriented Recognition)에서 강건한 오픈월드 운용(Robust Open-World Operation)으로 확장한다. 빈번하게 등장하는 객체는 계속 정확하게 검출되어야 하고, 알려진 희귀 클래스에는 의도적인 표현과 학습이 필요하며, 이전에 관측하지 못한 객체도 물리적으로 중요한 개체(Physically Relevant Entity)로 처리되어야 한다. 이러한 기능은 인지 스택을 이 장의 이후 절에서 다루는 ROS 2 통합(ROS 2 Integration), 지연시간 분석(Latency Analysis), 실외 보행자 및 차량 검출(Outdoor Pedestrian and Vehicle Detection)로 연결하기 위한 기반을 제공한다.

## 05.08. Object Detection ROS2 Node Integration [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

객체 검출(Object Detection)은 추론(Inference)이 자율 로봇 소프트웨어 아키텍처(Autonomous Robot Software Architecture)에 통합될 때 비로소 실제 운용에 활용할 수 있다. ROS 2에서 검출기는 일반적으로 센서 데이터를 수신하고, 전처리(Preprocessing)와 신경망 추론(Neural-Network Inference)을 수행하며, 예측 결과를 구조화된 메시지(Structured Message)로 변환한 후 추적 및 계획 모듈에 게시(Publish)하는 노드(Node)로 구현된다. 이 절은 장의 구조상 희귀 클래스 처리(Rare-Class Handling) 이후와 지연시간 분석(Latency Analysis) 이전에 위치하며, 인지 알고리즘(Perception Algorithm)을 실제 배포 가능한 로봇 소프트웨어와 연결한다.

ROS 2 검출 파이프라인(Detection Pipeline)은 명확하게 정의된 입력 및 출력 인터페이스(Input and Output Interface)에서 시작한다. 카메라 기반 검출기는 이미지 및 카메라 정보 토픽(Camera-Information Topic)을 구독할 수 있으며, LiDAR 검출기는 PointCloud2 메시지를 수신할 수 있다. 다중 센서 시스템(Multi-Sensor System)은 이미지, 포인트 클라우드(Point Cloud), 레이더 관측값, 위치 추정 상태(Localization State), 보정 정보(Calibration Information)를 입력으로 사용할 수 있다. 출력은 특정 신경망의 내부 구현 방식에 이후 구성요소가 종속되지 않도록 검출 객체를 일관된 형태로 표현해야 한다.

검출 노드(Detection Node)는 일반적으로 여러 처리 단계로 구성된다. 입력되는 센서 메시지를 검증하고 모델이 요구하는 표현 형태로 변환한다. 이미지는 크기 조정(Resize), 정규화(Normalization), 텐서 변환(Tensor Conversion)을 수행할 수 있으며, 포인트 클라우드는 필터링, 좌표 변환, 복셀화(Voxelization) 또는 필러 변환(Pillar Conversion)을 수행할 수 있다. 이후 추론을 통해 원시 신경망 예측(Raw Network Prediction)을 생성하고, 이를 디코딩(Decoding) 및 필터링한 다음 객체 클래스, 신뢰도, 위치, 크기, 방향을 포함하는 ROS 2 메시지로 변환한다.

메시지 설계(Message Design)는 객체 검출이 일반적으로 더 큰 인지 파이프라인의 한 구성요소이기 때문에 중요한 아키텍처 결정이다. 구조화된 검출 메시지는 타임스탬프(Timestamp)와 좌표계 식별자(Frame Identifier)를 포함하는 헤더(Header), 그리고 검출 객체 배열(Array of Detected Objects)을 포함할 수 있다. 각 객체에는 의미 클래스(Semantic Class), 신뢰도 점수(Confidence Score), 자세(Pose), 크기, 선택적으로 속도 또는 불확실성(Uncertainty) 정보를 포함할 수 있다. 안정적인 인터페이스를 사용하면 이후의 모든 노드를 다시 설계하지 않고도 내부 검출기를 교체할 수 있다.

타임스탬프는 단순히 추론이 완료된 시간이 아니라 센서 관측값이 실제로 획득된 시간을 보존해야 한다. 신경망 추론은 수십 밀리초의 지연을 발생시킬 수 있으며, 추가적인 큐잉(Queuing)이나 전처리 과정은 관측 시점과 게시 시점의 차이를 더욱 증가시킬 수 있다. 이후의 추적(Tracking) 및 센서 융합(Sensor Fusion) 구성요소는 검출 결과를 위치 추정, 자차 운동(Ego Motion), 레이더, LiDAR 및 기타 시간적으로 연관된 정보와 정렬하기 위해 원래 측정 타임스탬프(Measurement Timestamp)를 필요로 한다.

좌표계(Coordinate Frame) 역시 중요하다. 카메라 검출기는 초기에는 영상 공간 경계 상자(Image-Space Bounding Box)를 생성할 수 있으며, LiDAR 검출기는 객체를 3차원 센서 좌표계에서 직접 추정할 수 있다. 자율주행 내비게이션(Autonomous Navigation)은 일반적으로 로봇 중심 좌표계(Robot-Centric Coordinate System) 또는 월드 좌표계(World Coordinate System)의 객체 정보를 필요로 한다. ROS 2 좌표 변환 인프라(Transform Infrastructure)를 사용하여 센서 프레임, 베이스 프레임(Base Frame), 오도메트리 프레임(Odometry Frame), 맵 프레임(Map Frame)을 연결할 수 있지만, 검출 노드는 게시되는 모든 측정값이 어느 좌표계에 속하는지를 명확하게 식별해야 한다.

검출기는 불필요한 이후 단계의 책임을 하나의 노드에 포함하지 않는 것이 좋다. 검출, 추적, 예측, 시각화(Visualization), 로깅(Logging), 계획(Planning)은 명확한 인터페이스를 가진 독립적인 구성요소로 분리할 수 있다. 이러한 모듈형 아키텍처(Modular Architecture)를 사용하면 각 기능을 적절한 갱신 주기(Update Rate)로 실행할 수 있으며 디버깅과 교체도 단순해진다. 동일한 추적기와 계획기를 유지하면서 새로운 검출기를 평가할 수 있으므로 인지 연구와 실제 제품 소프트웨어 사이의 결합도(Coupling)를 줄일 수 있다.

ROS 2 서비스 품질(Quality of Service, QoS) 설정은 센서 및 검출 데이터가 시스템 내부에서 전달되는 방식에 영향을 준다. 고속 카메라 또는 LiDAR 스트림은 모든 메시지의 전달을 보장하는 것보다 최신 데이터의 사용을 우선하는 경우가 많다. 오래된 센서 프레임은 실시간 내비게이션에서 가치가 낮을 수 있기 때문이다. 따라서 큐 깊이(Queue Depth), 신뢰성(Reliability), 지속성(Durability), 이력(History) 설정은 각 토픽의 특성에 맞게 구성해야 한다. 부적절한 설정은 메시지 누적, 과도한 지연시간 또는 게시자(Publisher)와 구독자(Subscriber) 사이의 통신 비호환성을 발생시킬 수 있다.

콜백 아키텍처(Callback Architecture) 역시 실시간 동작에 영향을 준다. 센서 수신, 전처리, 추론, 게시가 하나의 블로킹 콜백(Blocking Callback) 내부에서 순차적으로 실행되면 느린 추론 주기가 새로운 메시지의 신속한 처리를 방해할 수 있다. 실행기(Executor), 콜백 그룹(Callback Group), 작업 스레드(Worker Thread), 비동기 추론 큐(Asynchronous Inference Queue)를 이용하면 통신 처리와 계산 비용이 높은 처리를 분리할 수 있다. 다만 프레임 순서가 뒤바뀌거나 공유 GPU 및 메모리 자원의 안전성이 손상되지 않도록 동시성(Concurrency)을 신중하게 제어해야 한다.

GPU 추론(GPU Inference)은 추가적인 통합 요소를 고려해야 한다. 대용량 이미지나 포인트 클라우드를 프로세스 사이 또는 CPU와 GPU 메모리 사이에서 반복적으로 복사하면 상당한 대역폭을 소비할 수 있다. 플랫폼에서 지원하는 경우 프로세스 내부 통신(Intra-Process Communication), 컴포지션(Composition), 고정 메모리(Pinned Memory), 공유 버퍼(Shared Buffer), 하드웨어 가속 전송(Hardware-Accelerated Transport)을 사용하여 불필요한 복사를 줄일 수 있다. 목표는 단순히 높은 신경망 처리량(Throughput)을 달성하는 것이 아니라 센서 획득에서 사용 가능한 검출 결과까지 낮고 예측 가능한 종단간 지연시간(End-to-End Latency)을 확보하는 것이다.

ROS 2 컴포지션(ROS 2 Composition)을 사용하면 여러 구성요소를 동일한 프로세스 내부에서 실행하여 밀접하게 연결된 처리 단계 사이의 직렬화(Serialization) 및 전송 오버헤드(Transport Overhead)를 줄일 수 있다. 카메라 전처리 컴포넌트, 검출기, 후처리 컴포넌트(Postprocessing Component)는 독립적인 프로세스로 실행하는 것보다 데이터를 효율적으로 공유할 수 있다. 반면 프로세스 격리(Process Isolation)는 장애 격리(Fault Containment)와 디버깅 측면에서 유리할 수 있다. 따라서 배포 아키텍처는 통신 효율, 소프트웨어 모듈성, 재시작 동작(Restart Behavior), 장애 격리를 균형 있게 고려해야 한다.

수명주기 관리(Lifecycle Management)는 실제 제품 수준의 인지 노드에서 유용하다. 검출기는 센서 데이터를 안전하게 처리하기 전에 모델 로딩(Model Loading), GPU 자원 할당, 보정 검증(Calibration Verification), 파라미터 초기화(Parameter Initialization)가 필요할 수 있다. 관리형 수명주기(Managed Lifecycle)를 사용하면 노드를 설정(Configuration), 활성화(Activation), 비활성화(Deactivation), 정리(Cleanup), 종료(Shutdown) 상태로 제어하면서 전환할 수 있다. 이를 통해 시스템 시작 또는 복구 과정에서 완전히 초기화되지 않은 인지 모듈이 정상적인 것처럼 보이는 검출 결과를 게시하는 것을 방지할 수 있다.

파라미터(Parameter)는 소스 코드를 수정하지 않고 배포 환경에 특화된 설정을 변경할 수 있도록 제공되어야 한다. 모델 경로(Model Path), 신뢰도 임계값(Confidence Threshold), 비최대 억제(Non-Maximum Suppression) 설정, 클래스 매핑(Class Mapping), 센서 토픽, 좌표계, 이미지 크기, 추론 정밀도(Inference Precision), 진단 임계값(Diagnostic Threshold) 등을 외부에서 구성할 수 있다. 잘못된 모델 경로, 부정확한 클래스 매핑 또는 일치하지 않는 센서 프레임은 설정 오류임에도 인지 오류처럼 나타날 수 있으므로 파라미터 검증(Parameter Validation)이 중요하다.

모델 초기화(Model Initialization)는 일반적으로 각 콜백마다 수행하지 않고 한 번만 수행해야 한다. 노드는 초기화 과정에서 네트워크 가중치(Network Weight)를 로딩하고, 추론 엔진(Inference Engine)을 생성하고, GPU 버퍼를 할당하고, 필요하면 워밍업(Warm-Up)을 수행할 수 있다. 활성화 이후에는 입력되는 각 프레임에서 이러한 자원을 재사용한다. 목표 하드웨어에 적합한 경우 TensorRT 또는 기타 최적화된 추론 런타임(Optimized Inference Runtime)을 사용할 수 있으며, 외부 ROS 2 인터페이스는 변경하지 않아도 된다. 이러한 분리를 통해 모델 최적화를 외부 인지 인터페이스 계약(Perception Interface Contract)과 독립적으로 유지할 수 있다.

다중 센서 검출(Multi-Sensor Detection)에서는 동기화(Synchronization)가 더욱 복잡해진다. 카메라 영상, LiDAR 스캔, 레이더 측정값, 위치 추정 갱신은 서로 다른 주파수와 전송 지연시간을 가지고 도착할 수 있다. 정확한 시간 동기화(Exact Temporal Synchronization) 또는 근사 시간 동기화(Approximate Temporal Synchronization)를 사용하여 정의된 허용 범위 내의 측정값을 연관시킬 수 있으며, 버퍼링(Buffering)을 통해 특정 목표 타임스탬프에 해당하는 관측값을 검색할 수 있다. 그러나 동기화를 위해 지나치게 오래 기다리면 향상된 시간 정렬 효과가 증가한 인지 지연시간으로 상쇄될 수 있으므로 주의해야 한다.

검출 결과에는 의미 있게 추정할 수 있는 경우 불확실성(Uncertainty) 정보도 포함해야 한다. 신뢰도 점수는 의미적 또는 검출 신뢰도(Detection Confidence)를 나타내지만 반드시 공간적 불확실성(Spatial Uncertainty)을 의미하지는 않는다. 3차원 검출기는 종방향 위치(Longitudinal Position), 횡방향 위치(Lateral Position), 깊이(Depth), 크기, 방향에서 서로 다른 불확실성을 가질 수 있다. 이러한 정보를 ROS 2 메시지를 통해 보존하면 추적 및 융합 모듈이 모든 검출값을 동일한 정확도로 취급하지 않고 측정값에 적절한 가중치를 적용할 수 있다.

진단 기능(Diagnostics)은 문제 해결 단계에서만 추가하는 것이 아니라 노드 자체에 통합되어야 한다. 유용한 런타임 정보(Runtime Information)에는 입력 주파수(Input Frequency), 처리 프레임률(Processed Frame Rate), 추론 시간(Inference Time), 전처리 및 후처리 지연시간, 큐 깊이, 드롭된 프레임(Dropped Frame), GPU 사용률, 모델 상태(Model State), 출력 객체 수 등이 포함된다. 이러한 신호를 모니터링하면 실제 현장 운용에서 신경망 자체의 문제와 통신, 동기화, 자원 또는 센서 문제를 구분하는 데 도움이 된다.

오류 처리(Error Handling)는 검출기 장애가 전체 자율주행 스택을 불안정하게 만들지 않도록 설계되어야 한다. 센서 데이터 누락, 비정상 메시지(Malformed Message), 사용할 수 없는 좌표 변환, GPU 할당 오류, 추론 예외(Inference Exception), 유효하지 않은 모델 출력 등을 명시적으로 검출해야 한다. 시스템 요구사항에 따라 노드는 해당 프레임을 거부하거나 진단 상태를 게시하거나 비활성 상태로 전환하거나 상위 수준의 대체 동작(Fallback)을 요청할 수 있다. 오래된 검출 결과를 현재 데이터인 것처럼 게시하는 것은 피해야 한다.

실용적인 구현에서는 시각화(Visualization)와 기계 판독 가능한 출력(Machine-Readable Output)을 분리해야 한다. 경계 상자, 포인트 클라우드 마커(Point-Cloud Marker), 클래스 라벨, 궤적은 개발자와 운영자에게 유용하지만 시각화 메시지는 상당한 통신 대역폭과 처리 자원을 소비할 수 있다. 기본 검출 토픽(Primary Detection Topic)은 간결하고 구조화된 형태로 유지하고, 필요한 경우 선택적인 시각화 노드가 동일한 결과를 구독하여 RViz 마커, 주석 영상(Annotated Image), 디버깅 오버레이(Debugging Overlay)를 생성하도록 구성할 수 있다.

기록 및 재생(Recording and Replay)은 검출기 개발에서 필수적이다. ROS 2 bag 데이터는 실제 운용 과정에서 센서 토픽, 좌표 변환, 위치 추정 정보, 검출 출력, 진단 정보를 기록할 수 있다. 이후 동일한 데이터를 수정된 검출 노드를 통해 다시 재생하여 장애를 재현하거나 동일한 조건에서 서로 다른 모델을 비교할 수 있다. 재현 가능한 재생(Reproducible Replay)은 현장 시험(Field Testing), 모델 개발, 소프트웨어 디버깅, 회귀 검증(Regression Validation)을 연결하는 중요한 수단을 제공한다.

통합 시험(Integration Testing)은 단순히 검출 결과가 특정 토픽에 나타나는지만 확인해서는 안 된다. 메시지 스키마(Message Schema), 타임스탬프, 좌표계, 좌표 변환 사용 가능 여부, 갱신 주기, 시작 동작(Startup Behavior), 파라미터 처리, 프레임 드롭 동작, 이후 추적 모듈과의 호환성을 검증해야 한다. 기록된 시나리오를 이용하면 검출기 또는 추론 런타임의 변경이 다른 모듈에서 기대하는 인터페이스나 시간적 동작을 의도하지 않게 변경하지 않았는지 확인할 수 있다.

실외 AMR에서 ROS 2 검출 노드는 궁극적으로 센서 특화 AI 연산(Sensor-Specific AI Computation)과 나머지 자율주행 소프트웨어 스택 사이의 경계 역할을 한다. 카메라, LiDAR, 레이더 또는 융합된 관측값을 추적, 운동 예측, 계획, 안전 기능이 사용할 수 있는 타임스탬프 기반 및 좌표계 일관 객체 표현(Frame-Consistent Object Representation)으로 변환한다. 잘 설계된 통합 구조는 모듈성을 유지하면서 신뢰할 수 있는 자율주행에 필요한 시간 정보와 불확실성 정보를 함께 제공한다.

따라서 실제 제품 배포(Production Deployment)에서는 객체 검출을 기계학습 기능(Machine-Learning Function)이면서 동시에 실시간 소프트웨어 구성요소(Real-Time Software Component)로 다루어야 한다. 메시지가 지연되거나 좌표계가 일치하지 않거나 큐가 통제되지 않은 상태로 증가하거나 장애가 관측되지 않는다면 높은 모델 정확도만으로 유용한 인지를 보장할 수 없다. 강건한 ROS 2 통합(Robust ROS 2 Integration)은 명확한 인터페이스, 동기화, 좌표 변환, 제어된 실행(Controlled Execution), 진단, 수명주기 관리, 재현 가능한 시험을 결합하며, 다음 절에서 다루는 검출 지연시간과 정확도 절충 분석(Detection Latency and Accuracy Trade-Off Analysis)의 기반을 제공한다.

## 05.09. Detection Latency and Accuracy Trade off Analysis

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

검출 지연시간(Detection Latency)과 정확도(Accuracy)는 자율주행 및 실외 AMR 인지에서 가장 중요한 공학적 절충 관계(Engineering Trade-Off) 중 하나를 형성한다. 정확도가 매우 높은 검출기라도 결과가 추적과 계획에 너무 늦게 전달되면 실제 활용 가치가 제한되며, 반대로 매우 빠른 검출기라도 중요한 객체를 자주 놓친다면 안전에 문제가 발생할 수 있다. 따라서 이 장에서는 ROS 2 검출기 통합 이후와 실외 AMR 검출 사례 이전에 지연시간과 정확도 분석을 배치하여 알고리즘 성능을 실제 배포 가능한 시스템 동작과 연결한다.

검출 지연시간은 신경망 추론 시간(Neural-Network Inference Time)만이 아니라 종단간 지연시간(End-to-End Latency)으로 측정해야 한다. 센서 노출 또는 스캔 획득, 데이터 전송, ROS 2 전송, 전처리(Preprocessing), 호스트-장치 전송(Host-to-Device Transfer), 추론(Inference), 후처리(Postprocessing), 좌표 변환, 메시지 게시, 이후 모듈과의 통신이 모두 지연에 영향을 줄 수 있다. 따라서 GPU 실행 시간만 측정하면 계획기(Planner)가 전달받는 정보의 실제 경과 시간을 크게 과소평가할 수 있다.

지연시간(Latency)과 처리량(Throughput)의 차이를 이해하는 것은 매우 중요하다. 초당 60프레임을 처리하는 검출기가 반드시 각 관측 결과를 16.7밀리초의 지연시간으로 전달하는 것은 아니다. 파이프라이닝(Pipelining)과 배치 처리(Batching)를 사용하면 높은 처리량을 달성하면서도 개별 프레임에는 상당한 대기시간이 발생할 수 있다. 자율 시스템은 데이터를 지속적으로 처리하는 동시에 각각의 결과가 실시간 의사결정에 충분히 최신 상태를 유지해야 하므로 일반적으로 두 지표 모두에 민감하다.

정보 경과 시간(Information Age)은 또 다른 유용한 관점을 제공한다. 카메라 프레임이 시각 t0에 촬영되었지만 검출 결과가 t0 이후 100밀리초에 계획기에 도달한다면 그 시간 동안 객체는 상당한 거리를 이동했을 수 있다. 10 m/s로 이동하는 객체의 경우 100밀리초는 약 1미터의 이동 거리에 해당한다. 타임스탬프 인식 추적(Timestamp-Aware Tracking)은 이러한 지연의 일부를 보상할 수 있지만, 크거나 변동성이 높은 지연시간은 불확실성을 증가시키고 충돌 회피에 사용할 수 있는 시간을 감소시킨다.

정확도 역시 다차원적인 개념이다. 정밀도(Precision)와 재현율(Recall)은 각각 잘못된 검출과 놓친 객체의 특성을 나타내며, 위치 추정 품질(Localization Quality)은 객체 위치 또는 경계 상자(Bounding Box)가 얼마나 정확하게 추정되었는지를 나타낸다. 3차원 검출기는 추가적으로 신뢰할 수 있는 깊이, 크기, 방향 정보를 필요로 한다. 클래스별 성능(Class-Specific Performance)도 중요하다. 전체 성능 지표가 우수해 보이더라도 보행자, 자전거, 원거리 차량 또는 운용상 중요한 희귀 장애물에서 낮은 검출 성능이 숨겨질 수 있기 때문이다.

모델 복잡도(Model Complexity)는 지연시간과 정확도의 절충 관계를 발생시키는 주요 요인이다. 더 큰 백본(Backbone), 깊은 특징 피라미드(Feature Pyramid), 높은 용량의 검출 헤드(Detection Head), 복잡한 융합 모듈(Fusion Module)은 표현 품질을 향상시킬 수 있지만 더 많은 연산과 메모리 대역폭을 요구한다. 작은 모델은 일반적으로 더 빠르게 실행되지만 작거나 멀리 있거나 부분적으로 가려지거나 시각적으로 모호한 객체에서 정확도가 감소할 수 있다. 따라서 모델 선택은 벤치마크 정확도만이 아니라 실제 운용 요구사항을 기준으로 수행해야 한다.

입력 해상도(Input Resolution)도 강한 영향을 미친다. 높은 해상도의 카메라 영상은 작고 멀리 있는 객체에 대한 더 많은 정보를 유지하지만 영상 크기가 증가하면서 신경망이 처리해야 하는 픽셀 수도 빠르게 증가한다. 해상도를 낮추면 연산량과 메모리 전송량을 감소시켜 지연시간을 개선할 수 있지만 안정적인 검출에 필요한 특징이 손실될 수 있다. 따라서 해상도는 실제 운용 설계 영역(Operational Design Domain, ODD)에서 요구되는 객체 크기와 감지 거리를 기준으로 평가해야 한다.

LiDAR 검출에서는 포인트 클라우드 밀도(Point-Cloud Density)와 공간적 처리 범위(Spatial Range)가 계산 비용에 영향을 준다. 넓은 3차원 영역의 모든 포인트를 처리하면 복셀화(Voxelization), 특징 추출(Feature Extraction), 검출 헤드 연산량이 증가할 수 있다. 관심 영역(Region of Interest)을 제한하거나 불필요한 포인트를 줄이거나 복셀 및 필러 해상도(Voxel and Pillar Resolution)를 조정하면 처리 속도를 향상시킬 수 있다. 그러나 지나친 축소는 작은 객체나 원거리 장애물의 검출 성능과 기하학적 위치 정확도를 저하시킬 수 있다.

다중 센서 융합(Multi-Sensor Fusion)은 강건성과 검출 정확도를 향상시킬 수 있지만 추가적인 시간 비용을 발생시킨다. 카메라, LiDAR, 레이더 관측값은 전송, 동기화(Synchronization), 좌표 변환, 인코딩, 융합 과정을 거쳐야 한다. 특히 센서의 갱신 주기가 서로 다른 경우 여러 센서의 데이터를 기다리는 과정에서 지연시간이 증가할 수 있다. 따라서 융합 아키텍처는 추가 모달리티(Modality)를 통해 얻는 정확도 향상이 사용 가능한 실시간 예산(Real-Time Budget) 내에서 동기화 오버헤드와 증가된 계산량을 보상할 수 있는지 평가해야 한다.

배치 크기(Batch Size)는 또 다른 중요한 배포 파라미터이다. 큰 배치는 특히 오프라인 처리에서 GPU 활용률과 전체 처리량을 향상시킬 수 있지만 여러 프레임이 모일 때까지 기다리는 시간이 추가적인 지연을 발생시킨다. 실시간 로봇 시스템은 관측 데이터를 즉시 처리하기 위해 배치 크기 1 또는 매우 작은 배치를 사용하는 경우가 많다. 다중 카메라 시스템에서는 여러 동기화된 영상을 함께 처리하면 효율성이 향상될 수 있지만 메모리 소비와 스케줄링 복잡성(Scheduling Complexity)이 증가하므로 추가적인 분석이 필요하다.

수치 정밀도(Numerical Precision)는 추론 속도와 모델 동작 모두에 영향을 준다. FP32는 넓은 수치 범위를 제공하며 일반적으로 기준 정밀도로 사용되는 반면, FP16은 메모리 전송량을 줄이고 가속기의 처리량을 향상시킬 수 있다. INT8 양자화(INT8 Quantization)는 호환되는 하드웨어에서 추가적인 성능 향상을 제공할 수 있지만 양자화가 검출 정확도에 영향을 줄 수 있으므로 보정(Calibration)과 모델 검증이 필요하다. 따라서 정밀도 선택은 자동적인 최적화가 아니라 실험적으로 검증된 배포 결정으로 다루어야 한다.

모델 가지치기(Model Pruning)와 아키텍처 최적화(Architecture Optimization)는 최종 성능에 거의 기여하지 않는 연산을 제거할 수 있다. 채널 가지치기(Channel Pruning), 계층 단순화(Layer Simplification), 연산자 융합(Operator Fusion), 최적화된 커널(Optimized Kernel), 그래프 수준 변환(Graph-Level Transformation)을 사용하여 추론 비용을 줄일 수 있다. TensorRT와 같은 하드웨어 특화 런타임(Hardware-Specific Runtime)은 실행 효율을 추가로 향상시킬 수 있다. 이론적인 연산량만으로 실제 실행 지연시간을 직접 결정할 수 없으므로 이러한 최적화는 실제 배포 모델과 목표 하드웨어에서 검증해야 한다.

후처리(Postprocessing) 역시 지연시간 예산에서 의미 있는 부분을 차지할 수 있다. 수천 개의 후보 경계 상자를 디코딩하고, 신뢰도 필터링(Confidence Filtering), 비최대 억제(Non-Maximum Suppression, NMS), 좌표 변환, 메시지 구성을 수행하는 과정은 신경망 연산이 종료된 이후에도 시간을 소비한다. 추론은 GPU에서 가속되지만 후처리가 CPU에 남아 있다면 후처리가 새로운 병목(Bottleneck)이 될 수 있다. 따라서 프로파일링(Profiling)은 모델 출력 텐서에서 끝나는 것이 아니라 전체 검출 파이프라인을 대상으로 수행해야 한다.

메모리 이동(Memory Movement) 역시 엣지 컴퓨터(Edge Computer)에서 실행 시간을 지배할 수 있다. 카메라 영상, 포인트 클라우드, 중간 텐서(Intermediate Tensor), 검출 출력은 센서 버퍼, CPU 메모리, GPU 메모리, ROS 2 메시지 사이에서 반복적으로 복사될 수 있다. 불필요한 직렬화(Serialization)와 복사를 제거하면 신경망 자체를 변경하지 않고도 지연시간을 개선할 수 있다. 따라서 프로세스 내부 통신(Intra-Process Communication), 재사용 가능한 버퍼(Reusable Buffer), 고정 메모리(Pinned Memory), 제로카피(Zero-Copy) 메커니즘은 모델의 계산량을 줄이는 것만큼 중요할 수 있다.

지연시간 변동(Latency Variation), 즉 지터(Jitter)는 평균 지연시간과 별도로 고려해야 한다. 평균적으로 40밀리초가 걸리지만 간헐적으로 150밀리초가 필요한 검출기는 항상 50밀리초 안에 완료되는 검출기보다 안전하게 통합하기 어려울 수 있다. GPU 자원 경쟁(Contention), 메모리 할당, 백그라운드 프로세스, 열 스로틀링(Thermal Throttling), 동기화 대기, 큐 누적 등이 이러한 변동을 발생시킬 수 있다. 따라서 실시간 평가는 평균값뿐만 아니라 백분위수(Percentile)와 최악 조건(Worst-Case Behavior)도 함께 보고해야 한다.

큐 관리(Queue Management)는 정보의 최신성에 큰 영향을 준다. 추론 속도가 입력 센서 주기보다 느리면 프레임이 큐에 누적될 수 있다. 큐에 있는 모든 프레임을 처리하면 데이터의 완전성은 유지할 수 있지만 점점 오래된 검출 결과가 계획기에 전달된다. 실시간 내비게이션에서는 오래된 프레임을 버리고 가장 최신 관측값을 처리하는 것이 더 적절할 수 있다. 올바른 정책은 애플리케이션이 완전한 기록과 즉각적인 환경 인식 중 어느 것을 우선하는지에 따라 달라진다.

동적 추론(Dynamic Inference)은 운용 조건에 따라 계산량을 조정할 수 있는 방법을 제공한다. 혼잡하지 않은 환경에서 저속으로 이동하는 로봇은 낮은 처리 주기 또는 가벼운 모델을 사용할 수 있으며, 높은 속도 또는 밀집된 보행자 환경에서는 더 많은 계산 자원을 사용하는 것이 적절할 수 있다. 관심 영역 처리(Region-of-Interest Processing)를 통해 안전에 중요한 영역에 계산 자원을 집중할 수도 있다. 그러나 구성 사이의 전환이 예측할 수 없는 인지 공백(Perception Gap)을 발생시키지 않도록 적응형 전략(Adaptive Strategy)을 신중하게 검증해야 한다.

지연시간 예산(Latency Budget)은 사용 가능한 컴퓨팅 성능만을 기준으로 선택하는 것이 아니라 차량 동역학(Vehicle Dynamics)과 안전 요구사항으로부터 도출해야 한다. 로봇 속도, 제동 성능, 정지 거리(Stopping Distance), 센서 거리, 제어기 주기(Controller Frequency), 계획 구간(Planning Horizon), 예상 장애물 운동은 인지 정보가 어느 정도까지 오래되어도 안전한지를 결정한다. 따라서 저속 실내 AMR과 더 빠른 실외 차량은 유사한 인지 모델을 사용하더라도 서로 다른 검출 마감시간(Detection Deadline)을 요구할 수 있다.

정확도 역시 거리와 운용 위험도(Operational Risk)를 기준으로 평가해야 한다. 충분한 시간이 남아 있어 이후에 다시 검출할 수 있는 원거리 객체의 누락은 즉각적인 영향이 제한적일 수 있지만, 가까운 보행자를 놓치는 것은 치명적인 문제가 될 수 있다. 거리 구간별 재현율(Range-Binned Recall)과 위치 오차(Localization Error)는 하나의 전체 성능 점수로는 확인할 수 없는 특성을 보여준다. 또한 정적 장애물, 이동 차량, 보행자, 자전거, 희귀 객체를 목표 운용 환경에서의 중요도에 따라 구분하여 평가할 수 있다.

유용한 공학적 분석은 하나의 보편적으로 최적인 모델을 찾기보다 여러 검출기 구성(Detector Configuration)을 평가한다. 각 구성은 아키텍처, 입력 해상도, 수치 정밀도, 센서 조합, 신뢰도 임계값, 추론 런타임 등을 다르게 설정할 수 있다. 정확도와 종단간 지연시간을 함께 측정하면 일부 구성은 다른 구성에 의해 지배되고, 나머지는 의미 있는 절충점을 형성하는 파레토 프런티어(Pareto Frontier)를 구성한다. 이후 명확하게 정의된 플랫폼 및 운용 제약조건을 기준으로 적절한 구성을 선택할 수 있다.

벤치마킹(Benchmarking)은 가능하면 실제 목표 컴퓨터(Target Computer)에서 수행해야 한다. 데스크톱 GPU에서 측정된 결과는 연산 처리량, 메모리 대역폭, 열 제한(Thermal Limit), 전력 모드(Power Mode), 가속기 지원이 크게 다르기 때문에 임베디드 플랫폼의 성능을 정확하게 나타내지 못할 수 있다. 또한 동일한 모델이라도 위치 추정, 추적, 계획, 기록, 시각화 작업이 동시에 실행될 때 다른 성능을 보일 수 있다. 따라서 독립적인 검출기 벤치마크뿐만 아니라 전체 자율주행 스택이 실행되는 상태에서도 성능을 측정해야 한다.

프로파일링 도구(Profiling Tool)를 사용하면 전체 파이프라인을 센서 수신, 전처리, 데이터 전송, 추론, 후처리, ROS 2 통신, 이후 모듈의 데이터 소비 단계로 분해할 수 있다. 이러한 분해를 통해 최적화 대상이 모델인지, 소프트웨어 아키텍처인지, 메모리 전송인지, 동기화인지 또는 스케줄링인지 판단할 수 있다. 프로파일링 없이 단순히 검출기를 더 작은 신경망으로 교체하면 실제 지배적인 지연 원인이 메시지 전송이나 전처리에 존재하는 경우 거의 효과를 얻지 못할 수 있다.

최종 목표는 최소 지연시간이나 최대 벤치마크 정확도를 각각 독립적으로 달성하는 것이 아니라 예측 가능한 시간 예산(Temporal Budget) 안에서 충분한 인지 품질(Perception Quality)을 제공하는 것이다. 선택된 구성은 실제 시스템 부하 조건에서 제한된 종단간 지연시간과 허용 가능한 지터를 유지하면서 운용상 중요한 객체를 요구되는 거리에서 검출할 수 있어야 한다. 이를 위해 AI 정확도, 컴퓨팅 성능, ROS 2 통신, 센서 타이밍, 차량 동역학을 함께 평가해야 한다.

따라서 검출 지연시간과 정확도의 절충 분석(Detection Latency and Accuracy Trade-Off Analysis)은 모델 평가를 시스템 수준 공학 과정(System-Level Engineering Process)으로 확장한다. 정확도 지표는 검출기가 무엇을 인식할 수 있는지를 결정하고, 지연시간은 해당 정보가 언제 자율주행 스택에 제공되는지를 결정한다. 이 두 요소의 상호작용은 추적, 예측, 계획, 안전 기능에서 인지가 가지는 실제적인 가치를 결정한다. 이러한 시스템 수준의 관점은 이 장을 마무리하는 다음 절의 실외 AMR 보행자 및 차량 검출 사례(Outdoor AMR Pedestrian and Vehicle Detection Case)를 위한 기반을 제공한다.

## 05.10. Outdoor AMR Pedestrian Vehicle Detection Case

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

실외 자율이동로봇(Outdoor Autonomous Mobile Robot)은 기존의 실내 AMR 공간보다 구조화 정도가 낮은 환경에서 운용되면서 보행자(Pedestrian)와 차량(Vehicle)을 신뢰성 있게 검출해야 한다. 도로, 캠퍼스, 산업 야드(Industrial Yard), 항만, 아파트 단지, 물류 시설에는 보행자, 승용차, 트럭, 자전거, 서비스 차량, 기계 장비가 동일한 운용 공간을 공유할 수 있다. 이 사례 연구(Case Study)는 앞에서 다룬 검출, 추적, 예측, ROS 2 통합, 지연시간 개념을 실외 AMR 시나리오에 적용하면서 객체 검출 및 추적(Object Detection and Tracking) 장을 마무리한다.

인지 목표(Perception Objective)는 단순히 객체에 클래스 라벨(Object Label)을 할당하는 것이 아니다. AMR은 객체가 로봇의 미래 경로를 점유하고 있는지 또는 진입할 가능성이 있는지를 판단하고, 객체의 3차원 위치를 추정하며, 객체 식별자(Identity)를 지속적으로 유지하고, 객체의 이동 여부를 판단하며, 계획 모듈이 사용할 수 있을 만큼 최신 상태의 정보를 제공해야 한다. 보행자는 궤적이 빠르게 변화할 수 있기 때문에 특별한 주의가 필요하며, 차량은 더 긴 감지 거리에서 신뢰할 수 있는 거리, 방향, 운동 추정이 요구된다.

대표적인 실외 센서 구성(Outdoor Sensor Configuration)은 카메라(Camera), LiDAR, 레이더(Radar), 위치 추정(Localization), 관성 정보(Inertial Information)를 결합할 수 있다. 카메라는 보행자와 다양한 차량 클래스를 구분하는 데 유용한 밀집된 시각 및 의미 정보를 제공한다. LiDAR는 미터 단위 기하 정보(Metric Geometry)와 3차원 객체 위치를 제공하며, 레이더는 시각적 외관 정보의 품질이 저하되는 조건에서도 거리와 방사 속도(Radial Velocity)를 제공할 수 있다. 위치 추정과 IMU 정보는 자차 운동 보상(Ego-Motion Compensation)과 좌표 변환(Coordinate Transformation)을 지원한다.

센서 배치(Sensor Placement)는 개별 센서의 명목상 시야각(Field of View)을 최대화하는 것보다 AMR 주변에 상호 보완적인 감지 범위(Complementary Coverage)를 제공하도록 설계해야 한다. 전방 센서는 정지 및 회피에 충분한 감지 거리를 확보해야 하며, 측면과 후방 감지는 회전, 후진, 공유 공간 운용에서 중요해진다. 차량 본체 가까이에 존재하는 사각지대(Blind Zone)는 특히 보행자에게 위험할 수 있다. 센서 감지 영역의 중첩(Sensor Overlap)은 중복성(Redundancy)을 제공하고 객체가 서로 다른 감지 영역 사이를 이동하는 동안에도 지속적으로 관측될 수 있도록 한다.

처리 파이프라인(Processing Pipeline)은 센서 관측값에 타임스탬프(Timestamp)를 부여하고 동기화(Synchronization)하는 단계에서 시작한다. 카메라 영상은 영상 보정(Rectification)과 정규화(Normalization)를 수행하고, LiDAR 스캔은 필터링 및 좌표 변환을 수행하며, 레이더 측정값은 공간적 관측 정보와 연관시킬 수 있다. 보정(Calibration)을 통해 모든 측정값은 로봇을 기준으로 정의된 센서 좌표계로 변환된다. 정확한 시간 정보가 없으면 이동하는 AMR이 서로 다른 실제 장면 상태를 나타내는 측정값을 하나의 시점 정보처럼 결합할 수 있으므로 정확한 타이밍이 필수적이다.

검출(Detection)은 각각의 모달리티(Modality)에 대해 독립적으로 수행하거나 융합 아키텍처(Fusion Architecture)를 통해 수행할 수 있다. 카메라 네트워크는 보행자, 승용차, 트럭, 자전거 및 기타 의미 클래스(Semantic Class)를 검출할 수 있으며, LiDAR 검출기는 포인트 클라우드 기하 구조(Point-Cloud Geometry)로부터 3차원 경계 상자(3D Bounding Box)를 추정한다. 융합 모델은 영상의 의미 정보와 LiDAR 기하 정보, 레이더 운동 정보를 결합할 수 있다. 적절한 아키텍처는 사용 가능한 컴퓨팅 자원, 센서 특성, 환경 조건, 요구되는 검출 거리에 따라 달라진다.

보행자 검출(Pedestrian Detection)은 여러 실제적인 문제를 가진다. 사람은 서로 다른 크기, 방향, 의복, 자세로 나타날 수 있으며 주차 차량, 식생, 인프라 또는 다른 사람에 의해 부분적으로 가려질 수 있다. 원거리에서는 소수의 영상 픽셀 또는 LiDAR 포인트만으로 보행자가 표현될 수 있다. 따라서 모든 보행자 인스턴스를 대상으로 계산된 전체 정확도에만 의존하기보다 거리와 가림(Occlusion) 정도에 따라 검출기를 평가해야 한다.

차량 검출(Vehicle Detection)은 보행자와 다른 기하학적 특성을 가진다. 승용차, 밴, 버스, 트럭, 지게차(Forklift), 특수 산업 차량은 크기와 외관에서 상당한 차이를 보일 수 있다. 주차 구역이나 물류 야드에서는 차량 일부가 컨테이너 또는 다른 장비에 가려지는 부분 가시성(Partial Visibility)이 흔하게 발생한다. 계획 시스템은 단순한 2차원 영상 경계 상자가 아니라 객체가 실제로 점유하는 물리적 영역을 필요로 하기 때문에 3차원 위치 추정(3D Localization)이 특히 중요하다.

검출 이후에는 다중 객체 추적(Multi-Object Tracking)이 보행자와 차량의 지속적인 식별자를 유지한다. 칼만 필터 기반 추적기(Kalman-Filter-Based Tracker)는 관측 사이의 객체 상태를 예측할 수 있으며, 연관 로직(Association Logic)은 새로운 검출 결과를 기존 추적 상태와 연결한다. ByteTrack 방식으로 낮은 신뢰도의 검출 결과를 활용하면 일시적인 가림 상황에서도 추적 상태를 유지하는 데 도움이 될 수 있다. 추적 관리(Track Management)는 한 프레임의 검출 누락만으로 객체를 삭제하지 않으면서도 객체가 사라졌다는 충분한 증거가 확보되면 오래된 추적 상태를 제거해야 한다.

AMR 자체가 환경을 관측하면서 이동하기 때문에 자차 운동 보상(Ego-Motion Compensation)은 필수적이다. 센서 좌표계에서 움직이는 것처럼 보이는 객체가 실제 월드 좌표계에서는 정지해 있을 수 있으며, 이동 차량의 측정된 상대 속도(Relative Velocity)에는 차량 자체의 운동과 로봇 운동이 함께 포함된다. 위치 추정 및 좌표 변환을 사용하면 검출 결과와 추적 상태를 로봇 중심 좌표계(Robot-Centric Coordinate Frame) 또는 월드 좌표계(World Coordinate Frame)에서 일관되게 표현할 수 있으며, 이를 통해 속도 추정과 이후의 운동 예측 성능을 향상시킬 수 있다.

보행자의 행동은 빠르게 변화할 수 있기 때문에 보행자 운동 예측(Pedestrian Motion Prediction)은 불확실성(Uncertainty)을 고려해야 한다. AMR과 평행하게 걷던 보행자가 갑자기 로봇의 경로를 횡단할 수 있으며, 도로 주변에 정지해 있던 사람이 긴 사전 동작 없이 움직이기 시작할 수도 있다. 단기 일정 속도 예측(Short-Horizon Constant-Velocity Prediction)은 유용한 기준 모델을 제공하지만, 공유 보행 환경에서는 여러 개의 가능한 미래 궤적(Future Trajectory) 또는 점유 영역(Occupancy Region)을 이용하여 불확실성을 표현하는 것이 보다 현실적일 수 있다.

차량 예측(Vehicle Prediction)은 상대적으로 강한 운동학적 제약(Kinematic Constraint)을 활용할 수 있다. 승용차와 트럭은 일반적으로 조향과 도로 기하 구조에 따라 방향을 변경하기 때문에 일정 속도(Constant Velocity), 일정 가속도(Constant Acceleration), 운동학 모델(Kinematic Model)을 이용하여 유용한 단기 예측을 수행할 수 있다. 지도 정보(Map Information)와 주행 가능 영역 추정(Drivable-Area Estimation)은 가능한 궤적을 추가로 제한할 수 있다. 그러나 산업 현장의 지게차와 서비스 차량은 후진하거나 급격하게 회전하거나 비정형 경로를 따를 수 있으므로 일반적인 도로 차량의 가정을 모든 객체에 동일하게 적용해서는 안 된다.

정적 객체(Static Object)는 동적 에이전트(Dynamic Agent)와 구분되어야 한다. 주차 차량, 건설 장비, 방호물, 컨테이너 및 기타 구조물도 유효한 객체 검출 결과를 생성할 수 있지만 서로 다른 예측 동작이 필요하다. 안정적인 속도 추정과 시간적 분류(Temporal Classification)를 사용하면 정지 객체와 이동 객체를 구분할 수 있다. 의미적 분류가 불확실한 경우에도 신뢰할 수 있는 기하 정보는 장애물로 유지하여 계획 시스템이 주행 가능 영역을 점유하는 미지 객체(Unknown Object)를 무시하지 않도록 해야 한다.

희귀 객체와 익숙하지 않은 객체는 실외 배포 환경에서 특히 중요하다. 일반 도로 차량을 중심으로 학습된 검출기는 유지보수 장비, 트레일러, 이동 보조 장치(Mobility Device), 비정형 화물, 임시 장비 또는 개조 차량을 만날 수 있다. 오픈셋 처리(Open-Set Handling)를 사용하면 이러한 검출 결과를 잘못된 기존 클래스로 강제 분류하는 대신 미지 객체로 유지할 수 있다. 이후 AMR은 의미적 불확실성이 존재하더라도 위치, 크기, 점유 정보, 운동 정보를 이용하여 보수적인 회피(Conservative Avoidance)를 수행할 수 있다.

환경 조건(Environmental Condition)은 센서 성능을 크게 변화시킬 수 있다. 강한 햇빛, 그림자, 야간 운용, 비, 안개, 먼지, 반사 표면, 젖은 도로는 카메라와 능동 센서(Active Sensor)에 서로 다른 영향을 준다. 특정 조건에서는 LiDAR 반사 신호가 희박하거나 잡음이 증가할 수 있으며, 카메라는 대비를 잃거나 눈부심(Glare)의 영향을 받을 수 있다. 따라서 다중 센서 중복성(Multi-Sensor Redundancy)은 정상 조건에서 정확도를 향상시키는 것뿐만 아니라 개별 모달리티의 신뢰성이 저하될 때 점진적 성능 저하(Graceful Degradation)를 가능하게 한다는 점에서도 가치가 있다.

ROS 2 구현(ROS 2 Implementation)에서는 검출, 추적, 예측 기능을 모듈형 노드(Modular Node) 또는 컴포지션된 구성요소(Composed Component)로 제공할 수 있다. 센서 토픽은 전처리와 추론 단계에 입력되고, 구조화된 검출 메시지는 추적기로 전달되며, 추적된 상태는 운동 예측과 계획으로 전달된다. 이 전체 과정에서 타임스탬프와 좌표계 식별자(Frame Identifier)는 일관성을 유지해야 한다. 고대역폭 카메라와 포인트 클라우드 데이터를 엣지 GPU(Edge GPU)에서 처리할 때 컴포지션(Composition)과 효율적인 메모리 처리를 사용하면 불필요한 데이터 복사를 줄일 수 있다.

실시간 성능(Real-Time Performance)은 검출기 추론 시간만이 아니라 종단간 지연시간(End-to-End Latency)을 사용하여 평가해야 한다. 센서 획득, 동기화, 전처리, GPU 전송, 추론, 후처리, 추적, 좌표 변환, ROS 2 통신이 모두 사용 가능한 시간 예산(Temporal Budget)의 일부를 소비한다. 높은 정확도를 가진 보행자 검출 결과라도 너무 늦게 전달되면 정확도가 조금 낮더라도 충분히 최신 상태인 관측보다 실제 운용 가치가 떨어질 수 있으므로 큐 누적(Queue Buildup)을 방지해야 한다.

요구되는 지연시간은 AMR 속도, 제동 성능, 센서 거리, 장애물 행동에 따라 달라진다. 속도가 증가할수록 인지 지연의 매 밀리초 동안 로봇이 이동하는 거리가 증가하여 계획과 제동에 사용할 수 있는 남은 거리가 감소한다. 보행자 역시 로봇의 경로를 향해 움직일 수 있기 때문에 보행자의 운동은 추가적인 불확실성을 발생시킨다. 따라서 검출 거리(Detection Range)와 시간 예산은 서로 독립적으로 최적화하기보다 운용 안전 영역(Operational Safety Envelope)을 기준으로 함께 도출해야 한다.

신뢰도 임계값(Confidence Threshold)도 실제 시스템 동작에 영향을 준다. 높은 임계값은 오탐(False Detection)을 줄일 수 있지만 원거리 또는 부분적으로 가려진 보행자를 제거할 수 있으며, 낮은 임계값은 재현율(Recall)을 높이지만 잘못된 객체를 증가시킬 수 있다. 추적 기능은 시간에 따라 일부 낮은 신뢰도의 관측값을 안정화하여 약하지만 일관된 증거를 유지할 수 있다. 목표 운용 설계 영역(Operational Design Domain, ODD)에 대해 충분히 검증한다면 클래스 또는 거리에 따라 서로 다른 임계값을 적용할 수도 있다.

평가는 AMR이 실제로 운용될 조건을 재현해야 한다. 시험 시나리오(Test Scenario)에는 주차 차량 뒤에서 갑자기 나타나 횡단하는 보행자, 로봇의 경로를 따라 걷는 사람, 접근하거나 멀어지는 차량, 횡방향으로 이동하는 자전거, 인프라에 부분적으로 가려진 트럭, 주행 가능 영역 경계 주변의 정지 차량 등을 포함할 수 있다. 주간, 야간, 날씨, 거리, 속도, 센서 성능 저하 조건을 함께 포함하여 이상적인 장면만을 기준으로 성능을 추정하지 않도록 해야 한다.

평가 지표(Metric)는 인지 성능과 실제 운용 결과를 연결해야 한다. 클래스별 정밀도(Class-Specific Precision)와 재현율은 여전히 유용하지만, 3차원 위치 오차, 속도 오차, 추적 연속성(Track Continuity), 검출 거리, 가림 이후 복구(Occlusion Recovery), 지연시간, 지터(Jitter)도 추가적인 시스템 수준 정보를 제공한다. 거리와 객체 종류별로 성능을 구분하면 보행자와 차량이 즉각적인 충돌 위험이 되기 전에 로봇이 충분히 신뢰할 수 있는 상태 정보를 확보하는지 판단할 수 있다.

기록된 운용 데이터(Recorded Operational Data)는 배포 이후의 지속적인 개선(Continuous Improvement)을 지원할 수 있다. ROS 2 bag 기록을 통해 어려운 이벤트 전후의 동기화된 센서 데이터, 위치 추정, 검출 결과, 추적 상태, 예측 결과, 진단 정보를 보존할 수 있다. 이후 미검출(False Negative), 오탐(False Positive), 불확실한 객체, 추적 단절(Track Fragmentation), 사람의 개입(Human Intervention)을 식별하여 라벨링, 재학습(Retraining), 회귀 시험(Regression Testing), 통제된 모델 업데이트(Controlled Model Update)를 위한 데이터 파이프라인으로 다시 전달할 수 있다.

따라서 실제 제품 수준의 실외 AMR 인지 시스템(Production Outdoor AMR Perception System)은 객체 검출을 독립적인 신경망 작업으로 다루기보다 의미적 검출(Semantic Detection), 3차원 기하 정보, 다중 객체 추적, 속도 추정, 운동 예측, 불확실성, 실시간 소프트웨어 통합을 결합해야 한다. 최종 객체 상태(Object State)는 공간적으로 일관되고, 시간적으로 최신 상태를 유지하며, 충돌 평가(Collision Assessment), 행동 계획(Behavior Planning), 궤적 생성(Trajectory Generation)에 사용할 수 있을 만큼 충분한 신뢰성을 제공해야 한다.

이 보행자 및 차량 검출 사례(Pedestrian and Vehicle Detection Case)는 검출기 아키텍처에서 실제 배포 가능한 인지 시스템으로 이어지는 이 장 전체의 흐름을 보여준다. 센서 관측은 객체 검출로 변환되고, 검출 결과는 지속적인 추적 상태가 되며, 추적 상태에는 속도와 예측 운동 정보가 추가되고, 희귀 객체 또는 미지 객체 역시 계속 표현되며, ROS 2는 이러한 상태를 제어된 지연시간 예산 내에서 전달한다. 최종 목표는 사람, 차량 및 기타 물리적 에이전트 사이에서 AMR이 안전하게 주행할 수 있도록 실외 환경의 동적 표현(Dynamic Representation)을 지속적으로 갱신하는 것이다.
