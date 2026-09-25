**Volume 12. Autonomous Driving Software**

# Chapter 01. Autonomous Driving Fundamentals

## 01.01. Autonomous Driving Levels SAE L0 to L5 for AMR

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

자율주행 단계(Autonomous Driving Levels)는 환경 인지(Sensing), 의사결정(Decision Making), 차량 제어(Vehicle Control)에 대한 책임이 인간 운전자(Human Operator)와 자동주행 시스템(Automated Driving System) 사이에서 어떻게 분담되는지를 체계적으로 설명하는 방법이다. SAE J3016 프레임워크(Framework)는 레벨 0(Level 0)부터 레벨 5(Level 5)까지 여섯 단계를 정의한다. 본래 도로 차량(Road Vehicle)을 위해 개발되었지만, 인간과 기계의 책임을 구분하는 기본 개념은 야외 환경에서 운용되는 자율이동로봇(Autonomous Mobile Robot, AMR)을 설계할 때에도 유용하게 적용할 수 있다.

레벨 0(Level 0)에서는 기계가 동적 주행 작업(Dynamic Driving Task)을 지속적으로 수행하지 않는다. 자동화 기능(Automated Function)이 경고, 비상 개입(Emergency Intervention), 제한적인 운전 지원을 제공할 수 있지만, 주변 환경을 관찰하고 이동을 제어하는 책임은 인간에게 있다. AMR에서는 충돌 경고(Collision Warning), 근접 센서(Proximity Sensor), 비상 제동(Emergency Braking)이 운전자를 보조하지만 지속적인 주행 책임을 담당하지 않는 원격조작(Teleoperation) 또는 수동 주행(Manual Driving)에 해당한다.

레벨 1(Level 1)은 특정 제어 기능(Control Function)을 지속적으로 자동화하지만, 나머지 주행 작업에 대해서는 여전히 인간이 책임을 갖는다. 도로 차량에서는 종방향 제어(Longitudinal Control) 또는 횡방향 제어(Lateral Control) 중 하나를 자동화할 수 있다. AMR 관점에서는 로봇이 속도를 자동으로 조절하거나, 사전에 정의된 방향을 유지하거나, 단순한 경로를 추종하면서 운영자가 인지, 장애물, 교차로, 경로 변경 및 비정상적인 환경 조건을 감독하는 형태로 해석할 수 있다.

레벨 2(Level 2)는 여러 자동 제어 기능(Automated Control Function)을 결합하여 시스템이 경로를 따라 이동하면서 동시에 속도를 조절할 수 있도록 한다. 그러나 환경 모니터링(Environment Monitoring)과 감독 책임(Supervisory Responsibility)은 여전히 인간 운영자에게 있다. 이 단계의 야외 AMR은 지도화된 경로를 추종하고 조향과 속도를 제어하며 감지된 장애물 앞에서 정지할 수 있지만, 인지 또는 경로계획이 신뢰하기 어려운 상황에서는 운영자의 지속적인 감독과 즉각적인 개입이 필요하다.

레벨 3(Level 3)은 조건부 자동화(Conditional Automation)를 의미한다. 자동화 시스템은 정의된 운용 조건 안에서 전체 동적 주행 작업(Dynamic Driving Task)을 수행하고 주변 환경도 모니터링하지만, 시스템이 처리할 수 없는 조건에 도달하면 인간의 개입을 요청할 수 있다. AMR에 적용하면 로봇은 지원되는 환경 내에서 장애물을 자율적으로 인지하고, 자기 위치를 추정하며, 궤적을 계획하고, 제어를 실행하는 동시에 인수 상황(Takeover Situation)에 대비한 명확한 폴백 절차(Fallback Procedure)를 유지한다.

따라서 레벨 2(Level 2)에서 레벨 3(Level 3)으로의 전환은 단순히 더 우수한 인지 알고리즘(Perception Algorithm)을 추가하는 것보다 훨씬 중요한 의미를 갖는다. 자동화 기능이 활성화된 동안 운용 환경을 모니터링하는 책임이 인간 감독자에서 자동화 시스템으로 이동하기 때문이다. 이를 위해서는 신뢰성 높은 객체 검출(Object Detection), 위치추정(Localization), 주행가능성 추정(Traversability Estimation), 행동계획(Behavior Planning), 궤적생성(Trajectory Generation), 시스템 진단(System Diagnostics), 그리고 로봇이 자율주행 능력의 경계에 접근하고 있는지를 판단하는 메커니즘이 필요하다.

레벨 4(Level 4)는 정의된 운용설계영역(Operational Design Domain, ODD) 안에서의 고도 자동화(High Automation)를 의미한다. 해당 영역에서 운용되는 동안 자동화 시스템은 인간이 폴백 응답(Fallback Response)을 제공할 것을 기대하지 않고 주행 작업을 수행할 수 있다. 지속적인 운행이 불가능해질 경우 시스템 자체가 안전 상태(Safe State) 또는 최소위험상태(Minimal-Risk Condition)로 자율적으로 전환해야 한다. 이러한 개념은 지리적 또는 운용적으로 제한된 시설에 배치되는 상업용 야외 AMR에 특히 중요하다.

따라서 야외 AMR은 가능한 모든 공공 환경을 주행할 능력이 없어도 레벨 4와 유사한(Level-4-like) 운용을 달성할 수 있다. AMR의 운용설계영역(ODD)은 물류 야드(Logistics Yard), 산업단지(Industrial Campus), 항만(Port), 건설현장(Construction Site), 농경지(Agricultural Field), 통제된 보행자 구역(Controlled Pedestrian Area) 등을 정의할 수 있으며, 허용 속도, 경사도, 노면 상태, 기상 조건, 조도 범위, 통신 조건 및 위치추정 가용성 등을 함께 규정할 수 있다. 이러한 경계가 명확하게 정의되고 검증되어야만 자율성에 대한 주장이 의미를 갖는다.

예를 들어 보안 순찰 AMR(Security Patrol AMR)은 지도화된 산업단지 내부에서 예정된 임무를 자율적으로 수행하고, 도로와 보행로를 주행하며, 보행자와 차량을 회피하고, 일시적인 장애물을 우회하도록 경로를 재계획하며, 충전소로 복귀하고, 중대한 고장이 발생했을 때 안전 상태로 전환할 수 있다. 로봇이 선언된 운용설계영역(ODD) 안에서 즉각적인 인간 개입에 의존하지 않고 임무를 완료하거나 안전하게 종료할 수 있다면 이러한 동작은 레벨 4와 유사한 특성을 갖는다.

레벨 5(Level 5)는 낮은 자동화 단계에서 적용되는 제한과 비교할 수 있는 특정 주행 영역에 의존하지 않는 완전 자동화(Full Automation)를 의미한다. 개념적으로 시스템은 인간 운전자가 합리적으로 운전할 수 있는 모든 조건에서 전체 주행 작업을 수행할 수 있어야 한다. 이를 로봇에 직접 적용하는 것은 쉽지 않다. AMR은 매우 다양한 지형, 비정형 공간(Unstructured Space), 극한 기상, 센서 성능 저하, 비정상적인 객체 및 임무별 물리적 제약과 마주하기 때문이다.

따라서 레벨 5(Level 5)는 단순히 더 정확한 레벨 4 AMR로 해석해서는 안 된다. 이는 환경적·운용적 일반성(Environmental and Operational Generality)이 근본적으로 확장되는 것을 의미한다. 레벨 5와 유사한(Level-5-like) 이동로봇은 익숙하지 않은 환경을 이해하고, 이전에 경험하지 못한 상황에 행동을 적응시키며, 불확실한 지형과 동적 객체(Dynamic Agent)를 추론하고, 예상하지 못한 고장으로부터 복구하며, 사전에 정의된 지도, 경로 또는 좁게 규정된 환경 조건에 의존하지 않고 안전한 운용을 유지할 수 있어야 한다.

SAE 자동화 단계(SAE Automation Levels)는 인공지능 지능 수준(AI Intelligence), 인지 정확도(Perception Accuracy), 하드웨어 성능(Hardware Performance)을 직접 측정하는 척도가 아니라 주행 작업의 책임 할당(Allocation of Driving Task)을 설명한다. 강력한 GPU, 다수의 라이다(LiDAR), 카메라(Camera), 레이더(Radar), GNSS RTK 및 정교한 신경망(Neural Network)을 탑재한 로봇이라고 해서 자동으로 레벨 4가 되는 것은 아니다. 핵심은 명확하게 정의된 운용 조건과 시스템 고장 상황에서 어느 주체가 인지, 의사결정, 제어, 감독 및 폴백을 수행하는가이다.

이러한 구분은 AMR에서 특히 중요하다. 자율주행(Autonomous Navigation), 장애물 회피(Obstacle Avoidance), 무인 운용(Unmanned Operation)과 같은 용어가 자율성을 설명하기 위해 자주 사용되기 때문이다. 그러나 이러한 기능만으로 자동화 단계를 정의할 수는 없다. 로봇이 정상 상태에서는 자율적으로 주행하더라도 위치추정 실패 또는 알 수 없는 장애물이 나타날 때마다 원격 운영자(Remote Operator)에게 의존한다면 실질적인 자율주행 아키텍처(Autonomy Architecture)는 성능저하 모드(Degraded Mode), 폴백 동작(Fallback Behavior), 복구 메커니즘(Recovery Mechanism)을 함께 포함해야 한다.

원격 감독(Remote Supervision) 역시 신중하게 해석해야 한다. 레벨 4와 유사한 AMR은 인간이 지속적으로 동적 주행 작업을 수행하지 않으면서도 차량관제시스템(Fleet Management System) 또는 원격운영센터(Remote Operations Center)와 통신할 수 있다. 운영자는 임무를 할당하고 상태를 관찰하며 예외적인 행동을 승인하거나 복구를 지원할 수 있지만, 실시간 안전 이동(Real-Time Safe Motion)에 대한 책임은 온보드 자율주행 시스템(Onboard Autonomy)에 있다. 반면 지속적인 원격 운전(Continuous Remote Driving)은 고도 자율주행이 아니라 인간 제어(Human Control)에 해당한다.

야외 AMR에서 L0부터 L4까지의 발전 과정은 인간 제어(Human Control)에서 기계 자율성(Machine Autonomy)으로 운용 책임이 점진적으로 이전되는 과정으로 이해할 수 있다. 수동 제어(Manual Control)는 보조 제어(Assisted Control), 감독형 자율주행(Supervised Navigation), 조건부 자율주행(Conditional Autonomy)을 거쳐 기계가 폴백까지 관리하는 영역 제한형 자율운용(Domain-Bounded Autonomous Operation)으로 발전한다. 각 단계의 전환은 인지 범위, 위치추정 무결성(Localization Integrity), 계획 강건성(Planning Robustness), 고장 감지, 중복성(Redundancy), 검증 및 런타임 안전 감독(Runtime Safety Supervision)에 대한 요구를 증가시킨다.

이러한 발전 과정은 자연스럽게 자율주행 단계와 전체 자율주행 소프트웨어 스택(Autonomous-Driving Software Stack)을 연결한다. 센서 처리(Sensor Processing)는 신뢰할 수 있는 관측 정보를 생성하고, 주행가능영역 추정(Drivable-Area Estimation)은 이동 가능한 지형을 식별하며, 객체 검출 및 추적(Object Detection and Tracking)은 동적 객체를 표현한다. 행동계획(Behavior Planning)은 적절한 행동을 결정하고, 궤적생성(Trajectory Generation)은 실행 가능한 이동 경로를 생성하며, 제어(Control)는 궤적을 실제 물리적 구동으로 변환한다. 안전 모니터링(Safety Monitoring)과 위치추정(Localization)은 이러한 모든 기능에 핵심적인 제약조건을 제공한다.

높은 자동화 수준에서는 고장 관리(Failure Management)의 중요성도 증가한다. 낮은 단계에서는 인간이 자동화 기능의 많은 한계를 보완할 수 있지만, 높은 단계에서는 시스템 자체가 센서 고장, 위치추정 성능 저하, 차단된 경로, 액추에이터 이상(Actuator Abnormality), 통신 손실 및 지원되는 운용 영역을 벗어난 환경 조건을 감지해야 한다. 이후 시스템은 감속, 안전 정지, 운용 모드 변경, 지원 요청 또는 최소위험기동(Minimal-Risk Maneuver) 수행과 같은 적절한 대응을 선택해야 한다.

따라서 소프트웨어 아키텍처(Software Architecture)는 정상 자율주행(Nominal Autonomy)과 안전 메커니즘(Safety Mechanism)을 구분해야 한다. 기본 인지-계획-제어 파이프라인(Perception-Planning-Control Pipeline)은 임무 수행을 담당하고, 독립적인 모니터링 기능은 그 출력이 계속 신뢰 가능하고 안전한지를 평가한다. 워치독(Watchdog), 상태 모니터링(Health Monitoring), 중복 센싱(Redundant Sensing), 폴백 계획(Fallback Planning), 비상 제동(Emergency Braking), 안전 케이지(Safety Cage)는 하나의 자율주행 기능 실패가 곧바로 위험한 물리적 동작으로 이어지는 것을 방지할 수 있다.

자율주행 수준이 높아질수록 운용설계영역(Operational Design Domain, ODD)의 정의도 더욱 중요해진다. ODD는 자율주행 기능이 어디에서, 언제, 어떤 조건에서 운용되도록 설계되었는지를 규정한다. 야외 AMR의 경우 지리적 경계, 도로와 지형 특성, 최대 경사도, 장애물 종류, 보행자 밀도, 기상, 조도, 속도 범위, GNSS 가용성, 지도 품질, 무선 통신 상태 및 다른 기계와 허용되는 상호작용 등이 중요한 ODD 구성 요소가 된다.

가능한 경우 운용설계영역(ODD)은 기계가 직접 관측할 수 있는 형태(Machine-Observable)로 정의되어야 한다. 로봇이 현재 조건의 충족 여부를 판단할 수 없다면 허용 가능한 기상 또는 위치추정 조건에서만 운용한다고 문서에 기록하는 것만으로는 충분하지 않다. 런타임 모니터(Runtime Monitor)는 위치추정 신뢰도, 센서 가시성(Sensor Visibility), 지형 상태, 통신 건전성(Communication Health), 액추에이터 상태 및 환경의 심각도를 추정하여 로봇이 자신의 운용 가정이 더 이상 유효하지 않은 시점을 인식할 수 있도록 해야 한다.

AMR에 대한 SAE 단계의 적용은 궁극적으로 로봇 전용 안전 표준(Robot-Specific Safety Standard)이나 실제 배치 요구사항을 대체하는 것이 아니라 공학적 유추(Engineering Analogy)로 이해해야 한다. SAE 단계는 책임과 폴백을 분석하기 위한 유용한 개념적 언어를 제공하지만, 실제 AMR 설계에서는 기계 안전(Machinery Safety), 산업 환경, 로봇 고유 위험요소, 차량군 운용(Fleet Operation), 사이버보안(Cybersecurity), 원격 지원(Remote Assistance) 및 응용 분야별 위험 통제(Risk Control)를 추가적으로 고려해야 한다.

따라서 시스템 개발(System Development)의 가장 유용한 목표는 단순히 로봇에 숫자로 된 자율주행 등급을 부여하는 것이 아니다. 엔지니어는 자동화 기능, 운용설계영역(ODD), 인간의 책임, 폴백 전략(Fallback Strategy), 최소위험상태(Minimal-Risk Condition), 고장 대응(Failure Response), 성능 한계 및 해당 능력을 입증하는 검증 증거(Validation Evidence)를 명확하게 정의해야 한다. 이를 통해 소프트웨어 요구사항, 시뮬레이션 시나리오, 필드 테스트(Field Testing), 안전 논증(Safety Argument)에 직접 연결할 수 있는 검증 가능한 자율성 정의를 구축할 수 있다.

본 권(Volume)에서는 이러한 해석을 야외 AMR을 위한 자율주행 소프트웨어 아키텍처(Autonomous-Driving Software Architecture)의 기초로 사용한다. 이후 내용은 시스템 과제(System Challenges)와 운용설계영역(ODD) 정의에서 시작하여 규제 고려사항, 핵심성과지표(Key Performance Indicators, KPI), 고장안전(Fail-Safe) 원칙, 디스인게이지먼트 분석(Disengagement Analysis), 데이터 기반 개발(Data-Driven Development)을 거쳐 인지, 계획, 제어, 위치추정 및 안전 검증(Safety Validation)으로 확장된다. 더 넓은 로보틱스 소프트웨어 체계(Robotics Software Structure)에서는 이러한 자율주행 소프트웨어가 시뮬레이션(Simulation), 동시적 위치추정 및 지도작성(SLAM), 인지(Perception), 내비게이션(Navigation), 차량군 지능(Fleet Intelligence), 로봇 학습(Robot Learning)과 연결되는 핵심 영역으로 구성된다.

## 01.02. AV System Challenges Perception Planning Control

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

자율주행 차량(Autonomous Vehicle)과 자율이동로봇(Autonomous Mobile Robot, AMR) 시스템은 불확실한 물리적 환경을 안전하고 실행 가능한 행동으로 변환해야 한다. 근본적인 과제는 하나의 인지(Perception), 계획(Planning), 제어(Control) 알고리즘에 있는 것이 아니라 이러한 기능들이 지속적으로 상호작용하는 데 있다. 센서(Sensor)는 환경을 관측하고, 인지(Perception)는 의미 있는 객체와 지형 정보를 추출하며, 계획(Planning)은 차량이 무엇을 해야 하는지를 결정하고, 제어(Control)는 계획된 움직임을 실제 물리적 이동으로 변환한다. 하나의 기능에서 발생한 오류나 지연은 전체 자율주행 파이프라인(Autonomy Pipeline)으로 전파될 수 있다.

인지(Perception)는 물리적 환경이 부분적으로만 관측 가능하다는 점에서 첫 번째 주요 과제가 된다. 카메라(Camera), 라이다(LiDAR), 레이더(Radar), GNSS, 관성 센서(Inertial Sensor)는 서로 다른 불완전한 측정값을 제공한다. 조명 변화, 비, 먼지, 반사, 가림(Occlusion), 센서 잡음(Sensor Noise), 동적 객체(Dynamic Object)는 신뢰성을 저하시킬 수 있다. 따라서 야외 AMR은 여러 센싱 방식(Sensing Modality)을 결합하고 개별 센서의 성능이 저하되더라도 유용한 환경 표현(Environmental Representation)을 유지해야 한다. 인지는 객체 검출(Object Detection)뿐만 아니라 신뢰도(Confidence), 불확실성(Uncertainty), 관측 정보의 품질에 대한 정보도 제공해야 한다.

객체 이해(Object Understanding)는 사전에 정의된 클래스를 검출하는 것보다 더욱 복잡하다. 자율 시스템은 객체의 위치(Position), 방향(Orientation), 속도(Velocity), 움직임 경향(Motion Tendency)을 추정하는 동시에 통과 가능한 지형(Traversable Terrain)과 관련 장애물을 구분해야 한다. 보행자, 차량, 시설물, 식생, 연석(Curb), 건설 자재 및 비정상적인 객체가 학습 과정에서 충분히 표현되지 않았던 위치나 형태로 나타날 수 있다. 따라서 롱테일(Long-Tail) 및 이전에 관측되지 않은 상황을 처리하기 위해서는 강건한 인지 전략(Robust Perception Strategy), 센서 융합(Sensor Fusion), 시간적 추적(Temporal Tracking), 적절한 불확실성 처리가 필요하다.

계획(Planning)은 환경에 대한 이해를 행동으로 변환해야 하기 때문에 또 다른 종류의 어려움을 제공한다. 계획기는 현재 경로, 장애물, 도로 또는 지형 제약, 차량 크기, 속도, 가속도, 정지 거리(Stopping Distance), 그리고 동적 객체와의 상호작용을 고려해야 한다. 동일한 인지 결과도 임무 목표와 환경 상황에 따라 서로 다른 행동으로 이어질 수 있다. 따라서 보안 순찰(Security Patrol), 인프라 검사(Infrastructure Inspection), 물류 운송(Logistics Transport)을 수행하는 야외 AMR은 동일한 물리적 환경에서도 서로 다른 행동 정책(Behavior Policy)을 필요로 할 수 있다.

행동계획(Behavior Planning)은 세부적인 궤적(Trajectory)을 생성하기 전에 수행해야 할 정성적인 행동(Qualitative Action)을 결정한다. 시스템은 계속 전진하거나, 감속하거나, 정지하거나, 양보하거나, 장애물을 회피하거나, 경로를 변경하거나, 복구 행동(Recovery Behavior)에 진입할 수 있다. 규칙 기반 로직(Rule-Based Logic), 유한상태기계(Finite-State Machine), 의사결정 트리(Decision Tree), 최적화(Optimization), 머신러닝(Machine Learning), 하이브리드 방식(Hybrid Approach) 등을 사용할 수 있다. 안전이 중요한 응용 분야에서는 복잡하고 불확실한 상황을 처리하면서도 검증(Verification)을 지원할 수 있을 정도로 예측 가능한 계획기가 필요하다.

궤적생성(Trajectory Generation)은 선택된 행동을 실행 가능한 경로와 시간 시퀀스로 변환한다. 수학적으로 유효한 경로가 반드시 로봇이 실행할 수 있는 경로를 의미하지는 않는다. 곡률(Curvature), 속도(Velocity), 가속도(Acceleration), 조향 한계(Steering Limit), 타이어 제약(Tire Constraint), 지형 조건(Terrain Condition), 액추에이터 한계(Actuator Limitation), 차량 동역학(Vehicle Dynamics)을 고려해야 한다. 또한 궤적은 지속적으로 변화하는 환경과 일치해야 한다. 따라서 자율 시스템은 경로를 한 번 생성한 후 계속 유효하다고 가정하기보다 빈번한 재계획(Re-planning)과 궤적 검증(Trajectory Validation)을 수행해야 한다.

제어(Control)는 계획된 움직임과 실제 물리적 움직임 사이의 차이를 줄이는 역할을 담당한다. 고품질의 궤적이라도 액추에이터 지연(Actuator Delay), 휠 슬립(Wheel Slip), 부정확한 차량 모델(Vehicle Model), 변화하는 적재량(Payload), 불균일한 지형 또는 외란(Disturbance)으로 인해 실패할 수 있다. 따라서 종방향 제어(Longitudinal Control)와 횡방향 제어(Lateral Control)는 충분히 낮은 지연시간(Latency)으로 동작하면서 모델 불확실성(Model Uncertainty)을 보상해야 한다. 야외 AMR에서는 접지력 변화(Traction Variation)와 지형과의 상호작용이 이러한 문제를 더욱 중요하게 만든다. 명령된 움직임과 실제 움직임 사이의 관계가 빠르게 변할 수 있기 때문이다.

인지(Perception), 계획(Planning), 제어(Control) 사이의 인터페이스는 또 다른 주요 시스템 과제를 만든다. 각 모듈은 서로 다른 주파수(Frequency)로 동작하고 서로 다른 좌표계(Coordinate Frame)를 사용하며 서로 다른 처리 지연(Processing Delay)을 경험할 수 있다. 센서 타임스탬프(Sensor Timestamp), 위치추정 업데이트(Localization Update), 예측 시간 범위(Prediction Horizon), 제어 주기(Control Cycle)는 시간적으로 일관성을 유지해야 한다. 정확하지만 수백 밀리초 정도 오래된 인지 결과는 근처의 보행자나 차량이 빠르게 이동하는 상황에서 안전하지 않은 계획 결정을 초래할 수 있다.

계산 자원(Computational Resource) 역시 자율 시스템 설계를 제한한다. 고해상도 카메라 처리, 3차원 라이다 인지(3D LiDAR Perception), 레이더 처리, 다중 객체 추적(Multi-Object Tracking), 위치추정(Localization), 계획(Planning), 신경망 추론(Neural Network Inference)은 CPU, GPU, 메모리, 통신 대역폭(Communication Bandwidth)을 서로 경쟁적으로 사용할 수 있다. 따라서 야외 AMR은 명확한 지연시간 예산(Latency Budget)과 계산 예산(Computational Budget)을 필요로 한다. 하드웨어 가속(Hardware Acceleration), 효율적인 데이터 이동(Data Movement), 비동기 처리(Asynchronous Processing), 모델 최적화(Model Optimization), 실시간(Real-Time) 및 비실시간(Non-Real-Time) 작업의 적절한 분리는 중요한 아키텍처 고려사항이 된다.

강건성(Robustness)은 정상적인 운용 조건을 넘어 확장되어야 한다. 자율 시스템은 센서 측정값이 서로 일치하지 않거나, 위치추정 신뢰도가 저하되거나, 계획된 경로가 차단되거나, 환경 조건이 지원되는 운용 범위를 초과한 경우를 인식해야 한다. 시스템은 겉보기에는 확신에 차 있지만 잘못된 행동을 생성하기보다는 안전 아키텍처(Safety Architecture)에 따라 속도를 낮추거나, 정지하거나, 재계획하거나, 운용 모드를 변경하거나, 지원을 요청해야 한다. 이를 위해서는 자율주행 스택 전체에 걸친 상태 모니터링(Health Monitoring)과 명시적인 성능저하 모드(Degraded-Mode) 동작이 필요하다.

운용설계영역(Operational Design Domain, ODD)은 시스템 복잡성을 관리하기 위한 중요한 프레임워크를 제공한다. 자율 AMR은 모든 가능한 주행 문제를 동시에 해결할 필요가 없으며, 의도된 운용 환경이 명확하게 정의되어 있다면 그 범위 내에서 자율성을 구현할 수 있다. 지리적 경계(Geographic Boundary), 지형 유형(Terrain Type), 기상 조건(Weather Condition), 조도(Illumination), 속도 범위(Speed Range), 위치추정 가용성(Localization Availability), 지도 품질(Map Quality), 통신 가정(Communication Assumption), 보행자 또는 차량과의 상호작용(Interaction)을 모두 ODD의 일부로 정의할 수 있다. 이후 자율 시스템은 현재 조건이 이러한 가정과 여전히 호환되는지를 판단해야 한다.

가장 어려운 상황은 인지, 계획, 제어가 ODD의 경계에서 불확실한 조건과 동시에 상호작용할 때 발생한다. 인지 시스템이 낮은 신뢰도로 객체를 인식하고, 동시에 위치추정이 불안정해지며, 계획기는 회피할 수 있는 여유 공간이 매우 적은 상황을 예로 들 수 있다. 이러한 상황에서는 각 모듈을 독립적으로 처리하는 것만으로는 충분하지 않다. 전체 시스템은 신뢰도, 시간, 실행 가능성(Feasibility), 안전성을 함께 고려하고 불완전한 정보에도 불구하고 허용 가능한 상태를 유지할 수 있는 대응을 선택해야 한다.

이러한 이유로 자율주행 소프트웨어(Autonomous-Driving Software)는 독립적인 알고리즘의 집합이 아니라 폐루프 시스템(Closed-Loop System)으로 개발되어야 한다. 센서 데이터는 전처리(Preprocessing)와 인지(Perception)를 거쳐 위치추정(Localization)과 환경 표현(Environmental Representation)으로 전달되고, 계획(Planning)은 제어(Control)에 의해 실행되는 궤적을 생성한다. 이후 실제 차량 상태와 새로운 센서 관측값이 다음 주기(Natural Cycle)의 입력으로 다시 사용된다. 전체 폐루프가 안정적이고 신뢰성 있게 유지되는지를 판단하기 위해서는 지속적인 모니터링(Monitoring), 검증(Validation), 로깅(Logging), 시뮬레이션(Simulation), 필드 테스트(Field Testing)가 필요하다.

따라서 야외 AMR에서 핵심적인 공학적 목표는 개별 모듈에서 최대 성능을 얻는 것이 아니라 시스템 수준에서 예측 가능한 동작(Predictable System-Level Behavior)을 구현하는 것이다. 인지는 충분히 신뢰할 수 있는 정보를 제공해야 하고, 계획은 적절한 행동을 선택해야 하며, 궤적생성은 물리적으로 실행 가능한 움직임을 생성해야 하고, 제어는 이용 가능한 동적 한계(Dynamic Limit) 내에서 해당 움직임을 실행해야 한다. 안전 메커니즘(Safety Mechanism)은 전체 연결 구조를 모니터링하고 가정이 실패할 경우 폴백 동작(Fallback Behavior)을 제공해야 한다. 이러한 통합적 관점은 이후 본 권에서 다루는 소프트웨어 아키텍처(Software Architecture), 인지(Perception), 계획(Planning), 제어(Control), 안전(Safety), 위치추정(Localization), 검증(Validation) 주제의 기초가 된다.

## 01.03. Operational Design Domain ODD Definition for AMR

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

운용설계영역(Operational Design Domain, ODD)은 자율이동로봇(Autonomous Mobile Robot, AMR)이 운용되도록 설계되고, 검증되며, 운용이 허용되는 구체적인 조건과 경계를 정의한다. AMR에서 ODD는 단순한 지리적 경계 이상의 의미를 갖는다. 자율주행 동작은 지형, 도로 구조, 기상, 조도, 속도, 위치추정, 통신 연결성, 교통 상황 및 임무 조건에 영향을 받기 때문이다. 명확하게 정의된 ODD는 인지(Perception), 계획(Planning), 제어(Control), 안전(Safety) 기능이 신뢰성 있게 동작할 것으로 기대되는 조건을 규정한다.

AMR의 ODD는 로봇이 임무를 수행하도록 의도된 운용 환경에서 시작해야 한다. 일반적인 환경에는 산업단지(Industrial Campus), 물류 야드(Logistics Yard), 항만(Port), 건설현장(Construction Site), 농경지(Agricultural Field), 철도 시설(Railway Facility), 주거 단지(Residential Complex), 통제된 보행자 구역(Controlled Pedestrian Area) 등이 포함된다. 각각의 환경은 서로 다른 물리적 및 행동적 특성을 갖는다. 물류 야드에는 트럭과 컨테이너가 존재할 수 있는 반면, 건설현장에는 불규칙한 지형, 임시 구조물, 먼지 및 빈번하게 변경되는 경로가 존재할 수 있다.

지리적 경계(Geographic Boundary)는 ODD에서 가장 눈에 띄는 구성요소 중 하나이다. 자율운용이 허용되는 영역과 금지되는 영역을 정의해야 한다. 이러한 경계는 지도(Map), 지오펜스(Geofence), 도로 네트워크(Road Network), 시설 구역(Facility Zone), 의미론적 영역(Semantic Region) 등을 사용하여 표현할 수 있다. 그러나 경계를 단순한 정적 지도 객체로만 취급해서는 안 된다. 임시 폐쇄, 공사, 출입 제한 구역 및 운용상의 변경으로 인해 실제 배치 환경에서 유효한 자율운용 영역이 변경될 수 있기 때문이다.

도로 및 지형 특성(Road and Terrain Characteristics)은 야외 AMR에서 동일하게 중요하다. ODD는 포장도로(Paved Road), 보도(Sidewalk), 자갈길(Gravel), 다져진 흙길(Compacted Soil), 잔디(Grass), 경사면(Slope), 연석(Curb), 경사로(Ramp), 계단(Stair) 또는 기타 지형 유형을 규정할 수 있다. 노면 거칠기(Surface Roughness), 최대 경사도(Maximum Slope), 장애물 높이(Obstacle Height), 지상고(Clearance), 접지력(Traction), 주행가능성(Traversability)은 특정 로봇 구성이 안전하게 운용될 수 있는지를 결정할 수 있다. 오프로드 AMR의 경우 지형 능력(Terrain Capability)은 차량 동역학(Vehicle Dynamics), 서스펜션(Suspension), 휠 구성(Wheel Configuration), 적재량(Payload), 가용 접지력과 직접 연결되어야 한다.

기상 및 환경 조건(Weather and Environmental Conditions) 역시 명확하게 표현해야 한다. 비, 눈, 안개, 먼지, 강한 햇빛, 어둠, 바람, 노면의 물 등은 카메라(Camera), 라이다(LiDAR), 레이더(Radar), GNSS 및 차량 접지력에 서로 다른 영향을 줄 수 있다. 따라서 ODD는 단순히 시스템이 야외 운용에 적합하다고 선언하기보다는 허용 가능한 운용 범위를 정의할 수 있다. 런타임 모니터링(Runtime Monitoring)은 실제 환경 조건이 검증된 범위 안에 계속 존재하는지를 판단해야 한다.

조도(Illumination)는 카메라 기반 인지(Camera-Based Perception)에서 특히 중요하다. 주간, 야간, 그림자, 눈부심(Glare), 인공조명 및 급격한 조도 변화는 센서 관측값을 크게 변화시킬 수 있다. 따라서 주간 운용을 중심으로 검증된 시스템이 야간에도 동일한 성능을 제공한다고 자동으로 가정해서는 안 된다. ODD는 조도 조건을 인지 시스템의 능력 및 검증 근거(Validation Evidence)와 연결해야 하며, 특히 카메라가 객체 검출(Object Detection), 의미론적 이해(Semantic Understanding), 주행가능영역 추정(Drivable-Area Estimation)에 사용되는 경우 더욱 중요하다.

속도(Speed)는 또 다른 중요한 ODD 차원(Dimension)이다. 속도가 증가하면 인지, 예측(Prediction), 계획, 제동(Braking)에 사용할 수 있는 시간이 감소하기 때문이다. 통제된 산업 환경에서 저속으로 운용되는 로봇은 고속 야외 플랫폼과 다른 인지 및 계획 특성을 허용할 수 있다. 따라서 최대 속도는 정지거리(Stopping Distance), 장애물 검출 범위(Obstacle Detection Range), 제어 지연(Control Latency), 위치추정 불확실성(Localization Uncertainty), 도로 상태 및 주변 동적 객체(Dynamic Agent)의 예상 행동과 함께 고려해야 한다.

위치추정 및 지도 조건(Localization and Mapping Conditions)도 ODD에 포함되어야 한다. 야외 AMR은 GNSS, RTK, 라이다 위치추정(LiDAR Localization), 비전 위치추정(Visual Localization), 관성 추정(Inertial Estimation) 또는 이러한 기술의 조합을 사용할 수 있다. 시스템은 자율운용에 필요한 위치추정 품질을 정의하고 위치추정이 신뢰하기 어려워지는 조건을 식별해야 한다. GNSS 장애, 다중경로(Multipath), 신호 손실, 지도 품질 저하 또는 충분하지 않은 기하학적 특징(Geometric Feature)은 모두 위치추정 신뢰도를 낮출 수 있으며, 적절한 운용 대응을 유발해야 한다.

통신 연결성(Connectivity)은 특히 차량군(Fleet)으로 운용되는 AMR에서 또 다른 중요한 차원이다. 일부 임무는 차량군 관리 시스템(Fleet Management System)과의 지속적인 통신을 필요로 할 수 있지만, 다른 임무는 일시적인 네트워크 손실이 발생하더라도 안전하게 계속 운용되도록 설계될 수 있다. 따라서 ODD는 연결성이 필요한 기능과 로컬에서 계속 사용할 수 있어야 하는 기능을 구분해야 한다. 강건한 아키텍처(Robust Architecture)는 기술적으로 로컬 폴백(Local Fallback)이 요구되는 경우 즉각적인 물리적 안전이 원격 연결에 의존하지 않도록 해야 한다.

보행자, 차량 및 다른 로봇과의 상호작용(Interaction)도 명확하게 고려해야 한다. 보행자 통행이 제한된 통제된 산업 구역은 캠퍼스, 주거 단지 또는 일반 대중이 이용하는 시설과 다른 상호작용 요구사항을 갖는다. ODD는 예상되는 동적 객체, 이들의 운용 영역, 교통 패턴 및 상호작용 규칙을 설명해야 한다. 이후 인지와 행동계획(Behavior Planning)은 정의된 운용 환경에서 예상되는 객체의 유형과 밀도에 대해 검증되어야 한다.

ODD에는 임무별 제약조건(Mission-Specific Constraint)도 포함해야 한다. 동일한 AMR이라도 서로 다른 운용 조건에서 상당히 다른 작업을 수행할 수 있기 때문이다. 보안 순찰(Security Patrol), 인프라 검사(Infrastructure Inspection), 물류 운송(Logistics Transport), 화물 이동(Cargo Movement), 배송(Delivery) 임무는 서로 다른 경로, 속도, 센서 구성, 정지 정책(Stopping Policy), 상호작용 행동을 요구할 수 있다. 따라서 ODD는 물리적인 로봇뿐만 아니라 실행되는 임무 프로파일(Mission Profile)과 운용 모드(Operational Mode)에도 연결되어야 한다.

유용한 ODD는 이상적인 조건만 설명하는 문서에 그쳐서는 안 된다. 가능한 경우 ODD의 경계는 관측 가능한 시스템 변수(Observable System Variable)와 연결되어야 한다. AMR은 현재 조건이 개발 및 검증 과정에서 사용된 가정과 계속 호환되는지를 지속적으로 추정해야 한다. 예를 들어 위치추정 신뢰도, 센서 상태, 가시성(Visibility), 지형 주행가능성, 통신 상태, 차량 상태, 기상 조건, 지도 일관성(Map Consistency), 이용 가능한 자유 공간(Free Space) 등이 이에 포함될 수 있다.

이는 기계가 관측 가능한 ODD 경계(Machine-Observable ODD Boundary)라는 개념으로 이어진다. "좋은 날씨에서만 운용한다"와 같은 문장은 시스템이 허용 가능한 기상을 판단할 수 있는 측정 기준을 갖고 있지 않다면 구현하기 어렵다. 마찬가지로 "적합한 지형에서 운용한다"는 조건도 경사도, 거칠기, 접지력, 지상고, 휠 슬립(Wheel Slip), 차량 능력 사이의 측정 가능한 관계를 필요로 한다. ODD 가정을 측정 가능한 조건으로 변환하면 런타임 모니터링과 안전 의사결정에 실제로 활용할 수 있다.

ODD 관리는 전체 운용 영역 안에서 발생하는 일시적인 변화도 고려해야 한다. 익숙한 도로라도 주차 차량, 건설 장비, 낙하물, 침수 또는 예상하지 못한 군중으로 인해 차단될 수 있다. 지리적 영역 자체는 여전히 승인된 ODD에 포함되어 있을 수 있지만, 국부적인 운용 조건은 자율주행에 필요한 가정을 더 이상 만족하지 않을 수 있다. 따라서 로봇은 전역적인 ODD 소속(Global ODD Membership)과 순간적인 운용 유효성(Instantaneous Operational Validity)을 구분해야 한다.

ODD 경계에 접근하거나 이를 초과했을 때의 대응은 배치 전에 정의되어야 한다. 상황에 따라 로봇은 속도를 낮추거나, 정지하거나, 대체 경로를 선택하거나, 성능저하 모드(Degraded Mode)에 진입하거나, 안전한 위치로 복귀하거나, 원격 지원(Remote Assistance)을 요청하거나, 최소위험상태(Minimal-Risk Condition)를 실행할 수 있다. 적절한 대응은 ODD 위반의 심각도와 지속시간에 따라 달라진다. 중요한 것은 원래의 임무가 아직 완료되지 않았다는 이유만으로 정상적인 자율주행을 계속해서는 안 된다는 점이다.

ODD 정의는 안전 아키텍처(Safety Architecture)와 밀접하게 연결된다. ODD는 자율 기능이 유효하다고 판단되는 조건을 설정하기 때문이다. 인지, 계획 및 제어는 ODD 가정을 기준으로 설계되고 시험되며, 안전 모니터링(Safety Monitoring)은 운용 중 이러한 가정이 계속 유효한지를 판단한다. 가정이 실패하면 안전 아키텍처는 자율주행 스택이 검증된 능력 범위를 벗어난 행동을 생성하지 않도록 해야 한다.

ODD 경계는 운용 데이터(Operational Data)와 검증 결과를 통해 지속적으로 발전해야 한다. 실제 현장 운용은 초기 개발 과정에서 충분히 표현되지 않았던 환경 조건, 객체 유형, 지형 특성 또는 상호작용 패턴을 발견할 수 있다. 이러한 관측 결과는 분석되어 업데이트된 시나리오, 테스트 케이스, 지도, 운용 제약조건 및 인지 데이터셋에 반영될 수 있다. 이를 통해 ODD 관리는 일회성 문서화 작업이 아니라 폐루프 개발 프로세스(Closed-Loop Development Process)의 일부가 된다.

야외 AMR 플랫폼에서 ODD는 자율 시스템과 운용 환경 사이의 계약(Contract)으로 볼 수 있다. 환경은 정의된 범위 안에서 조건을 제공하고, 로봇은 해당 범위에 맞게 설계되고 검증된 자율주행 동작을 제공한다. 이 계약의 강도는 경계를 얼마나 정확하게 정의하는지, 그 경계를 얼마나 안정적으로 관측할 수 있는지, 그리고 조건이 범위를 벗어날 때 로봇이 얼마나 안전하게 대응하는지에 달려 있다.

실질적인 목표는 ODD를 불필요하게 좁게 만드는 것이 아니라 명확하고, 측정 가능하며, 시험 가능하고, 증거를 통해 확장할 수 있도록 만드는 것이다. 잘 설계된 ODD는 엔지니어가 자율주행 동작이 정확히 어디에서 작동할 것으로 예상되는지, 어디에서 추가 검증이 필요한지, 그리고 어디에서 폴백(Fallback) 또는 인간 지원(Human Assistance)이 필요한지를 명확하게 식별할 수 있도록 한다. 이는 야외 AMR 자율주행을 체계적으로 개발하기 위한 기반을 제공하며, 동시에 시스템이 실제로 지원할 수 있는 조건을 넘어서는 광범위한 능력 주장을 방지한다.

## 01.04. Outdoor AMR vs On Road AV Requirements Comparison

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

야외 자율이동로봇(Outdoor AMR)과 도로 주행 자율주행 차량(On-Road Autonomous Vehicle)은 인지(Perception), 위치추정(Localization), 계획(Planning), 궤적생성(Trajectory Generation), 제어(Control)를 기반으로 하는 공통적인 자율주행 파이프라인(Autonomy Pipeline)을 공유하지만, 운용 요구사항은 상당히 다르다. 도로 주행 자율주행 차량은 주로 구조화된 도로 네트워크(Structured Road Network), 표준화된 교통 규칙(Standardized Traffic Rules), 예측 가능한 차선 형상(Lane Geometry), 도로 이용자(Road User)와의 상호작용을 중심으로 설계된다. 반면 야외 AMR은 산업단지, 물류 야드, 항만, 건설현장, 농경지 또는 다양한 지형이 혼재된 환경에서 운용될 수 있으며, 도로 구조가 불완전하거나 존재하지 않을 수도 있다. 이러한 차이는 시스템 아키텍처, 센싱, 차량 동역학, 계획, 안전 및 검증 요구사항에 직접적인 영향을 준다.

도로 주행 자율주행 차량은 일반적으로 차선(Lane), 교통 표지판(Traffic Sign), 신호등(Traffic Signal), 도로 경계(Road Boundary), 운전 관행(Driving Convention)이 강력한 구조적 정보를 제공하는 환경에서 운용된다. 자율주행 스택(Autonomy Stack)은 실행 가능한 행동을 결정할 때 표준화된 도로 형상과 기존의 교통 행동 패턴을 활용할 수 있다. 야외 AMR은 이러한 규칙성이 없는 환경을 자주 경험한다. 하나의 임무 중에도 포장도로, 보도, 자갈, 흙, 잔디, 경사로 또는 거친 지형 사이를 이동할 수 있다. 따라서 AMR 자율주행은 주행가능성(Traversability), 국부 환경 이해(Local Environmental Understanding), 지형 추정(Terrain Estimation), 불규칙한 운용 공간에 대한 적응에 더 큰 비중을 두어야 한다.

차량 플랫폼 자체도 또 다른 주요 차이를 만든다. 승용차와 도로 차량은 비교적 높은 속도의 운용, 도로 안정성, 제동 성능, 탑승자 안전, 장거리 이동에 최적화되어 있다. 야외 AMR은 저속 또는 중속 운용, 좁은 공간에서의 기동성, 화물 운송, 장시간 운용, 제한된 시설 내 운용에 최적화되는 경우가 많다. 따라서 차체 크기, 휠베이스(Wheelbase), 조향 메커니즘(Steering Mechanism), 서스펜션(Suspension), 지상고(Ground Clearance), 타이어 특성(Tire Characteristics), 적재 하중 분포(Payload Distribution)는 기존 도로 차량과 상당히 다를 수 있다. 이러한 기계적 특성은 자율주행 및 제어 아키텍처에 직접 반영되어야 한다.

센싱 요구사항(Sensing Requirements) 역시 환경에 대한 가정이 다르기 때문에 차이가 발생한다. 도로 주행 자율주행 차량은 일반적으로 차선, 차량, 보행자, 교통 신호, 표지판 및 도로 형상을 장거리에서 인지해야 한다. 야외 AMR은 장애물이 로봇의 바로 앞에 나타날 수 있고 기존 도로 객체 분류에 맞지 않을 수도 있기 때문에 근거리와 중거리 센싱을 더욱 폭넓게 결합해야 할 수 있다. 라이다(LiDAR), 카메라(Camera), 레이더(Radar), GNSS, IMU, 초음파 센서(Ultrasonic Sensor) 및 기타 센서를 결합하여 상호 보완적인 정보를 제공할 수 있다. 센서 구성은 모든 플랫폼이 동일한 센싱 아키텍처를 필요로 한다는 고정된 가정보다는 ODD와 임무를 기준으로 결정해야 한다.

위치추정(Localization)에서도 유사한 차이가 나타난다. 도로 주행 자율주행 차량은 차선 형상, 도로 지도, 시각적 랜드마크(Visual Landmark), GNSS 및 고정밀 지도(High-Definition Map)를 효과적으로 활용할 수 있다. 야외 AMR은 차선 표시가 없거나, 지도가 자주 변경되거나, 건물, 구조물, 식생 또는 기타 환경 조건으로 인해 GNSS 성능이 저하되는 환경에서 운용될 수 있다. 따라서 라이다 위치추정(LiDAR Localization), 비전 위치추정(Visual Localization), 관성 추정(Inertial Estimation), GNSS 또는 RTK, 지역 지도작성(Local Mapping)을 함께 운용해야 할 수 있다. 위치추정 아키텍처는 위치추정 신뢰도가 저하되는 상황도 인식하고, 신뢰할 수 없는 위치추정 상태에서 정상 운용을 계속하기보다 적절한 대응을 제공해야 한다.

환경 표현(Environment Representation) 역시 이에 따라 달라진다. 도로 차량은 차량이 합법적이고 물리적으로 주행할 수 있는 위치를 표현하는 차선 수준 및 도로 수준 표현(Lane-Level and Road-Level Representation)에 크게 의존하는 경우가 많다. 야외 AMR은 점유 격자(Occupancy Grid), 자유공간 표현(Free-Space Representation), 의미론적 지도(Semantic Map), 고도 정보(Elevation Information), 지형 분류(Terrain Classification), 주행가능성 지도(Traversability Map) 등이 필요할 수 있다. 포장면, 느슨한 자갈, 젖은 흙, 잔디, 연석, 도랑 또는 건설 잔해는 기존 도로 객체가 아니더라도 로봇에 서로 다른 영향을 줄 수 있다. 따라서 환경 표현은 기하학적 장애물뿐만 아니라 지형의 물리적 특성도 표현해야 한다.

계획 요구사항(Planning Requirements)도 이용 가능한 공간과 상호작용 규칙이 다르기 때문에 달라진다. 도로 차량은 교통 법규, 차선 변경, 교차로, 교통 신호, 우선권(Right-of-Way), 그리고 다양한 도로 이용자와의 상호작용을 고려해야 한다. 야외 AMR은 보행자, 서비스 차량, 지게차(Forklift), 다른 로봇, 임시 차단물, 컨테이너, 장비 또는 예상하지 못한 객체를 만날 수 있다. 계획기는 지정된 경로를 추종할지, 개방된 공간을 통과할지, 장애물을 국부적으로 회피할지, 정지하여 기다릴지, 또는 대체 경로를 선택할지를 결정해야 할 수 있다. 또한 임무 목표는 행동에 큰 영향을 줄 수 있기 때문에 검사 로봇과 물류 로봇은 동일한 물리적 환경에서도 서로 다른 방식으로 운용될 수 있다.

궤적생성(Trajectory Generation)은 플랫폼의 물리적 특성과 운용 환경을 반영해야 한다. 상대적으로 높은 속도로 주행하는 도로 차량은 곡률(Curvature), 횡가속도(Lateral Acceleration), 제동거리(Braking Distance), 조향 한계(Steering Limit), 동적 안정성(Dynamic Stability)을 정밀하게 고려해야 한다. 야외 AMR은 더 낮은 속도로 운용될 수 있지만 거친 지형, 휠 슬립(Wheel Slip), 변화하는 접지력(Traction), 불균일한 노면, 제한된 기동 공간을 만날 수 있다. 이러한 시스템에서는 기하학적으로 적절한 궤적만으로는 충분하지 않다. 궤적은 실제로 주행 가능한 상태를 유지해야 하며, 로봇의 서스펜션, 휠 구성, 적재량, 액추에이터 한계 및 지형 조건과도 호환되어야 한다.

따라서 제어 아키텍처(Control Architecture)에서도 우선순위가 달라진다. 도로 주행 자율주행 차량은 일반적으로 상대적으로 높은 속도에서 정밀한 종방향 제어(Longitudinal Control)와 횡방향 제어(Lateral Control)를 강조하며, 예측 가능한 제동 및 조향 응답에 대한 강한 요구사항을 갖는다. 야외 AMR은 저속 기동성, 접지력 관리(Traction Management), 휠 슬립, 지형 적응(Terrain Adaptation), 적재량에 따른 동역학(Payload-Dependent Dynamics)에 더 큰 비중을 둘 수 있다. 동일한 로봇 플랫폼이 빈 상태에서 중량 적재 상태로 변경되면 가속, 제동, 휠 하중 및 접지 특성이 달라질 수 있다. 제어 알고리즘은 계획된 궤적을 액추에이터 명령으로 변환할 때 이러한 변화를 고려해야 한다.

안전 요구사항(Safety Requirements) 역시 운용 환경에 의해 결정된다. 도로 차량은 고속 교통과 지속적으로 상호작용할 수 있으므로 매우 빠른 검출, 예측, 제동 및 폴백 동작(Fallback Behavior)이 필요할 수 있다. 야외 AMR은 더 낮은 속도로 운용될 수 있지만 산업 장비, 불균일한 지형, 제한된 공간, 보행자 또는 중량 화물과 같은 위험요소를 만날 수 있다. 최대 속도가 낮다고 해서 안전 요구사항이 없어지는 것은 아니며, 위험의 종류와 이용 가능한 완화 전략(Mitigation Strategy)이 달라지는 것이다. 따라서 안전 아키텍처는 실제 ODD, 차량 특성, 임무 및 예상 가능한 고장 조건에서 도출되어야 한다.

인간 개입(Human Involvement)과 원격 감독(Remote Supervision) 역시 상당히 다를 수 있다. 도로 주행 자율주행 차량은 자동화 기능이 활성화된 이후에도 자동화 수준과 운용설계영역에 따라 지속적인 원격 감독 없이 운용될 것이 기대될 수 있다. 통제된 시설에 배치된 야외 AMR은 임무 할당, 상태 모니터링, 예외 처리 또는 복구를 위해 원격 차량군 감독(Remote Fleet Supervision)을 사용할 수 있다. 그러나 원격 감독이 온보드 안전 메커니즘(Onboard Safety Mechanism)을 대체해서는 안 된다. 통신이 지연되거나 사용할 수 없는 상황에서도 즉각적인 충돌 회피, 비상 정지(Emergency Stop), 액추에이터 보호(Actuator Protection), 최소위험 동작(Minimal-Risk Behavior)은 일반적으로 로컬에서 수행될 수 있어야 한다.

따라서 ODD는 두 영역을 비교하는 핵심 메커니즘이 된다. 도로 차량은 도로 유형, 지리적 영역, 교통 환경, 기상, 조명, 속도 및 인프라 가용성을 사용하여 운용 조건을 정의할 수 있다. 야외 AMR은 시설 경계, 지형 유형, 노면 상태, 최대 경사도, 장애물 특성, 보행자 밀도, GNSS 가용성, 통신 범위, 기상 및 임무 제약조건을 사용하여 ODD를 정의할 수 있다. ODD는 단순히 로봇이 어디에서 운용될 수 있는지를 지정하는 것뿐만 아니라, 검증된 자율주행 기능이 적용 가능한 환경 및 시스템 조건도 정의해야 한다.

소프트웨어 아키텍처(Software Architecture)는 이러한 차이를 반영하면서도 전체 자율주행 스택을 불필요하게 중복해서는 안 된다. 두 플랫폼은 센서 처리(Sensor Processing), 인지(Perception), 위치추정(Localization), 계획(Planning), 궤적생성(Trajectory Generation), 제어(Control), 진단(Diagnostics), 안전 모니터링(Safety Monitoring)과 같은 공통 기능 개념을 공유할 수 있다. 그러나 실제 구현, 모델, 파라미터, 데이터 표현 및 검증 시나리오는 상당히 다를 수 있다. 명확한 인터페이스를 갖는 모듈형 아키텍처(Modular Architecture)를 사용하면 공통 자율주행 기능을 재사용하면서도 플랫폼별 구성요소가 지형, 차량 동역학, 센서 구성, 임무 요구사항 및 운용 제약조건을 처리할 수 있다.

검증 전략(Validation Strategy)도 중요한 차이점이다. 도로 주행 시스템은 교통 시나리오, 교차로, 취약 도로 이용자(Vulnerable Road User), 기상 조건, 도로 구성, 고속 상호작용을 폭넓게 시험해야 한다. 야외 AMR은 지형 변화, 임시 장애물, 시설별 배치, 보행자 상호작용, GNSS 성능 저하, 위치추정 고장, 경로 변경, 적재량 변화 및 환경 외란(Environmental Disturbance)에 대한 충분한 검증이 필요하다. 시뮬레이션(Simulation), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험, 통제된 필드 테스트(Controlled Field Testing), 운용 데이터(Operational Data)를 결합하여 실제 배치 환경에서 나타나는 조건을 기준으로 자율주행 시스템을 평가해야 한다.

핵심적인 차이는 야외 AMR이 단순히 더 작거나 느린 자율주행 차량이라는 것이 아니다. 야외 AMR은 구조화되지 않은 환경, 임무별 운용, 다양한 지형, 서로 다른 차량 동역학, 변화할 수 있는 운용 경계에 의해 형성되는 별도의 자율주행 문제를 가진다. 도로 주행 자율주행 차량은 표준화된 도로 인프라와 교통 관행을 활용할 수 있지만, 야외 AMR은 로봇이 어디에서 어떻게 이동할 수 있는지에 대해 더욱 유연한 이해를 스스로 구축해야 하는 경우가 많다. 두 시스템 모두 인지, 계획, 제어를 필요로 하지만 운용 영역에 따라 이러한 기능의 상대적 중요성과 구현 방식은 달라진다.

야외 AMR 개발 프로그램에서 실질적인 목표는 센싱, 환경 이해(Environmental Understanding), 위치추정, 주행가능성 추정(Traversability Estimation), 행동계획(Behavior Planning), 궤적생성 및 제어가 로봇의 물리적 능력과 ODD에 명확하게 연결되는 시스템 아키텍처를 구축하는 것이다. 도로 주행 자율주행 차량과의 비교는 어떤 기술을 재사용할 수 있고 어떤 가정을 그대로 이전해서는 안 되는지를 명확하게 해준다는 점에서 중요하다. 이러한 구분은 의도된 야외 운용 환경에서 예측 가능하고, 시험 가능하며, 안전한 상태를 유지할 수 있는 AMR 전용 자율주행 스택(AMR-Specific Autonomy Stack)을 설계하는 기반을 제공한다.

## 01.05. Regulatory Landscape ISO 22737 UL 3300 Overview

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

자율이동로봇(Autonomous Mobile Robot, AMR)의 규제 및 표준 요구사항(Regulatory and Standards Requirements)은 하나의 인증 규칙(Certification Rule)이 아니라 여러 계층으로 구성된 프레임워크(Layered Framework)로 이해해야 한다. 야외 AMR은 자율주행 기능(Automated Driving Function), 기계 안전(Machinery Safety), 전기 안전(Electrical Safety), 기능 안전(Functional Safety), 사이버보안(Cybersecurity), 무선통신(Radio Communication), 응용 분야별 요구사항(Application-Specific Requirements)과 관련될 수 있다. ISO 22737과 UL 3300은 유용한 참조 기준(Reference)으로 활용될 수 있지만, 서로 다른 로봇 및 운용 환경을 대상으로 한다. 따라서 표준을 선택할 때에는 로봇의 의도된 기능(Intended Function), 운용 환경, 사용자, 운용설계영역(Operational Design Domain, ODD)부터 정의해야 한다.

ISO 22737:2021은 사전에 정의된 경로(Predefined Route)에서 운용되는 저속 자동주행(Low-Speed Automated Driving, LSAD) 시스템을 다룬다. 이 표준은 ODD, 시스템 기능(System Function), 최소 성능(Minimum Performance), 안전한 운용을 위한 성능 시험 절차(Performance Test Procedure)와 관련된 요구사항을 규정한다. 이 표준은 특정 ODD 내에서 레벨 4(Level 4) 자동화를 중심으로 설계되었으며, 상업지역, 업무용 캠퍼스, 대학 캠퍼스와 같은 저속 자동 운송(Low-Speed Automated Transportation) 응용 분야와 특히 관련성이 있다.

ISO 22737의 중요한 특징은 특정 센서 기술(Sensor Technology)을 규정하지 않는다는 것이다. 대신 LSAD 시스템이 의도된 운용 조건 내에서 충족해야 하는 시스템 및 성능 요구사항을 설정한다. 그 구조에는 위험 상황 결정(Hazardous-Situation Determination), 최소위험기동(Minimal-Risk Manoeuvre, MRM), 주행가능영역(Drivable Area) 내 운용, 비상정지(Emergency Stop), 최대 차량 속도(Maximum Vehicle Speed), 장애물 검출(Obstacle Detection), 안전 관련 이벤트 기록(Safety-Critical Event Recording), 그리고 관련 성능 시험 절차가 포함된다. 따라서 이 표준은 자율주행 요구사항을 측정 가능한 시스템 동작과 연결하는 데 유용하다.

야외 AMR의 경우 ISO 22737은 로봇이 사전에 정의된 경로를 따라 저속 자율주행을 수행할 때 유용한 개념적 및 공학적 참조 기준으로 활용할 수 있다. 이는 캠퍼스 운송(Campus Transportation), 통제된 물류 경로(Controlled Logistics Route), 산업 또는 상업 시설(Industrial or Commercial Premises), 그리고 경로와 운용 조건을 명확하게 정의할 수 있는 기타 환경에 특히 적용할 수 있다. 그러나 실제 적용 가능성은 실제 배치 환경을 기준으로 평가해야 한다. 건설현장이나 농경지와 같은 지형에서 자유롭게 이동하는 오프로드 AMR은 사전에 정의된 경로를 따르는 LSAD 운용과 상당히 다른 요구사항을 가질 수 있기 때문이다.

UL 3300은 다른 범주의 로봇을 다룬다. 현재 ANSI/CAN/UL 3300의 명칭은 "Service, Communication, Information, Education and Entertainment Robots"이며, 서비스·통신·정보·교육·엔터테인먼트 로봇(Service, Communication, Information, Education and Entertainment Robots, SCIEE Robots)에 대한 안전 요구사항을 규정한다. 이 표준의 요구사항은 제품의 비로봇 기능(Non-Robotic Function)에 적용되는 관련 안전 요구사항을 보완한다. 이 표준은 실내 및 실외에서 사용되는 해당 로봇을 대상으로 할 수 있으며, 로봇의 이동성(Mobility)과 비구속형 조작(Uncontained Manipulation)에 따른 위험을 고려한다. 여기에는 로봇 속도, 질량 및 운용 환경의 영향이 포함된다.

특정 AMR에 UL 3300이 적합한지를 평가할 때에는 그 적용 범위(Scope)를 특히 중요하게 고려해야 한다. 이 표준은 일반 소비자가 사용하거나 일반 소비자와 가까운 곳에서 사용하도록 의도된 SCIEE 로봇을 포함하지만, 사람의 도로 또는 오프로드 운송(On-Road or Off-Road Transport of Persons), 산업 환경(Industrial Environment), 위험 장소(Hazardous Location), 농업용 사용(Agricultural Use), 식품 조리(Food Preparation), 의료 응용(Medical Application) 및 기타 일부 전문 응용 분야는 명시적으로 제외한다. 따라서 UL 3300을 모든 야외 산업용 AMR에 적용되는 보편적인 안전 표준(Universal Safety Standard)으로 해석해서는 안 된다.

UL 3300의 현재 상태(Status)도 추적해야 한다. 표준은 지속적으로 발전하기 때문이다. UL 3300 제1판(Edition 1)은 2024년 5월 14일에 발행되었으며, UL Standards & Engagement 기록에는 2026년 8월 14일의 개정(Revision)이 표시되어 있고 ANSI 및 SCC 승인을 포함한다. 2026년 개정에는 시험을 위한 주변 조도 조건(Ambient-Light Condition)의 명확화, 로봇 안정성 시험(Robot Stability Testing)의 변경, 장애물 시험(Obstacle Testing)의 변경 등이 포함된다. 따라서 개발 프로그램에서는 오래된 사본이나 2차 자료에 의존하기보다 적용 가능한 판(Edition)과 개정(Revision)을 식별해야 한다.

표준(Standard)과 규정(Regulation)의 차이를 이해하는 것도 중요하다. 표준은 일반적으로 기술 요구사항, 시험 방법, 용어 또는 공학적 지침을 제공하는 반면, 규정은 특정 관할권(Jurisdiction)에서 법적으로 강제되는 의무를 설정한다. 인증(Certification) 또는 적합성 평가(Conformity Assessment)는 제품이 특정 표준을 충족한다는 것을 입증할 수 있지만, 특정 표준을 준수한다고 해서 모든 법적 배치 요구사항을 충족했다는 의미는 아니다. 따라서 AMR 제조업체는 적용 가능한 국가, 지역, 현장 및 응용 분야별 규제 체계를 별도로 평가해야 한다.

자율주행 기능에 대한 규제 분석은 차량의 의도된 기능과 ODD를 정의하는 것에서 시작해야 한다. 사전에 정의된 캠퍼스 경로에서 저속으로 운용되는 로봇은 항만, 건설현장, 철도 시설 또는 광산 환경에서 화물을 운송하는 대형 야외 AMR과 다른 규제 특성을 갖는다. 중요한 변수에는 최대 속도, 로봇 질량, 적재량(Payload), 경로 구조, 지형, 보행자 및 차량과의 상호작용, 원격 감독(Remote Supervision), 환경 조건, 그리고 로봇이 사람을 운송하는지 여부 등이 포함된다.

이후 안전 요구사항(Safety Requirements)을 시스템 수준의 공학적 요구사항(System-Level Engineering Requirements)으로 변환해야 한다. 비상정지(Emergency Stop), 위험 상황 감지(Hazardous-Situation Detection), 최소위험기동(Minimal-Risk Manoeuvre), 장애물 검출(Obstacle Detection), 안전 상태 전환(Safe-State Transition), 안전 관련 이벤트 기록(Safety-Critical Event Recording)과 같은 개념은 단순한 문서상의 내용으로 남아 있어서는 안 되며 명확한 시스템 동작으로 구현되어야 한다. 자율주행 소프트웨어, 차량 제어기(Vehicle Controller), 안전 제어기(Safety Controller), 센서, 액추에이터, 통신 인터페이스 및 인간-기계 인터페이스(Human-Machine Interface)는 명확하게 정의된 책임과 시험 가능한 안전 요구사항을 가져야 한다.

ODD와 적합성(Compliance)의 관계는 특히 중요하다. 자율 시스템은 제한된 ODD 안에서는 정의된 성능 요구사항을 충족할 수 있지만, 그 영역 밖에서는 운용에 적합하지 않을 수 있다. 따라서 규제 및 안전 논증(Safety Case)은 시스템이 설계되고 검증된 조건을 명확하게 식별해야 한다. 지리적 경계, 지형, 속도, 기상, 조도, 위치추정 가용성, 통신 연결성, 보행자 상호작용 및 장애물 특성은 모두 적합성 증거(Compliance Evidence)의 중요한 부분이 될 수 있다.

시험(Test)은 문서화된 요구사항을 반복 가능한 증거(Repeatable Evidence)와 연결해야 한다. 야외 AMR의 경우 장애물 검출, 비상정지, 안전 상태 전환, 안정성(Stability), 제동(Braking), 기동성(Maneuverability), 위치추정 성능 저하, 통신 손실, 센서 성능 저하, 환경 조건 및 대표적인 임무 시나리오 등을 포함할 수 있다. 시뮬레이션(Simulation)과 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험은 개발을 지원할 수 있지만, 실제 차량, 지형, 타이어, 적재량, 센서 및 액추에이터에 강하게 의존하는 특성에 대해서는 실제 물리적 시험(Physical Testing)도 중요하다.

유용한 적합성 아키텍처(Compliance Architecture)는 안전에 중요한 기능(Safety-Critical Function)과 일반 자율주행 지능(General Autonomy Intelligence)을 구분해야 한다. 인지(Perception)와 계획(Planning)은 정교한 머신러닝 모델(Machine-Learning Model)을 사용할 수 있지만, 비상정지, 안전 모니터링(Safety Monitoring), 워치독(Watchdog), 액추에이터 차단(Actuator Inhibition) 및 기타 핵심 기능은 보다 결정론적인 동작(Deterministic Behavior)과 독립적인 모니터링(Independent Monitoring)을 필요로 할 수 있다. 규제의 목적은 AI 모델이 높은 성능을 보인다는 것을 단순히 입증하는 것이 아니라, 정의된 운용 조건과 예측 가능한 고장 상황에서 전체 물리적 시스템이 허용 가능한 수준의 안전성을 유지한다는 것을 입증하는 것이다.

산업용 야외 AMR의 경우 실질적인 접근법은 하나의 표준을 보편적인 해답으로 선택하기보다는 표준 매트릭스(Standards Matrix)를 구축하는 것이다. ISO 22737은 사전에 정의된 경로에서 운용되는 저속 자동주행 시스템에 대해 관련 지침을 제공할 수 있으며, UL 3300은 명시된 적용 범위 내에서 특정 서비스 또는 소비자 지향 로봇 범주에 적용될 수 있다. 또한 로봇과 배치 환경에 따라 다른 기계, 전기, 기능 안전, 전자기 적합성(Electromagnetic Compatibility, EMC), 사이버보안 및 응용 분야별 요구사항이 적용될 수 있다. 정확한 표준 조합은 제품의 기능과 적용 관할권을 기준으로 결정해야 한다.

가장 중요한 공학적 원칙은 적합성(Compliance)을 최종 인증 단계로 취급하는 것이 아니라 처음부터 자율주행 아키텍처에 설계하는 것이다. ODD 정의, 위험 분석(Hazard Analysis), 시스템 요구사항, 안전 메커니즘, 성능 목표, 시험 시나리오, 이벤트 기록, 추적성(Traceability), 증거 생성(Evidence Generation)은 함께 발전해야 한다. 이러한 접근법을 통해 야외 AMR 개발자는 로봇이 자율적으로 주행할 수 있다는 것뿐만 아니라, 자율주행 동작이 명확한 범위로 제한되고, 측정 가능하며, 시험 가능하고, 운용 조건이나 시스템 가정이 위반될 때 적절하게 통제된다는 것을 입증할 수 있다.

## 01.06. Key Performance Indicators KPI for AV AMR Systems

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

핵심성과지표(Key Performance Indicators, KPI)는 자율주행 차량(Autonomous Vehicle, AV) 또는 자율이동로봇(Autonomous Mobile Robot, AMR)이 의도된 임무를 안전하고 신뢰성 있으며 효율적으로 수행하는지를 평가하기 위한 측정 가능한 기준을 제공한다. 유용한 KPI 프레임워크(KPI Framework)는 개별 알고리즘만이 아니라 전체 자율주행 시스템을 평가해야 한다. 따라서 인지 정확도(Perception Accuracy), 위치추정 정밀도(Localization Precision), 계획 성공률(Planning Success), 제어 품질(Control Quality), 안전 동작(Safety Behavior), 임무 완료, 계산 성능 및 운용 가용성(Operational Availability)을 운용설계영역(Operational Design Domain, ODD) 및 시스템 수준 요구사항과 연결해야 한다.

인지 KPI(Perception KPI)는 시스템이 주변 환경을 얼마나 정확하고 일관되게 이해하는지를 측정한다. 일반적인 지표에는 객체 검출 정밀도와 재현율(Object Detection Precision and Recall), 분류 정확도(Classification Accuracy), 거짓양성 및 거짓음성 비율(False-Positive and False-Negative Rates), 분할 품질(Segmentation Quality), 검출 거리(Detection Range), 추적 연속성(Tracking Continuity), 속도 추정 오차(Velocity-Estimation Error)가 포함된다. 야외 AMR에서는 지형 분류(Terrain Classification), 주행가능영역 추정(Drivable-Area Estimation), 장애물 높이, 자유공간 검출(Free-Space Detection), 어둠, 비, 먼지, 눈부심, 가림(Occlusion), 센서 오염으로 인한 성능 저하도 평가해야 한다.

인지 KPI를 단순히 평균 정확도(Average Accuracy) 하나로 표현해서는 안 된다. 안전과 관련된 성능은 거리, 객체 유형, 상대속도(Relative Velocity), 환경 조건 및 신뢰도(Confidence)에 따라 달라지는 경우가 많다. 이미 가용 정지거리(Available Stopping Distance)를 초과한 뒤 근거리에서 보행자를 검출하는 것은 안전한 제동이 가능할 정도로 충분히 일찍 동일한 보행자를 검출하는 것과 운용적으로 다르다. 따라서 인지 지표는 반응시간(Reaction Time), 정지거리(Stopping Distance), 차량 속도 및 누락되거나 지연된 검출의 심각도와 연결해야 한다.

위치추정 KPI(Localization KPI)는 로봇이 자신의 위치와 자세(Orientation)를 얼마나 정확하고 강건하게 추정하는지를 정량화한다. 위치 오차(Position Error), 방위각 오차(Heading Error), 상대 위치추정 오차(Relative Localization Error), 지도 정합 일관성(Map-Matching Consistency), 드리프트(Drift), 업데이트 주기(Update Rate), 위치추정 가용성(Localization Availability)을 측정할 수 있다. GNSS RTK, 라이다(LiDAR), 카메라(Camera), 관성 센서(Inertial Sensor)를 사용하는 야외 AMR은 GNSS 성능 저하, 다중경로(Multipath), 일시적인 신호 손실, 불충분한 환경 특징, 지도 변화 및 서로 다른 위치추정 소스 사이의 전환 과정에서도 성능을 평가해야 한다.

위치추정 무결성(Localization Integrity)은 정상 상태에서의 위치추정 정확도보다 더 중요할 수 있다. 일반적으로 센티미터 수준의 위치를 제공하지만 가끔 큰 오차를 발생시키면서도 이를 감지하지 못하는 시스템은 정확도는 다소 낮더라도 자신의 불확실성을 안정적으로 보고하는 시스템보다 더 위험할 수 있다. 따라서 KPI에는 신뢰도 일관성(Confidence Consistency), 고장 감지 지연(Fault-Detection Latency), 복구시간(Recovery Time), 모니터링 기능에서 감지되지 않은 잘못된 위치추정 상태의 발생 빈도 등이 포함되어야 한다. 이러한 측정값은 위치추정을 계획 및 안전 동작과 직접 연결한다.

계획 KPI(Planning KPI)는 자율 시스템이 안전하고 실행 가능하며 임무에 적합한 행동을 선택하는지를 평가한다. 지표에는 경로계획 성공률(Route-Planning Success Rate), 재계획 지연(Re-planning Latency), 장애물 회피 성공률(Obstacle-Avoidance Success), 교착상태 발생 빈도(Deadlock Frequency), 경로 효율(Path Efficiency), 규칙 준수(Rule Compliance), 복구 성공률(Recovery Success) 등이 포함될 수 있다. 야외 AMR의 계획기는 차단된 경로, 임시 공사 구역, 좁은 통로, 이동 차량, 보행자, 다른 로봇 및 일시적으로 주행 불가능해진 지형에 대해서도 평가되어야 한다.

궤적생성 성능(Trajectory-Generation Performance)은 경로 길이(Path Length), 곡률(Curvature), 장애물과의 여유거리(Clearance), 부드러움(Smoothness), 가속도(Acceleration), 저크(Jerk), 계산시간(Computation Time), 동적 실행 가능성(Dynamic Feasibility)을 사용하여 측정할 수 있다. 가장 짧은 궤적이 반드시 가장 좋은 궤적은 아니다. 과도한 곡률, 가속도 또는 장애물과의 지나치게 가까운 거리는 안전성과 제어 가능성을 저하시킬 수 있기 때문이다. 따라서 유용한 KPI 집합은 효율성과 차량 동역학(Vehicle Dynamics), 필요한 경우 승차감, 지형 제약, 액추에이터 능력 및 요구되는 안전 여유(Safety Margin) 사이의 균형을 평가해야 한다.

제어 KPI(Control KPI)는 실제 물리적 플랫폼이 명령된 궤적을 얼마나 정확하게 추종하는지를 나타낸다. 횡방향 추종 오차(Lateral Tracking Error), 종방향 속도 오차(Longitudinal Speed Error), 방위각 오차, 정지 위치 오차(Stopping-Position Error), 오버슈트(Overshoot), 정착시간(Settling Time), 조향 응답(Steering Response), 제동 응답(Braking Response), 제어 루프 지연(Control-Loop Latency)이 일반적인 지표이다. 야외 AMR에서는 적재량 변화, 경사면, 불균일한 지형, 변화하는 접지력(Traction), 휠 슬립(Wheel Slip), 액추에이터 성능 저하에서도 성능을 측정해야 한다. 이러한 요소가 제어 명령과 실제 차량 움직임 사이의 관계를 크게 변화시킬 수 있기 때문이다.

안전 KPI(Safety KPI)는 위험 사건의 예방뿐만 아니라 비정상적인 조건이 발생했을 때 대응의 효과도 측정해야 한다. 관련 지표에는 충돌률(Collision Rate), 근접사고 발생 빈도(Near-Miss Frequency), 비상정지 작동(Emergency-Stop Activation), 최소 충돌예상시간(Minimum Time-to-Collision), 최소 장애물 여유거리(Minimum Obstacle Clearance), 안전 정지 성공률(Safe-Stop Success), 최소위험기동 성공률(Minimal-Risk Maneuver Success), 안전 모니터 작동(Safety-Monitor Activation), 위험 고장 감지 지연(Hazardous-Failure Detection Latency)이 포함된다. 이러한 지표는 운용거리, 운용시간, 임무 또는 경험한 시나리오와 같은 노출도(Exposure)를 고려하여 해석해야 한다.

디스인게이지먼트 및 개입 지표(Disengagement and Intervention Metrics)는 자율주행 성숙도(Autonomy Maturity)를 평가하는 또 다른 관점을 제공한다. 디스인게이지먼트(Disengagement)는 현재 조건에서 시스템이 안전하게 자율운용을 계속할 수 없어 자율운용이 종료되거나 제어권이 이전되는 상황을 의미한다. 유용한 측정값에는 킬로미터당 개입 횟수, 운용시간당 개입 횟수, 개입 사이의 주행거리, 개입 원인, 복구 성공률, 그리고 인지, 위치추정, 계획, 제어, 하드웨어 또는 ODD 위반과 관련된 개입 비율이 포함된다.

그러나 디스인게이지먼트 횟수가 적다고 해서 자동으로 더 안전하거나 더 높은 능력을 갖춘 시스템이라는 의미는 아니다. 서로 다른 차량군(Fleet)은 상당히 다른 환경에서 운용될 수 있으며, 보고 정책에 따라 디스인게이지먼트의 정의도 달라질 수 있다. 단순하고 폐쇄된 경로에서 운용되는 AMR과 보행자, 차량, 경사면 및 변화하는 장애물이 존재하는 환경에서 운용되는 로봇을 킬로미터당 개입 횟수만으로 직접 비교할 수는 없다. 따라서 KPI 해석에는 ODD 복잡도, 환경 노출도, 임무 난이도 및 사건의 심각도가 포함되어야 한다.

임무 수준 KPI(Mission-Level KPI)는 자율주행 성능을 로봇의 실제 목적과 연결한다. 응용 분야에 따라 임무 완료율(Mission Completion Rate), 자율 임무 완료율(Autonomous Mission Completion Rate), 이동시간(Travel Time), 경로 효율, 작업 처리량(Task Throughput), 도킹 성공률(Docking Success), 충전 성공률(Charging Success), 배송 성공률(Delivery Success), 검사 범위(Inspection Coverage), 순찰 완료율(Patrol Completion), 복구율(Recovery Rate)을 측정할 수 있다. 이러한 지표는 기술적으로 성공적인 내비게이션이 단순히 이동만 생성하는 것이 아니라 실제로 유용한 운용 성능으로 이어지는지를 보여준다.

신뢰성 및 가용성 KPI(Reliability and Availability KPI)는 시스템이 장시간 유용한 운용을 지속할 수 있는지를 측정한다. 평균고장간격(Mean Time Between Failures, MTBF), 평균수리시간(Mean Time to Repair, MTTR), 운용 가용성(Operational Availability), 자율운용시간(Autonomous Operating Time), 고장 발생 빈도(Fault Frequency), 재부팅 빈도(Reboot Frequency), 센서 가용성(Sensor Availability), 임무 중단율(Mission Interruption Rate)은 중요한 정보를 제공할 수 있다. 상업용 AMR에서는 소프트웨어 오류, 센서 고장, 열 문제 또는 복구 절차가 서비스를 반복적으로 중단한다면 기술적으로 정교한 자율주행 시스템이라도 실질적인 가치는 제한된다.

계산 KPI(Computational KPI)는 현대 자율주행 스택이 제한된 엣지 컴퓨팅 자원(Edge-Computing Resource)에서 여러 신경망, 센서 처리 파이프라인, 위치추정 알고리즘, 계획기 및 제어기를 함께 실행하기 때문에 점점 더 중요해지고 있다. 종단간 지연시간(End-to-End Latency), 모듈 지연시간(Module Latency), 프레임률(Frame Rate), CPU 사용률, GPU 사용률, 메모리 사용량, 통신 대역폭(Communication Bandwidth), 전력소비(Power Consumption), 타이밍 지터(Timing Jitter)를 모니터링해야 한다. 안전에 중요한 실시간 기능에서는 평균 지연시간보다 최악조건(Worst-Case) 또는 백분위 지연시간(Percentile Latency)이 더 유용할 수 있다.

배터리 기반 AMR에서는 에너지 효율(Energy Efficiency)도 포함해야 한다. 킬로미터당 에너지 소비량, 임무당 에너지, 충전당 운용시간, 유휴 전력(Idle Power), 컴퓨팅 전력(Compute Power), 적재량에 따른 에너지 소비, 충전 효율(Charging Efficiency)은 차량군 생산성(Fleet Productivity)에 영향을 준다. 과도한 가속, 불필요한 재계획, 비효율적인 경로, 지나치게 높은 계산 부하 및 부적절한 열관리(Thermal Management)는 내비게이션 정확도가 높더라도 실제 운용시간을 감소시킬 수 있다.

ODD 준수 KPI(ODD Compliance KPI)는 로봇이 자율주행 기능이 검증된 조건 안에 계속 머물고 있는지를 판단한다. 측정값에는 ODD 경계 근처 또는 외부에서 운용된 시간, ODD 위반 빈도, 지원되지 않는 조건의 성공적인 감지, 대응 지연시간(Response Latency), 성능저하 모드 작동(Degraded-Mode Activation), 안전 복구(Safe Recovery) 등이 포함될 수 있다. 이러한 지표는 ODD를 정적인 사양에서 시뮬레이션, 시험 및 실제 배치 중에 모니터링할 수 있는 관측 가능한 런타임 속성(Observable Runtime Property)으로 변환한다.

성숙한 KPI 프레임워크는 구성요소 지표(Component Metrics)에서 시스템 및 임무 지표(System and Mission Metrics)에 이르기까지 측정값을 계층적으로 구성해야 한다. 높은 객체 검출 정확도가 안전한 내비게이션을 보장하지 않으며, 낮은 궤적 추종 오차가 성공적인 임무를 보장하지도 않는다. 구성요소 KPI는 하위 시스템의 동작을 설명하고, 시스템 KPI는 통합된 자율주행 성능을 측정하며, 안전 KPI는 위험 관련 동작을 평가하고, 운용 KPI(Operational KPI)는 로봇이 장기간의 실제 배치에서 유용한 서비스를 제공하는지를 판단한다.

KPI는 집계된 데이터셋(Aggregated Dataset)만을 대상으로 평가하기보다 대표적인 시나리오별로 평가해야 한다. 이러한 요소가 ODD에 포함되는 경우 주간과 야간 운용, 기상, 지형, 속도, 적재량, 보행자 밀도, 위치추정 품질, 경로 복잡도, 센서 상태에 따라 성능을 구분해야 한다. 시나리오 조건별 지표(Scenario-Conditioned Metrics)는 전체 차량군 평균값에 가려질 수 있는 취약점을 드러내며, 공학적 의사결정과 검증을 위한 더 유용한 증거를 제공한다.

각 KPI의 임계값(Threshold)은 단순히 측정하기 편리하다는 이유로 선택하는 것이 아니라 시스템 요구사항과 안전 분석(Safety Analysis)에서 도출되어야 한다. 최대 정지거리, 허용 가능한 위치추정 오차, 최소 장애물 여유거리, 제어 지연시간, 인지 범위, 임무 가용성 및 복구시간은 차량 동역학, 센서 능력, ODD 가정 및 식별된 위험요소(Hazard)와 연결되어야 한다. 이를 통해 공학적 요구사항에서 측정 가능한 검증 증거(Validation Evidence)에 이르는 추적성(Traceability)을 구축할 수 있다.

따라서 가장 유용한 KPI 아키텍처(KPI Architecture)는 폐루프 측정 과정(Closed Measurement Loop)을 형성한다. 시뮬레이션(Simulation), 통제된 시험(Controlled Testing), 현장 운용(Field Operation)을 통해 지표를 생성하고, 실패 및 취약 시나리오를 식별하며, 인지, 계획, 제어, 하드웨어 또는 ODD 가정을 개선한 후 시스템을 다시 시험한다. 이러한 과정을 통해 KPI는 단순한 보고용 통계를 넘어 자율주행 성숙도, 안전 검증(Safety Validation), 운용 최적화(Operational Optimization), AV 및 야외 AMR 시스템의 지속적인 개발을 위한 정량적 증거(Quantitative Evidence)를 제공한다.

## 01.07. Fail Safe and Fail Operational Design Principles

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

고장안전(Fail-Safe) 및 고장운용(Fail-Operational) 설계 원칙은 고장, 성능 저하 상태 또는 예상하지 못한 사건이 정상 운용을 위협할 때 자율주행 차량(Autonomous Vehicle, AV)이나 자율이동로봇(Autonomous Mobile Robot, AMR)이 어떻게 대응해야 하는지를 정의한다. 고장안전 설계(Fail-Safe Design)는 일반적으로 감속하거나 정지하여 허용할 수 없는 위험이 감소하는 상태로 시스템을 전환하는 것을 목표로 한다. 반면 고장운용 설계(Fail-Operational Design)는 안전한 목적지, 복구 지점 또는 최소위험상태(Minimal-Risk Condition)에 도달할 때까지 최소한 일시적으로라도 운용을 지속할 수 있는 충분한 기능을 유지하는 것을 목표로 한다.

이러한 개념의 차이는 임무(Mission)와 운용설계영역(Operational Design Domain, ODD)에 크게 의존한다. 통제된 구역에서 저속으로 운용되는 소형 AMR은 치명적인 고장을 감지한 직후 안전하게 정지할 수 있다. 반면 경사면에서 이동하거나, 교차로를 통과하거나, 대형 화물을 운반하거나, 다른 차량 주변에서 운용되는 중량급 야외 AMR은 갑작스럽게 정지할 경우 추가적인 위험을 발생시킬 수 있다. 따라서 적절한 고장 대응은 차량 상태, 환경, 교통 상호작용, 지형, 임무 상황 및 시스템에 남아 있는 기능을 고려해야 한다.

고장안전 동작(Fail-Safe Behavior)은 신뢰할 수 있는 고장 감지(Fault Detection)에서 시작된다. 센서, 컴퓨팅 하드웨어, 통신 네트워크, 전원 시스템, 액추에이터(Actuator), 위치추정 모듈(Localization Module), 자율주행 소프트웨어는 각각 서로 다른 방식으로 고장날 수 있다. 일부 고장은 완전하고 명확하게 나타나지만, 다른 고장은 성능 저하, 지연, 간헐적 이상 또는 잘못된 정보의 형태로 나타난다. 따라서 시스템은 기능의 완전한 상실뿐만 아니라 구성요소가 계속 동작하고 있더라도 그 출력이 더 이상 자율운용에 충분히 신뢰할 수 없는 상황까지 감지해야 한다.

따라서 고장 감지는 상태 모니터링(Health Monitoring) 및 타당성 검사(Plausibility Checking)와 결합되어야 한다. 센서 출력은 중복 또는 상호 보완적인 센싱 방식과 비교할 수 있고, 액추에이터 명령은 실제로 측정된 움직임과 비교할 수 있으며, 위치추정 결과는 독립적인 관측 정보와 대조할 수 있다. 워치독(Watchdog)은 정지된 소프트웨어 프로세스나 누락된 실행 마감시간을 감지할 수 있으며, 통신 모니터(Communication Monitor)는 메시지 손실과 과도한 지연시간을 식별할 수 있다. 이러한 메커니즘은 자율주행 스택(Autonomy Stack)이 계속해서 안전하게 운용될 수 있는지 판단하기 위한 근거를 제공한다.

고장이 감지되면 시스템은 고장의 심각도와 운용에 미치는 영향을 판단해야 한다. 모든 고장이 비상정지(Emergency Stop)를 요구하는 것은 아니다. 중요하지 않은 카메라 하나가 손실되더라도 다른 센서가 충분한 환경 인지 범위를 유지한다면 운용을 계속할 수 있지만, 조향 제어(Steering Control)가 상실되면 즉각적인 안전 조치가 필요할 수 있다. 따라서 고장 분류(Fault Classification)는 어떤 기능이 영향을 받는지, 중복 기능이 남아 있는지, 상태가 얼마나 빠르게 악화되는지, 현재 운용 상황에서 제어된 성능 저하가 가능한지를 고려해야 한다.

점진적 성능 저하(Graceful Degradation)는 정상 운용과 완전한 시스템 종료 사이를 연결하는 중요한 개념이다. 자율주행 기능을 단순히 사용 가능 또는 사용 불가능으로 구분하는 대신 시스템은 여러 단계의 기능 수준으로 운용될 수 있다. 로봇은 속도를 낮추거나, 장애물과의 여유거리를 증가시키거나, 추월 또는 복잡한 기동을 비활성화하거나, 운행 경로를 제한하거나, 모니터링 임계값을 강화하거나, 보다 단순한 내비게이션 모드(Navigation Mode)로 전환할 수 있다. 이를 통해 성능이 저하된 센싱, 계산, 위치추정 또는 구동 기능에 대한 요구를 줄이면서 유용한 기능을 계속 유지할 수 있다.

고장운용 능력(Fail-Operational Capability)을 확보하려면 특정 고장이 발생해도 차량을 안전하게 제어할 수 있는 능력을 즉시 상실하지 않도록 충분한 중복성(Redundancy)과 독립성(Independence)이 필요하다. 위험 분석(Hazard Analysis)에 따라 중복 센싱, 컴퓨팅, 통신, 전원, 제동, 조향 또는 위치추정 기능을 사용할 수 있다. 그러나 단순한 복제(Duplication)만으로 고장허용성(Fault Tolerance)이 보장되는 것은 아니다. 동일한 두 구성요소가 공통 전원, 소프트웨어 결함, 환경 조건, 네트워크 의존성 또는 물리적 손상으로 인해 동시에 고장날 수 있기 때문이다.

따라서 중복성을 설계할 때 공통원인고장(Common-Cause Failure)을 명시적으로 고려해야 한다. 카메라, 라이다(LiDAR), 레이더(Radar), GNSS, 관성 센싱(Inertial Sensing)과 같이 서로 다른 센서를 사용하면 상이한 고장 특성을 확보할 수 있으며, 독립적인 전원 경로나 컴퓨팅 채널을 사용하면 공통 의존성을 줄일 수 있다. 기능적 다양성(Functional Diversity)도 중요하다. 정교한 인공지능 인지 파이프라인(AI Perception Pipeline)이 정상 자율운용을 지원하는 동안, 보다 단순하고 독립적인 안전 채널(Safety Channel)이 보호 영역을 감시하고 주 자율주행 시스템의 신뢰성이 저하되었을 때 제동 명령을 수행할 수 있다.

고장운용 설계는 고장이 발생한 이후에도 반드시 원래 임무를 계속 수행해야 한다는 의미는 아니다. 그 목적은 오히려 제어된 폴백(Controlled Fallback)을 실행하기에 충분한 기능을 유지하는 것일 수 있다. 예를 들어 AMR은 새로운 임무의 수락을 중지하거나, 속도를 낮추거나, 교차로를 벗어나거나, 위험한 경사면에서 이동하거나, 지정된 안전 구역(Safe Zone)에 도달하거나, 정비 위치로 복귀하거나, 원격 지원(Remote Assistance)을 받을 수 있을 때까지 제어 가능한 상태를 유지할 수 있다. 운용을 계속 유지해야 하는 시간은 특정 위험과 폴백 전략(Fallback Strategy)에 따라 결정되어야 한다.

최소위험상태(Minimal-Risk Condition)는 다양한 폴백 전략의 목표 상태를 제공한다. 플랫폼과 환경에 따라 이 상태는 제어된 정지(Controlled Stop), 활성 교통 경로 밖으로의 주차, 기계식 브레이크(Mechanical Brake) 체결, 추진 기능 비활성화, 경고 표시 활성화 또는 제한적인 조향 능력 유지 등을 포함할 수 있다. 따라서 최소위험상태는 상황에 따라 달라진다. 중량급 AMR이 경사면에 있거나 다른 기계가 계속 운용되는 위치를 점유하고 있는 경우 단순히 모터 전원을 차단하는 것만으로는 안전하지 않을 수 있다.

인지 고장(Perception Failure)은 잘못된 환경 이해가 계획 의사결정으로 직접 전파될 수 있기 때문에 특히 신중하게 처리해야 한다. 카메라 가림, 라이다 오염, 레이더 성능 저하, 과도한 눈부심, 어둠, 먼지, 비 또는 센서 정렬 불량(Sensor Misalignment)은 완전한 하드웨어 고장을 발생시키지 않으면서도 인지 성능을 저하시킬 수 있다. 런타임 모니터링(Runtime Monitoring)은 센서 품질과 인지 범위를 추정하고, 센싱 채널 간의 불일치를 식별하며, 남아 있는 인지 능력이 현재 속도와 운용 환경에 충분한지를 판단해야 한다.

위치추정 고장(Localization Failure)도 유사한 문제를 발생시킨다. GNSS 다중경로(Multipath), 지도 불일치(Map Mismatch), 특징 부족(Feature Scarcity), 관성 드리프트(Inertial Drift) 또는 센서 고장으로 인해 위치추정 결과의 신뢰성이 떨어지더라도 로봇은 계속 위치 정보를 수신할 수 있다. 위치추정 무결성 모니터링(Localization Integrity Monitoring)은 단순히 위치 메시지의 존재 여부에 의존하기보다 불확실성과 일관성을 평가해야 한다. 신뢰도가 허용 가능한 임계값 아래로 떨어지면 시스템은 속도를 낮추거나, 움직임을 제한하거나, 위치추정 소스를 전환하거나, 정지하거나, 사전에 정의된 다른 폴백 전략을 시작할 수 있다.

계획 및 제어 기능도 고장 격리(Failure Containment)가 필요하다. 계획기(Planner)는 손상된 입력, 소프트웨어 결함, 타이밍 위반 또는 예상하지 못한 환경 조건으로 인해 실행 불가능하거나 안전하지 않은 궤적을 생성할 수 있다. 독립적인 궤적 검증(Independent Trajectory Validation)은 실행 전에 충돌 위험, 곡률, 속도, 가속도 및 운용 경계를 검사할 수 있다. 마찬가지로 제어 모니터링(Control Monitoring)은 명령된 차량 움직임과 실제 측정된 움직임을 비교하여 과도한 추종 오차, 휠 슬립(Wheel Slip), 액추에이터 포화(Actuator Saturation), 조향 고장 또는 제동 성능 저하를 식별할 수 있다.

타이밍 고장(Timing Failure)은 분산형 자율주행 아키텍처(Distributed Autonomy Architecture)에서 특히 중요하다. 인지 결과가 논리적으로 정확하더라도 안전한 제어에 사용하기에는 너무 오래된 정보일 수 있으며, 차량이 계속 움직이는 동안 계획기가 계산 마감시간을 초과할 수도 있다. 따라서 안전 모니터링은 데이터 수명(Data Age), 타임스탬프 일관성(Timestamp Consistency), 실행 마감시간(Execution Deadline), 통신 지연시간(Communication Latency), 제어 루프 타이밍(Control-Loop Timing)을 고려해야 한다. 개별 소프트웨어 모듈이 정상적으로 동작하는 것처럼 보이더라도 정보가 시간적으로 더 이상 유효하지 않다면 고장안전 동작이 실행되어야 한다.

전원 및 에너지 고장(Power and Energy Failure)도 폴백 아키텍처에 통합되어야 한다. 배터리 충전상태(State of Charge), 전압, 온도, 전류 제한, 전력분배 고장(Power-Distribution Fault), 컴퓨팅 전력 가용성은 로봇이 임무를 완료하거나 안전 상태에 도달할 수 있는지에 영향을 줄 수 있다. 에너지 관리(Energy Management)는 안전에 중요한 기능과 폴백 기동을 수행하기 위한 충분한 용량을 확보해야 한다. 로봇은 임무를 완료하는 데 남은 에너지를 모두 소비하여 제동, 통신, 안전 주차 또는 복구에 필요한 에너지를 사용할 수 없게 해서는 안 된다.

통신 손실(Communication Loss)은 자동적으로 위험한 동작을 발생시키는 것이 아니라 자율주행 아키텍처에 따라 처리되어야 한다. 차량군 관리(Fleet Management), 원격 감독(Remote Supervision), 클라우드 서비스(Cloud Service), 원격 지원은 임무 운용에 유용할 수 있지만, 즉각적인 물리적 안전은 일반적으로 로컬 시스템에서 계속 지원되어야 한다. 통신이 손실되면 로봇은 ODD 요구사항과 사전에 정의된 규칙에 따라 허용된 로컬 동작을 계속하거나, 짧은 폴백 기동을 완료하거나, 안전한 위치에서 정지하거나, 통신이 복구될 때까지 대기할 수 있다.

계층형 안전 아키텍처(Layered Safety Architecture)는 하나의 자율주행 기능 고장이 곧바로 위험한 물리적 움직임으로 이어지는 것을 방지하는 데 도움이 된다. 주 자율주행 스택(Primary Autonomy Stack)은 인지, 위치추정, 계획 및 제어를 수행하고, 독립적인 모니터는 시스템 상태, 궤적 유효성(Trajectory Validity), ODD 준수 및 차량 응답을 평가한다. 안전 제어기(Safety Controller) 또는 감독 계층(Supervisory Layer)은 필요한 경우 정상 명령을 재정의(Override)할 수 있다. 비상정지는 최종 보호 계층(Final Protective Layer)을 구성할 수 있지만, 모든 성능 저하 상황에 적용되는 유일한 전략이 되어서는 안 된다.

복구(Recovery)는 고장 관리(Fault Management)의 또 다른 핵심 요소이다. 일부 고장은 일시적이며 모듈 재시작, 센서 전환, 재위치추정(Re-localization), 재계획(Re-planning), 통신 재설정 또는 환경 조건이 개선될 때까지 기다리는 방식으로 해결할 수 있다. 복구 로직(Recovery Logic)은 정상 모드와 성능저하 모드 사이에서 반복적으로 전환되는 현상을 방지해야 하며, 전체 자율주행 능력을 복원하기 전에 근본적인 문제가 실제로 허용 가능한 상태로 돌아왔는지를 검증해야 한다.

고장안전 및 고장운용 동작의 검증(Validation)은 정상적인 임무만 시험하는 것이 아니라 의도적인 고장 주입(Fault Injection)을 필요로 한다. 센서 손실, 손상된 측정값, 지연된 메시지, 위치추정 드리프트, 통신 중단, 액추에이터 성능 저하, 전원 고장, 차단된 경로 및 컴퓨팅 과부하를 시뮬레이션(Simulation), 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL), 실제 물리 시험(Physical Testing)에 적용할 수 있다. 목적은 단순히 고장이 감지되는지를 확인하는 것이 아니라 전체 시스템이 의도된 성능저하 상태와 폴백 상태를 거쳐 올바르게 전환되는지를 검증하는 것이다.

최종 설계에서는 모든 중요한 고장 모드(Failure Mode)를 감지(Detection), 격리(Isolation), 성능 저하(Degradation), 폴백(Fallback), 복구(Recovery), 검증 증거(Validation Evidence)와 연결해야 한다. 이를 통해 위험 분석(Hazard Analysis)에서 런타임 동작(Runtime Behavior)과 시험 결과까지 이어지는 추적성(Traceability)을 구축할 수 있다. 따라서 야외 AMR에서 고장안전 및 고장운용 공학(Fail-Safe and Fail-Operational Engineering)은 하나의 비상정지 기능이 아니라 시스템 수준의 아키텍처가 된다. 목표는 고장이 발생했을 때 시스템 능력이 통제되고 예측 가능한 방식으로 감소하도록 하면서, 허용할 수 없는 물리적 위험을 방지하는 데 필요한 기능을 유지하는 것이다.

## 01.08. Disengagement Analysis and Edge Case Mining

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

디스인게이지먼트 분석(Disengagement Analysis)과 엣지 케이스 마이닝(Edge-Case Mining)은 자율주행 차량(Autonomous Vehicle, AV) 또는 자율이동로봇(Autonomous Mobile Robot, AMR)이 자율운용 능력의 한계에 도달하는 지점을 이해하기 위한 핵심 과정이다. 디스인게이지먼트(Disengagement)는 시스템, 안전 감독자(Safety Supervisor), 원격 운영자(Remote Operator) 또는 탑승 운전자가 자율제어를 계속하는 것이 적절하지 않거나 안전하지 않다고 판단하여 자율운용이 중단되는 상황을 의미한다. 개발팀은 이를 단순한 실패로 취급하기보다 인지, 위치추정, 계획, 제어, 시스템 통합 및 운용설계영역(Operational Design Domain, ODD) 가정의 취약점을 파악하기 위한 가치 높은 증거로 활용할 수 있다.

디스인게이지먼트는 사건 발생 전, 발생 중, 발생 후에 어떤 일이 있었는지를 재구성할 수 있을 정도로 충분한 상황 정보와 함께 기록되어야 한다. 유용한 정보에는 타임스탬프가 포함된 센서 데이터, 차량 자세, 속도, 계획된 궤적, 제어 명령, 위치추정 신뢰도, 객체 추적 정보(Object Tracks), 시스템 상태(System Health), ODD 상태, 소프트웨어 상태, 운영자 동작 및 환경 조건 등이 포함된다. 최종 개입 상황만 기록하면 실제 원인이 가시적인 고장이 발생하기 수초 또는 수분 전부터 진행되었을 수 있기 때문에 공학적 활용 가치가 제한된다.

디스인게이지먼트 분석은 트리거(Trigger)와 근본원인(Root Cause)을 구분하는 것에서 시작한다. 운영자는 로봇이 장애물에 지나치게 가까이 접근하기 때문에 개입할 수 있지만, 실제 근본원인은 지연된 인지, 부정확한 객체 추적, 위치추정 드리프트(Localization Drift), 부적절한 계획 비용(Planning Cost), 과도한 제어 오차 또는 통신 지연으로 발생한 오래된 데이터(Stale Data)일 수 있다. 따라서 효과적인 분석은 모든 사건을 최종적인 가시적 증상을 발생시킨 하위 시스템에 단순히 할당하는 대신 인과관계 사슬(Causal Chain)을 재구성해야 한다.

구조화된 분류체계(Structured Taxonomy)는 많은 사건을 일관성 있게 분석하는 데 도움이 된다. 디스인게이지먼트는 인지(Perception), 위치추정(Localization), 예측(Prediction), 계획(Planning), 궤적생성(Trajectory Generation), 제어(Control), 하드웨어(Hardware), 통신(Communication), 인간 상호작용(Human Interaction), ODD 위반(ODD Violation), 안전 시스템 작동(Safety-System Activation) 등에 따라 분류할 수 있다. 추가 속성으로 심각도, 환경 조건, 복구 가능성, 개입 유형, 영향을 받은 임무, 그리고 해당 사건이 알려진 문제인지 이전에 발견되지 않은 새로운 고장 모드(Failure Mode)인지를 기록할 수 있다.

소프트웨어 릴리스(Software Release) 또는 운용 기간 사이의 디스인게이지먼트 통계를 비교하려면 정규화(Normalization)가 필요하다. 운용거리, 운용시간, 임무 횟수, 경로 복잡도, 교통 밀도, 기상 또는 지형에 따라 노출도(Exposure)가 달라지기 때문에 단순한 사건 발생 횟수는 오해를 일으킬 수 있다. 킬로미터당 개입 횟수, 개입 사이의 자율운용시간 또는 임무당 디스인게이지먼트와 같은 지표는 ODD 및 시나리오 정보와 함께 사용할 때 더욱 유용하다. 개입 빈도의 감소는 기본적인 운용 노출 조건이 충분히 비교 가능한 경우에만 의미가 있다.

엣지 케이스(Edge Case)는 발생 빈도가 낮거나 시스템의 학습, 검증 또는 운용 경험의 경계 부근에 존재하면서도 상대적으로 큰 영향을 발생시킬 수 있는 상황이다. 예를 들면 특이한 객체 형상, 부분적으로 가려진 보행자, 예상하지 못한 차량 행동, 임시 도로 구조물, 반사 표면, 극단적인 조명 조건, 드문 지형 형상, 비정상적인 적재 조건, 센서 오염, GNSS 다중경로(Multipath), 또는 개별적으로는 익숙하지만 이전에 함께 나타난 적이 없는 여러 조건의 조합 등이 있다.

모든 희귀 사건이 중요한 엣지 케이스인 것은 아니다. 엣지 케이스 마이닝은 안전 관련성(Safety Relevance), 신규성(Novelty), 불확실성(Uncertainty), 발생 빈도 및 임무 성능에 미칠 잠재적 영향에 따라 상황의 우선순위를 결정해야 한다. 로봇의 이동 경로에서 멀리 떨어져 있는 시각적으로 특이한 객체는 운용상 중요성이 낮을 수 있지만, 정지거리 경계 부근에 존재하는 약간 특이한 장애물은 즉각적인 분석이 필요할 수 있다. 따라서 목표는 의사결정 경계(Decision Boundary)에 도전하거나 시스템의 강건성(Robustness) 부족을 드러내는 희귀 상황을 식별하는 것이다.

마이닝(Mining)은 대규모 운용 데이터 수집에서 시작된다. 차량군 로그(Fleet Logs)에는 수천 시간의 운용 과정에서 수집된 카메라 영상, 라이다 포인트 클라우드(LiDAR Point Cloud), 레이더 관측, 위치추정 결과, 궤적, 제어 상태, 진단 정보 및 임무 정보가 포함될 수 있다. 모든 데이터를 수동으로 검사하는 것은 현실적으로 어렵기 때문에 자동 필터(Automated Filter)를 사용하여 개입, 비상제동, 비정상적인 가속, 낮은 위치추정 신뢰도, 높은 예측 불확실성, 장애물 근접 상황, 반복적인 재계획, 안전 모니터 작동 또는 기타 비정상 동작 지표가 포함된 데이터 구간을 식별해야 한다.

이벤트 트리거 로깅(Event-Triggered Logging)은 가치가 높은 정보를 보존하면서 저장공간과 분석 요구량을 줄일 수 있다. 모든 고대역폭 신호를 무기한 저장하는 대신 시스템은 순환 버퍼(Rolling Buffer)를 유지하고 특정 트리거가 발생했을 때 선택된 시간 구간을 저장할 수 있다. 이벤트 발생 전 데이터(Pre-Event Data)는 상황이 어떻게 전개되었는지를 설명하고, 이벤트 발생 후 데이터(Post-Event Data)는 복구 동작과 2차 영향을 보여준다. 여러 유형의 트리거를 결합하여 안전 사건, 불확실성 급증, 센서 이상 및 비정상적인 계획기 동작이 자동으로 분석 패키지(Analysis Package)를 생성하도록 구성할 수 있다.

불확실성(Uncertainty)은 강력한 엣지 케이스 발견 신호로 활용될 수 있다. 신경망 기반 인지 모델은 낮은 신뢰도, 모델 간 불일치, 연속 프레임 사이의 불안정한 예측 또는 기존 학습 데이터와 크게 다른 표현(Representation)을 생성할 수 있다. 위치추정 시스템은 증가하는 공분산(Covariance)을 보고할 수 있으며, 계획기는 여러 후보 궤적 사이에서 반복적으로 전환할 수 있다. 이러한 신호가 반드시 고장의 존재를 의미하는 것은 아니지만, 보다 심층적인 검토가 필요한 운용 데이터 구간의 우선순위를 결정하고 수동으로 정의된 트리거에 대한 의존성을 줄일 수 있다.

유사도 분석(Similarity Analysis)과 클러스터링(Clustering)은 대규모 후보 사건 집합을 체계적으로 정리하는 데 도움이 된다. 이미지, 포인트 클라우드, 장면, 궤적 또는 시스템 상태 벡터에서 추출된 특징 임베딩(Feature Embedding)을 사용하여 유사한 특성을 가진 사건을 그룹화할 수 있다. 반복적으로 나타나는 클러스터는 구조적인 취약점을 나타낼 수 있으며, 고립된 샘플(Isolated Sample)은 실제로 새로운 상황을 나타낼 수 있다. 또한 클러스터링을 사용하면 동일한 근본 결함으로 발생한 수백 개의 거의 동일한 디스인게이지먼트를 엔지니어가 반복 분석하는 것을 방지할 수 있다.

시나리오 재구성(Scenario Reconstruction)은 실제 운용 사건을 재현 가능한 공학적 자산(Engineering Asset)으로 변환한다. 센서 기록, 지도 정보, 동적 객체 궤적, 환경 조건 및 로봇 상태를 사용하여 시뮬레이션 또는 통제된 시험 환경에서 사건을 재현할 수 있다. 유용한 재구성 시나리오는 원래 문제를 발생시킨 핵심 요인을 보존하면서 차량 속도, 보행자 위치, 장애물 크기, 조명, 마찰계수 또는 위치추정 오차와 같은 매개변수를 체계적으로 변경할 수 있어야 한다.

매개변수 변화(Parameter Variation)는 하나의 발견된 엣지 케이스를 관련된 시험 사례(Test Case)의 집합으로 확장한다. AMR이 주차된 차량 뒤에서 갑자기 나타난 보행자로 인해 디스인게이지먼트되었다면 기록된 정확한 궤적만 시험하는 것으로는 충분한 검증 범위를 확보하기 어렵다. 대신 보행자 속도, 출현 시점, 가림 수준(Occlusion Level), 로봇 속도, 제동 능력, 센서 노이즈 및 조명을 변화시킬 수 있다. 이를 통해 원래 실패 상황 주변의 경계를 형성하고 자율주행 시스템이 성공과 실패 사이에서 전환되는 조건을 파악할 수 있다.

엣지 케이스 마이닝은 데이터 및 학습 파이프라인(Data and Learning Pipeline)과 직접 연결되어야 한다. 확인된 인지 고장은 어노테이션 대기열(Annotation Queue)에 추가할 수 있고, 어려운 장면은 표적 학습 샘플(Targeted Training Sample)로 활용할 수 있으며, 합성 데이터(Synthetic Data)를 이용해 희귀한 환경 조건의 조합을 확장할 수 있다. 반면 계획 또는 제어 고장은 학습 데이터보다 회귀시험 시나리오(Regression Scenario)를 생성하는 것이 적합할 수 있다. 따라서 모든 디스인게이지먼트가 단순히 더 많은 신경망 학습 데이터를 생성해야 한다고 가정하는 대신 고장 메커니즘에 따라 적절한 개선 조치를 선택해야 한다.

회귀시험(Regression Testing)은 수정된 엣지 케이스가 소프트웨어가 발전한 이후에도 계속 해결된 상태로 유지되는지를 보장한다. 디스인게이지먼트가 재현되고 근본원인이 파악되면 가능한 경우 해당 시나리오를 자동화된 검증 스위트(Automated Validation Suite)의 일부로 포함해야 한다. 이후의 소프트웨어 버전은 이전에 발견된 실패 사례를 대상으로 반복 시험할 수 있다. 이를 통해 현장 경험을 축적되는 조직적 지식(Organizational Knowledge)으로 전환하고 관련 없는 소프트웨어 변경 이후 과거 문제가 조용히 다시 발생할 가능성을 줄일 수 있다.

엣지 케이스 데이터베이스(Edge-Case Database)는 메타데이터(Metadata)와 추적성(Traceability)을 보존해야 한다. 각 사건은 원본 운용 로그, 분류, 근본원인 분석, 영향을 받은 소프트웨어 버전, ODD 조건, 수정 조치, 재구성된 시나리오, 검증 결과 및 회귀시험 식별자(Regression-Test Identifier)와 연결될 수 있다. 중복되거나 서로 관련된 사건은 독립적으로 처리하기보다 상호 연결해야 한다. 시간이 지나면서 이 데이터베이스는 알려진 자율주행 한계와 이를 해결하기 위해 사용된 증거를 구조적으로 표현하는 자산으로 발전한다.

차량군 규모 운용(Fleet-Scale Operation)은 여러 로봇이 하나의 개발 차량보다 훨씬 넓은 범위의 환경 조건을 집단적으로 탐색할 수 있다는 중요한 장점을 제공한다. 중앙집중형 분석(Centralized Analytics)은 서로 다른 위치, 로봇 구성, 센서 버전, 소프트웨어 릴리스 및 임무 유형에 따른 사건 패턴을 비교할 수 있다. 하나의 로봇이 의미 있는 엣지 케이스를 발견하면 검증 이후 해당 시나리오, 모델 업데이트, 계획 수정 또는 안전 규칙을 전체 차량군의 성능 향상에 활용할 수 있다.

디스인게이지먼트 분석은 자율주행 시스템의 한계와 부적절한 ODD 노출(Inappropriate ODD Exposure)도 구분해야 한다. 로봇이 애초에 지원하도록 설계되지 않은 지형, 기상, 교통 상호작용, 통신 조건 또는 환경 형상을 경험할 수 있다. 이러한 경우 수정 조치는 자율주행 알고리즘 자체의 변경이 아니라 ODD 모니터링 개선, 경로 제한(Route Restriction), 운용 정책(Operational Policy) 또는 배치 구성(Deployment Configuration)의 변경일 수 있다. 이러한 구분은 시스템 요구사항이 통제되지 않은 방식으로 계속 확장되는 것을 방지한다.

성숙한 프로세스는 실제 운용에서 개선으로 이어지는 폐루프 개발 과정(Closed Development Loop)을 형성한다. 차량군 운용을 통해 사건이 생성되고, 자동화된 마이닝이 후보를 식별하며, 엔지니어가 이를 분류하고 분석한 뒤 중요한 사례를 재구성한다. 이후 수정 사항을 구현하고 시뮬레이션 또는 실제 물리 시험을 통해 결과를 검증한 후 다시 배포한다. 새로운 현장 데이터는 비교 가능한 조건에서 개입 빈도와 고장 패턴이 실제로 변화했는지를 다시 측정한다.

디스인게이지먼트 분석과 엣지 케이스 마이닝의 궁극적인 목적은 상황적 맥락을 고려하지 않은 채 디스인게이지먼트 횟수를 단순히 0으로 만드는 것이 아니다. 목표는 자율운용 능력의 경계를 체계적으로 발견하고, 그러한 경계가 존재하는 이유를 이해하며, 실제 운용 경험을 측정 가능한 개선으로 변환하는 것이다. 야외 AMR에서 이러한 과정은 실제 환경 운용, ODD 관리, 데이터 엔지니어링(Data Engineering), 시뮬레이션(Simulation), 검증(Validation), 안전 보증(Safety Assurance), 자율주행 시스템 개발 사이를 지속적으로 확장되는 하나의 연결 구조로 만든다.

## 01.09. Data Driven AV Development Closed Loop Approach

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

데이터 기반 개발(Data-Driven Development)은 운용 데이터를 자율주행 차량(Autonomous Vehicle, AV)과 자율이동로봇(Autonomous Mobile Robot, AMR) 시스템을 지속적으로 개선하기 위한 핵심 공학 자원으로 활용한다. 사전에 정의된 요구사항과 수동으로 설계된 시험 사례에만 의존하는 대신 실제 시스템 동작으로부터 학습하도록 개발 프로세스를 구성한다. 차량군 운용(Fleet Operation), 통제된 시험(Controlled Testing), 시뮬레이션(Simulation), 검증(Validation)은 인지, 위치추정, 계획, 제어, 안전 및 운용설계영역(Operational Design Domain, ODD) 성능에 대한 증거를 지속적으로 생성한다.

폐루프 접근법(Closed-Loop Approach)은 실제 배포(Deployment)를 개발 과정과 직접 연결한다. 로봇이 현장에서 운용되고, 관련 데이터가 수집되며, 중요한 사건을 발견하고, 엔지니어링 팀이 취약점을 분석한 후 개선 사항을 구현하고, 업데이트된 소프트웨어를 다시 배포하기 전에 검증한다. 이후 다음 운용 주기에서 이러한 변경이 실제로 효과가 있었는지를 보여주는 새로운 증거가 생성된다. 이러한 반복적인 피드백 메커니즘(Feedback Mechanism)은 자율 시스템 개발을 주로 순차적인 프로세스에서 지속적으로 진화하는 공학적 순환 구조로 전환한다.

이 프로세스는 사용 가능한 모든 신호를 제한 없이 기록하는 것이 아니라 체계적인 데이터 수집(Systematic Data Collection)에서 시작한다. 카메라, 라이다(LiDAR), 레이더(Radar), GNSS, 관성 센서(Inertial Sensor), 차량 제어기, 진단 시스템, 위치추정 모듈, 계획기 및 안전 시스템은 막대한 양의 데이터를 생성할 수 있다. 따라서 로깅 정책(Logging Policy)은 상황 재구성, 성능 측정, 고장 분석 및 학습에 필요한 신호를 식별해야 한다. 소프트웨어 버전, 센서 구성, 경로, 임무, ODD 상태 및 타임스탬프와 같은 메타데이터(Metadata)도 동일하게 중요하다.

이벤트 트리거 기반 수집(Event-Triggered Collection)은 저장공간과 엔지니어링 자원을 가치가 높은 운용 구간에 집중할 수 있도록 한다. 디스인게이지먼트(Disengagement), 비상제동(Emergency Braking), 안전 모니터 작동(Safety-Monitor Activation), 낮은 위치추정 신뢰도, 인지 불확실성, 장애물 근접 상황, 반복적인 재계획, 제어 불안정, 통신 손실 및 ODD 위반은 상세 기록을 자동으로 트리거할 수 있다. 순환 버퍼(Rolling Buffer)는 이벤트 전후의 정보를 보존하여 엔지니어가 문제 상황이 어떻게 전개되었으며 시스템이 어떻게 대응했는지를 재구성할 수 있도록 한다.

데이터가 많다고 해서 자동으로 더 우수한 자율 시스템이 만들어지는 것은 아니기 때문에 데이터 선택(Data Selection)은 매우 중요하다. 단순한 경로에서 수집된 수천 개의 거의 동일한 관측 데이터보다 소수의 어렵거나 새로운 상황이 더 많은 정보를 제공할 수 있다. 따라서 데이터 파이프라인(Data Pipeline)은 다양성(Diversity), 신규성(Novelty), 불확실성(Uncertainty), 환경 커버리지(Environmental Coverage), 안전 관련성(Safety Relevance)을 측정해야 한다. 샘플링 전략(Sampling Strategy)은 희귀한 기상, 특이한 장애물, 복잡한 상호작용, 성능이 저하된 센서, 어려운 지형 또는 ODD 경계에 가까운 운용 조건을 중점적으로 선택할 수 있다.

수집된 데이터는 유용하게 활용되기 전에 수집처리(Ingestion), 동기화(Synchronization), 품질 검사(Quality Checking), 인덱싱(Indexing), 저장(Storage) 과정을 거쳐야 한다. 서로 다른 주기로 동작하는 센서 스트림에는 일관된 타임스탬프와 좌표계(Coordinate Frame)가 필요하다. 누락된 패킷, 손상된 파일, 캘리브레이션(Calibration) 변경, 센서 교체 및 소프트웨어 버전 차이를 식별해야 한다. 신뢰할 수 있는 데이터 플랫폼(Data Platform)은 모든 학습 샘플, 시나리오, 지표 또는 검증 결과를 해당 데이터가 생성된 실제 운용 소스와 구성까지 추적할 수 있도록 데이터 출처정보(Provenance)를 보존한다.

자동화된 마이닝(Automated Mining)은 대규모 운용 데이터셋을 관리 가능한 엔지니어링 후보군으로 변환한다. 규칙 기반 방법은 작은 장애물 여유거리나 높은 추종 오차와 같이 사전에 정의된 조건을 검색할 수 있으며, 통계적 방법은 이상치(Outlier)와 비정상적인 상태 조합을 식별할 수 있다. 머신러닝 모델(Machine-Learning Model)은 불확실성, 예측 불일치 또는 기존 데이터와의 임베딩 거리(Embedding Distance)를 사용하여 장면의 우선순위를 결정할 수 있다. 결정론적 트리거(Deterministic Trigger)와 학습 기반 마이닝을 결합하면 알려진 고장 패턴과 이전에 발견되지 않은 상황을 모두 탐색할 수 있다.

자동화된 마이닝 이후에도 인간의 분석(Human Analysis)은 중요하다. 엔지니어는 후보 데이터가 센서 아티팩트(Sensor Artifact), 라벨링 문제, 소프트웨어 결함, 환경적 한계, ODD 위반 또는 실제로 새로운 시나리오인지를 판단할 수 있다. 근본원인 분석(Root-Cause Analysis)은 관찰된 동작을 해당 동작을 발생시킨 하위 시스템과 조건에 연결해야 한다. 이를 통해 모든 어려운 사건을 인지 문제로 간주하고 데이터를 추가하는 오류를 방지할 수 있으며, 실제 수정 대상이 계획, 제어, 하드웨어, 캘리브레이션 또는 운용 정책일 수 있음을 구분할 수 있다.

학습 기반 구성요소(Learning-Based Component)가 사용되는 경우 선택된 데이터는 어노테이션 및 데이터셋 관리 파이프라인(Annotation and Dataset-Management Pipeline)으로 전달된다. 모델의 목적에 따라 객체, 주행가능영역(Drivable Area), 지형, 차선, 자유공간(Free Space), 의미 클래스(Semantic Class), 궤적 또는 기타 작업별 대상에 라벨을 지정할 수 있다. 데이터셋 버전(Dataset Version)은 원본 데이터, 어노테이션 규칙, 클래스 정의, 품질 검사 및 학습-검증-시험 분할(Train-Validation-Test Partition)을 기록해야 한다. 버전 관리를 통해 모델 결과를 재현할 수 있으며 서로 다른 가정으로 수집된 데이터셋이 통제되지 않은 상태로 혼합되는 것을 방지할 수 있다.

학습(Training)은 단순히 추가 데이터가 존재한다는 이유로 수행하는 것이 아니라 측정 가능한 취약점과 연결되어야 한다. 분석 결과 역광 조건에서 보행자 검출 성능이 낮은 것으로 나타났다면 다음 데이터셋에서는 관련 조명 및 가림 조건의 커버리지를 의도적으로 증가시킬 수 있다. 젖은 자갈길에서 지형 분류(Terrain Classification)가 실패한다면 표적화된 실제 데이터(Targeted Real Data)와 합성 변형 데이터를 추가할 수 있다. 따라서 데이터 기반 개발은 데이터셋 구축을 수동적인 축적 과정에서 관찰된 시스템 한계에 의해 유도되는 능동적인 과정으로 변화시킨다.

합성 데이터(Synthetic Data)와 시뮬레이션은 실제 환경에서의 데이터 수집 비용이 높거나 위험하거나 통계적으로 드문 상황에 대한 커버리지를 확장할 수 있다. 중요한 시나리오가 식별되면 시뮬레이션에서 객체 위치, 차량 속도, 기상, 조명, 마찰, 센서 노이즈, 지형 및 교통 동작을 변화시킬 수 있다. 도메인 랜덤화(Domain Randomization)와 매개변수 스윕(Parameter Sweep)을 사용하면 발견된 취약점 주변에서 대규모 시나리오 집합을 생성할 수 있다. 실제 데이터는 운용 관련성을 제공하고, 시뮬레이션은 제어 가능성, 반복 가능성 및 확장 가능한 커버리지를 제공한다.

모델 평가(Model Evaluation)는 전체적인 정확도 지표만으로 제한되어서는 안 된다. 성능은 기상, 조명, 지형, 속도, 객체 거리, 센서 상태 및 상호작용 복잡도와 같은 ODD 차원을 나타내는 시나리오 슬라이스(Scenario Slice)별로 측정해야 한다. 새로운 모델이 평균 정확도는 향상시키면서도 안전에 중요한 특정 부분집합에서 성능이 저하될 수 있다. 따라서 릴리스 결정(Release Decision)은 전체 지표뿐만 아니라 알려진 취약점과 운용 위험에 연결된 표적 회귀시험(Targeted Regression Test) 결과도 함께 검토해야 한다.

폐루프에는 머신러닝에 직접 의존하지 않는 계획, 제어 및 시스템 수준 소프트웨어도 포함된다. 현장 데이터는 비효율적인 경로, 계획기 진동(Planner Oscillation), 교착상태(Deadlock), 불편한 궤적, 추종 오차, 과도한 제동 또는 부적절한 폴백 동작(Fallback Behavior)을 발견할 수 있다. 기록된 사건은 시뮬레이션에서 재구성하고 회귀시험(Regression Test)으로 변환하여 알고리즘이나 매개변수를 수정하는 데 사용할 수 있다. 따라서 데이터 기반 개발은 신경망 학습보다 넓은 개념이며 전체 자율주행 스택(Autonomy Stack)에 적용된다.

지속적 검증(Continuous Validation)은 한 영역의 개선이 다른 영역에서 회귀(Regression)를 발생시키는 것을 방지한다. 후보 소프트웨어는 과거의 실패 사례, 대표적인 ODD 시나리오, 안전에 중요한 사례 및 새롭게 발견된 엣지 케이스(Edge Case)를 대상으로 시험해야 한다. 시뮬레이션은 대규모 회귀시험을 가능하게 하며, 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL), 시험장 시험(Proving-Ground Testing), 현장 시험(Field Testing)을 통해 점진적으로 물리적 현실성을 높일 수 있다. 릴리스 게이트(Release Gate)는 기능 성능, 안전 요구사항, 계산 자원 한계 및 회귀시험 기준을 결합할 수 있다.

현장 피드백을 신뢰성 있게 해석하려면 배포(Deployment)가 통제되어야 한다. 각 로봇에 적용된 소프트웨어 버전, 구성 매개변수(Configuration Parameter), 하드웨어 리비전(Hardware Revision), 지도 및 모델 버전을 추적할 수 있어야 한다. 새로운 릴리스는 먼저 차량군의 제한된 일부에 배포하고, 더 광범위하게 적용하기 전에 기존 기준선(Baseline)과 성능을 비교할 수 있다. 모니터링을 통해 비교 가능한 운용 조건에서 개입 빈도, 임무 성공률, 지연시간, 에너지 소비, 안전 사건 및 기타 KPI(Key Performance Indicator)가 개선되는지를 판단해야 한다.

차량군 규모 운용(Fleet-Scale Operation)은 폐루프의 효과를 크게 증가시킨다. 서로 다른 위치에서 운용되는 여러 로봇은 지형, 기상, 인간 행동, 인프라 및 임무 조건에 대한 더욱 넓은 분포를 집단적으로 경험할 수 있다. 중앙집중형 분석(Central Analytics)은 반복적으로 발생하는 패턴을 식별하고 특정 사건이 개별적인 것인지 구조적인 것인지를 판단할 수 있다. 하나의 로봇에서 발견된 의미 있는 사례는 검증 이후 다른 로봇에도 도움이 되는 회귀시험 시나리오, 학습 샘플 또는 안전 규칙으로 전환할 수 있다.

모든 현장 문제를 자율주행 능력 확장으로 해결해야 하는 것은 아니므로 ODD 관리(ODD Management)는 데이터 폐루프와 지속적으로 연결되어야 한다. 운용 데이터는 특정 지형, 기상, 교통 조건 또는 위치추정 환경이 검증된 시스템 능력의 범위를 지속적으로 벗어나고 있음을 보여줄 수 있다. 이에 대한 공학적 대응은 자율주행 시스템의 개선일 수도 있지만, 경로 제한, 배치 조건 변경, 런타임 ODD 모니터링(Runtime ODD Monitoring) 강화 또는 성능저하 운용 모드(Degraded Operating Mode)의 정의일 수도 있다. 따라서 데이터는 능력 확장과 운용 경계의 강제 적용을 모두 지원한다.

폐루프가 확장될수록 거버넌스(Governance)와 추적성(Traceability)은 더욱 중요해진다. 데이터셋 버전, 모델 버전, 소프트웨어 릴리스, 캘리브레이션 파일, 시나리오 라이브러리(Scenario Library), 시험 결과, 안전 요구사항 및 운용 사건 사이에는 식별 가능한 관계가 유지되어야 한다. 엔지니어는 어떤 데이터가 특정 모델을 생성했는지, 어떤 소프트웨어가 해당 모델을 사용했는지, 어떤 시험이 릴리스를 뒷받침했는지, 그리고 배포 이후 어떤 현장 결과가 발생했는지를 확인할 수 있어야 한다. 이러한 계보(Lineage)는 지속적인 개발 과정을 감사 가능한 공학 프로세스(Auditable Engineering Process)로 전환한다.

성숙한 폐루프 아키텍처(Closed-Loop Architecture)는 궁극적으로 차량군 운용, 데이터 엔지니어링(Data Engineering), 엣지 케이스 마이닝(Edge-Case Mining), 어노테이션(Annotation), 학습(Training), 시뮬레이션, 검증, 배포 및 모니터링을 하나의 지속적인 시스템으로 연결한다. 목적은 단순히 대규모 데이터셋을 축적하거나 모델을 자주 재학습하는 것이 아니다. 실제 운용 증거를 우선순위가 정해진 공학적 조치로 변환하고, 이러한 조치가 측정 가능한 시스템 동작을 실제로 개선하는지 검증하며, 그 결과로 얻은 지식을 다음 개발 주기에 다시 반영하는 것이 핵심이다.

야외 AMR에서는 초기 개발 과정에서 환경의 변동성을 완전하게 표현할 수 없기 때문에 이러한 접근법이 특히 중요하다. 지형 변화, 기상, 공사 활동, 보행자, 차량, 센서 성능 저하, 통신 조건 및 임무 요구사항은 지속적으로 새로운 조합을 만들어낸다. 데이터 기반 폐루프(Data-Driven Closed Loop)를 사용하면 자율주행 시스템과 ODD 정의가 함께 발전하면서도 현장 증거(Field Evidence), 공학적 변경(Engineering Change), 검증 결과(Validation Result), 운용 성능(Operational Performance) 사이의 측정 가능한 연결 관계를 지속적으로 유지할 수 있다.

## 01.10. Robotics Outdoor AMR Autonomy Vision

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

로보틱스(Robotics)의 야외 자율이동로봇(Autonomous Mobile Robot, AMR) 자율주행 비전(Autonomy Vision)은 모바일 로보틱스(Mobile Robotics)를 구조화된 실내 환경에서 벗어나 지형, 기상, 위치추정 품질, 통신 조건, 사람 및 차량과의 상호작용이 지속적으로 변화하는 대규모 야외 운용으로 확장하는 것을 목표로 한다. 이는 단순히 실내용 AMR을 야외에서 운용하는 것이 아니라, 점점 복잡해지는 운용설계영역(Operational Design Domain, ODD)에서 안정적으로 인지하고, 추론하고, 계획하고, 제어하고, 복구하며, 임무를 관리할 수 있는 자율주행 아키텍처(Autonomy Architecture)를 구축하는 것을 의미한다.

개발 방향은 실내 자율주행(Indoor Autonomy)에서 야외 자율주행(Outdoor Autonomy)으로, 궁극적으로는 협력형 차량군 지능(Coordinated Fleet Intelligence)으로 발전하는 과정으로 볼 수 있다. 실내 AMR은 내비게이션(Navigation), 장애물 회피(Obstacle Avoidance), 임무 수행(Mission Execution), 충전(Charging), 로봇 관리 시스템(Robot Management System, RMS) 통합을 통해 기반을 제공한다. 야외 플랫폼은 여기에 더욱 강력한 위치추정, 환경 인지, 지형 이해, 차량 수준 제어, 안전 감독(Safety Supervision), 그리고 훨씬 크고 무거운 로봇 시스템에 적합한 컴퓨팅 자원을 추가한다.

야외 자율주행은 기존 실내 내비게이션과 다른 방식으로 환경을 해석해야 한다. 실내 로봇은 일반적으로 비교적 평탄한 바닥과 예측 가능한 기하 구조에서 운용되지만, 야외 AMR은 경사면, 연석, 불균일한 포장면, 자갈, 식생, 공사 구역, 물웅덩이, 손상된 노면 및 임시 장애물을 만날 수 있다. 따라서 자율주행 시스템은 자유공간(Free Space)뿐만 아니라 주행가능영역(Drivable Area), 지형 클래스(Terrain Class), 주행가능성(Traversability), 노면 상태, 경사도, 여유공간(Clearance), 특정 영역에 진입할 때 발생하는 위험까지 추정해야 한다.

다중모달 센싱(Multi-Modal Sensing)은 이러한 환경 이해의 기반을 형성한다. 카메라(Camera)는 외관, 의미 정보(Semantic Information), 객체 인식(Object Recognition), 시각적 맥락을 제공하며, 라이다(LiDAR)는 기하학적 구조와 장애물 거리를 제공한다. GNSS RTK는 전역 좌표 기준의 야외 위치추정을 지원하고, 관성 센싱(Inertial Sensing)은 단기적인 움직임 정보를 유지하며, 초음파 센서(Ultrasonic Sensor)는 추가적인 근거리 탐지 범위를 제공한다. 선택적으로 고해상도 라이다, 레이더(Radar), 팬-틸트-줌 센싱(Pan-Tilt-Zoom Sensing)을 적용하여 임무 요구사항과 환경 복잡도에 따라 인지 능력을 확장할 수 있다.

센서 융합(Sensor Fusion)은 이러한 이질적인 관측 정보를 주변 세계에 대한 일관된 표현으로 변환해야 한다. 시스템은 상위 수준의 추론을 수행하기 전에 측정값을 정확한 타임스탬프(Timestamp), 좌표계(Coordinate Frame), 차량 움직임 및 위치추정 결과와 연결해야 한다. 센서를 독립적인 장치로 취급하는 대신 자율주행 아키텍처는 각 센서의 상호 보완적인 장점과 고장 특성을 평가해야 하며, 하나의 센싱 방식에서 불확실성, 일시적인 성능 저하 또는 손실이 발생하더라도 즉시 통제되지 않은 동작으로 이어지지 않도록 해야 한다.

위치추정(Localization)은 야외 운용의 핵심 기능이다. 내비게이션 품질은 전역 위치(Global Position)와 주변 환경에 대한 로컬 정합(Local Environmental Alignment)에 모두 의존하기 때문이다. GNSS RTK는 적절한 개방 환경에서 고정밀 위치 정보를 제공할 수 있지만 건물, 구조물, 나무, 다중경로(Multipath), 간섭 또는 신호 차단으로 성능이 저하될 수 있다. 따라서 강건한 야외 위치추정(Robust Outdoor Localization)은 필요에 따라 GNSS, 관성 센싱, 휠 움직임, 라이다, 비전(Vision), 지도 정보를 결합하고 위치 신뢰도를 지속적으로 추정하면서 위치추정 성능 저하를 감지해야 한다.

월드 표현(World Representation)은 여러 자율주행 계층을 동시에 지원해야 한다. 기하학적 표현(Geometric Representation)은 장애물, 자유공간, 고도 및 로컬 구조를 제공하며, 의미 정보는 보행자, 차량, 도로, 식생, 인프라 및 임무 관련 객체를 설명한다. 주행가능성 표현(Traversability Representation)은 물리적 플랫폼이 안전하게 이동할 수 있는 영역을 평가한다. 이러한 표현들이 결합되어 인지와 행동 계획(Behavior Planning), 궤적생성(Trajectory Generation), 안전 모니터링(Safety Monitoring), 임무 수준 의사결정(Mission-Level Decision Making)을 연결하는 로컬 월드 모델(Local World Model)을 구성한다.

행동 계획(Behavior Planning)은 운용 상황이 변화할 때 AMR이 무엇을 수행해야 하는지를 결정한다. 로봇은 경로를 추종하거나, 보행자에게 양보하거나, 이동 차량을 기다리거나, 차단된 영역을 우회하거나, 불확실한 지형에서 속도를 줄이거나, 도킹 위치에 접근하거나, 충전 지점으로 복귀하거나, 성능저하 운용 모드(Degraded Operating Mode)로 진입할 수 있다. 행동 선택은 단순히 이동거리만 최적화하는 것이 아니라 임무 우선순위, ODD 조건, 교통 상호작용, 로봇 능력, 적재량(Payload), 에너지 상태, 위치추정 신뢰도 및 안전 제약을 고려해야 한다.

궤적생성(Trajectory Generation)은 행동 결정을 동역학적으로 실행 가능한 움직임으로 변환한다. 야외 플랫폼은 휠베이스(Wheelbase), 조향 기하(Steering Geometry), 회전반경(Turning Radius), 차량 질량, 적재량, 가속도 제한, 제동거리, 경사도, 노면 마찰 및 액추에이터 성능의 영향을 받는다. 기하학적으로 충돌이 없는 궤적이라도 중량급 AMR에는 물리적으로 적합하지 않을 수 있다. 따라서 계획은 환경 이해와 차량 동역학(Vehicle Dynamics)을 연결하고 추종 오차, 불확실성, 장애물 이동 및 비상 대응을 위한 충분한 안전 여유(Safety Margin)를 유지해야 한다.

차량 제어(Vehicle Control)는 자율주행 의사결정과 실제 물리적 움직임을 연결하는 최종 단계이다. 종방향 및 횡방향 제어(Longitudinal and Lateral Control)는 적재량 변화, 경사면, 불균일한 지형, 휠 슬립(Wheel Slip), 조향 응답 및 제동 특성에 강건성을 유지하면서 목표 속도와 경로를 추종해야 한다. 실제 차량 움직임의 피드백(Feedback)은 위치추정, 계획, 진단 및 안전 모니터링으로 지속적으로 반환되어야 하며, 명령된 동작과 측정된 동작 사이의 차이가 위험한 상태로 발전하기 전에 이를 감지할 수 있어야 한다.

안전(Safety)은 하나의 비상정지(Emergency Stop) 기능이 아니라 아키텍처적 속성(Architectural Property)으로 구현되어야 한다. 주 자율주행 스택(Primary Autonomy Stack)은 인지, 위치추정, 계획 및 제어를 수행하고, 독립적인 모니터링 기능은 센서 상태, 위치추정 무결성(Localization Integrity), 궤적 유효성(Trajectory Validity), 차량 응답, 타이밍 및 ODD 준수(ODD Compliance)를 평가할 수 있다. 정상적인 자율운용의 신뢰성이 저하되면 로봇은 속도를 줄이고, 행동을 제한하며, 제어된 폴백(Controlled Fallback)을 수행하고, 안전한 위치로 이동하거나, 최소위험상태(Minimal-Risk Condition)로 전환할 수 있어야 한다.

컴퓨팅 아키텍처(Computing Architecture)는 실시간 자율주행(Real-Time Autonomy)과 점점 발전하는 피지컬 AI(Physical AI) 워크로드를 모두 지원해야 한다. 엣지 컴퓨팅(Edge Computing)은 지연시간에 민감한 인지, 위치추정, 계획, 제어 및 안전 관련 기능을 차량 가까이에서 실행한다. 고성능 컴퓨팅(High-Performance Computing)은 다중 카메라 처리, 3차원 인지(3D Perception), 신경망 모델(Neural Model), 월드 모델 추론(World-Model Inference), 고급 지형 이해 및 미래의 정책 학습(Policy Learning) 기능을 지원할 수 있다. 따라서 컴퓨팅 확장 구조는 다양한 차량 등급과 임무에 적절한 성능 수준을 적용할 수 있도록 모듈화되어야 한다.

임무 수준 오케스트레이션(Mission-Level Orchestration)은 자율주행을 단순한 내비게이션 이상으로 확장한다. 유용한 야외 로봇은 작업 순서, 목적지, 순찰 또는 검사 목표, 충전 요구사항, 적재 상태, 도킹 동작, 임무 우선순위 및 복구 절차를 이해해야 한다. RMS는 임무 할당, 로봇 상태, 운용 상태, 지도, 충전, 진단 및 차량군 정보를 조정할 수 있으며, 외부 연결성이 저하되거나 일시적으로 사용할 수 없는 경우에도 안전에 중요한 물리적 동작은 로컬 시스템에서 실행할 수 있어야 한다.

배치되는 로봇의 수가 증가할수록 차량군 지능(Fleet Intelligence)은 더욱 중요해진다. 여러 AMR은 차단된 경로, 환경 변화, 어려운 지형, 위치추정 품질, 충전 자원 및 임무 수요에 관한 운용 지식을 공유할 수 있다. 차량군 수준 조정(Fleet-Level Coordination)은 혼잡을 줄이고, 작업을 분배하며, 적절한 로봇을 선택하고, 활용률을 최적화할 수 있다. 시간이 지나면서 다수 로봇에서 수집된 운용 데이터는 엣지 케이스(Edge Case)를 발견하고 배치된 전체 차량군의 자율주행 성능을 개선하기 위한 기반으로 활용될 수 있다.

데이터 기반 폐루프(Data-Driven Closed Loop)는 야외 배포를 엔지니어링 개발과 직접 연결해야 한다. 운용 로그(Operational Log)를 통해 디스인게이지먼트(Disengagement), 인지 불확실성, 위치추정 성능 저하, 계획 실패, 제어 오차, 특이 지형, 안전 개입 및 ODD 경계 사건을 식별할 수 있다. 중요한 사례는 시뮬레이션에서 재구성하고, 회귀시험 시나리오(Regression Scenario)로 변환하며, 필요한 경우 데이터셋에 추가하여 모델이나 알고리즘을 개선할 수 있다. 검증된 업데이트는 이후 다시 실제 현장으로 배포된다.

시뮬레이션(Simulation)과 디지털 트윈(Digital Twin)은 이러한 지속적인 개선 과정을 위한 확장 가능한 환경을 제공한다. 지형, 기상, 조명, 센서 노이즈, GNSS 성능 저하, 보행자, 차량, 장애물, 적재량, 마찰 및 로봇 동역학을 실제 환경에서 비용이 높거나 위험한 시험을 수행하기 전에 체계적으로 변화시킬 수 있다. 소프트웨어 인 더 루프(Software-in-the-Loop, SIL)와 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL) 시험은 자율주행 소프트웨어를 점차 현실적인 차량 동작과 연결하여 현장에서 발견된 문제를 일회성 운용 경험이 아닌 반복 가능한 공학 시험으로 전환할 수 있다.

장기적인 자율주행 비전은 인지와 내비게이션에서 더욱 풍부한 월드 모델(World Model)과 적응형 피지컬 AI(Adaptive Physical AI)로 발전할 수 있다. 월드 모델은 객체, 지형, 에이전트(Agent), 공간 관계, 시간에 따른 변화, 로봇 상태 및 가능한 미래 결과를 표현할 수 있다. 학습된 정책(Learned Policy)은 복잡한 상호작용이나 변화하는 환경에서 행동 능력을 향상시켜 기존 계획 방식을 보완할 수 있으며, 결정론적 제약(Deterministic Constraint)과 독립적인 안전 메커니즘은 계속해서 안전에 중요한 경계를 보호해야 한다.

이러한 발전은 점진적이고 증거 기반(Evidence-Driven)으로 이루어져야 한다. 고급 AI 기능은 운용상의 이점을 측정할 수 있고 그 한계를 이해할 수 있을 때 의미가 있다. 따라서 새로운 인지 모델, 월드 모델, 학습된 정책 또는 추론 기능(Reasoning Function)은 정의된 ODD, 측정 가능한 핵심성과지표(Key Performance Indicator, KPI), 시나리오 기반 검증(Scenario-Based Validation), 회귀시험, 런타임 모니터링(Runtime Monitoring), 통제된 배포(Controlled Deployment)를 통해 도입되어야 한다. 아키텍처는 시스템 동작을 감독하거나 검증할 수 없게 만들지 않으면서 지능 수준을 높일 수 있어야 한다.

최종적으로 힐스로보틱스의 야외 AMR 비전은 다중모달 센싱, 강건한 위치추정, 월드 표현, 행동 계획, 궤적생성, 차량 제어, 안전 감독, 엣지 컴퓨팅, 임무 오케스트레이션, RMS 통합, 차량군 지능, 시뮬레이션 및 데이터 기반 개선을 연결하는 계층형 피지컬 AI 시스템(Layered Physical AI System)이다. 이러한 아키텍처는 개별 야외 로봇에서 확장 가능한 자율 차량군(Scalable Autonomous Fleet)으로 발전하기 위한 경로를 제공하며, 물류, 산업, 인프라, 순찰, 검사 및 기타 복잡한 야외 환경에서 운용할 수 있는 기반을 형성한다.
