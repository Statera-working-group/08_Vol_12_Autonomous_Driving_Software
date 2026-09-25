**Volume 12. Autonomous Driving Software**

# Chapter 06. Behavior Planning

## 06.01. Behavior Planning Architecture Rule ML Hybrid

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

행동 계획(Behavior Planning)은 인지(Perception), 예측(Prediction), 위치추정(Localization), 지도 문맥(Map Context), 임무 목표(Mission Objectives), 안전 제약조건(Safety Constraints)을 이산적인 주행 또는 내비게이션 행동으로 변환하는 의사결정 계층(Decision-Making Layer)이다. 자율주행 소프트웨어 스택(Autonomous Driving Software Stack)에서 행동 계획은 장면 이해(Scene Understanding)와 궤적 생성(Trajectory Generation) 사이에 위치하며, 연속적인 이동 궤적이 계산되기 전에 차량이나 실외 자율이동로봇(Outdoor AMR)이 무엇을 해야 하는지를 결정한다.

행동 계획기(Behavior Planner)는 일반적으로 조향각(Steering Angle), 휠 토크(Wheel Torque), 가속도(Acceleration)를 직접 명령하지 않는다. 대신 경로 추종(Follow Route), 차로 유지(Keep Lane), 양보(Yield), 정지(Stop), 추월(Overtake), 장애물 회피(Avoid Obstacle), 교차로 진입(Enter Intersection), 도킹 영역 접근(Approach Docking Area), 최소위험 기동(Minimal-Risk Maneuver)과 같은 의미론적 의사결정(Semantic Decision)을 생성한다. 이러한 결정은 하위 궤적 생성기(Trajectory Generator)가 행동 의도를 동역학적으로 실행 가능한 경로와 속도 프로파일로 변환할 수 있도록 제약조건을 제공한다.

행동 계획기(Behavior Planner)는 현재 세계 상태(Current World State)를 구조화한 표현을 입력으로 받는다. 일반적인 입력에는 자차 위치 및 속도(Ego Pose and Velocity), 주행 가능 영역 경계(Drivable-Area Boundaries), 정적·동적 장애물(Static and Dynamic Obstacles), 추적 객체 상태(Tracked-Object States), 예측 궤적(Predicted Trajectories), 경로 정보(Route Information), 교통 또는 운용 규칙(Traffic or Operational Rules), 지형 상태(Terrain Conditions), 임무 상태(Mission State)가 포함된다. 실외 자율이동로봇(Outdoor AMR)에서는 주행 가능성(Traversability), 제한구역(Restricted Zones), 통신 상태(Communication Status), 적재물 상태(Payload Condition), 위치추정 신뢰도(Localization Confidence)도 추가될 수 있다.

규칙 기반 아키텍처(Rule-Based Architecture)는 유한상태기계(Finite-State Machine), 행동 트리(Behavior Tree), 의사결정 트리(Decision Tree), 상태 차트(Statechart), 조건 규칙(Conditional Rules)의 집합을 이용하여 주행 지식을 명시적으로 표현한다. 예를 들어 보행자가 충돌 가능 영역(Conflict Region)에 진입하면 주행(CRUISE) 상태에서 양보(YIELD) 상태로 전환하고, 안전 여유 공간이 부족해지면 정지(STOP) 상태로 이동하며, 해당 영역이 다시 안전해지면 주행(CRUISE) 상태로 복귀하도록 구성할 수 있다. 이러한 논리는 결정론적이고 추적 가능한 행동을 제공한다.

규칙 기반 계획(Rule-Based Planning)은 엔지니어가 의사결정을 명시적인 조건까지 추적하고 허용된 상태 전이(State Transition)를 검증할 수 있기 때문에 안전 필수 행동(Safety-Critical Behavior)에 특히 유용하다. 운용 제약조건(Operational Constraints), 속도 제한(Speed Restrictions), 비상 정지 논리(Emergency-Stop Logic), 지오펜싱 영역(Geofenced Areas), 통행 우선권 정책(Right-of-Way Policies), 임무 절차(Mission Procedures)를 직접 코드화할 수 있다. 또한 기록된 입력을 재생하여 특정 행동 전이가 발생한 이유를 분석할 수 있으므로 디버깅(Debugging)에도 유리하다.

규칙 기반 방식의 한계는 확장성(Scalability)이다. 실제 환경에서는 불확실한 인지(Uncertain Perception), 모호한 의도(Ambiguous Intent), 가림(Occlusion), 비정상적인 장애물 움직임(Unusual Obstacle Motion), 변화하는 지형(Changing Terrain), 사람이나 다른 차량과의 상호작용이 복합적으로 발생한다. 시나리오 다양성이 증가하면 규칙 집합에는 예외 조건과 중첩 조건이 계속 추가될 수 있다. 수백 또는 수천 개의 규칙 사이에서 우선순위를 유지하기 어려워지며, 개발 과정에서 고려되지 않은 조합은 규칙 체계의 공백을 노출할 수 있다.

머신러닝 기반 행동 계획(Machine-Learning-Based Behavior Planning)은 데이터 또는 환경과의 상호작용을 통해 의사결정 정책(Decision Policy)을 학습함으로써 이러한 복잡성의 일부를 해결한다. 모방학습(Imitation Learning)은 인간 운전자나 전문가 계획기(Expert Planner)가 시연한 행동을 재현할 수 있으며, 강화학습(Reinforcement Learning)은 안전성(Safety), 진행도(Progress), 효율성(Efficiency), 승차감 또는 이동 안정성(Comfort), 임무 완료(Mission Completion)와 관련된 보상(Reward)을 이용하여 정책을 최적화할 수 있다. 따라서 신경망 정책(Neural Policy)은 명시적인 규칙으로 기술하기 어려운 복잡한 의사결정 경계를 표현할 수 있다.

학습 기반 계획기(Learning-Based Planner)는 더욱 풍부한 장면 표현(Scene Representation)을 사용할 수도 있다. 단순한 기호 상태(Symbolic State)에만 의존하지 않고 객체 특징(Object Features), 점유 표현(Occupancy Representation), 지도 요소(Map Elements), 예측 움직임(Predicted Motion), 잠재 장면 임베딩(Latent Scene Embedding)을 입력으로 사용할 수 있다. 어텐션 기반 네트워크(Attention-Based Network)와 기타 시퀀스 모델(Sequence Model)은 여러 상호작용 객체와 시간적 문맥(Temporal Context)을 함께 추론할 수 있으므로, 단일 순간의 관측뿐 아니라 장면이 시간에 따라 어떻게 변화했는지를 기반으로 행동을 결정할 수 있다.

그러나 학습된 행동(Learned Behavior)은 시스템 수준에서 관리해야 하는 불확실성(Uncertainty)을 도입한다. 신경망 정책(Neural Policy)은 입력이 학습 분포(Training Distribution)에서 크게 벗어나면 예상하지 못한 행동을 생성할 수 있으며, 특정 출력이 생성된 이유를 설명하는 것도 명시적인 규칙을 분석하는 것보다 어려울 수 있다. 따라서 데이터셋 편향(Dataset Bias), 희귀 시나리오(Rare Scenario), 보상 설계(Reward Design), 모델 보정(Model Calibration), 분포 변화(Distribution Shift), 불충분한 커버리지(Insufficient Coverage)는 단순한 머신러닝 문제가 아니라 시스템 아키텍처 차원의 고려사항이 된다.

하이브리드 아키텍처(Hybrid Architecture)는 학습 기반 의사결정 능력(Learned Decision Capability)과 결정론적 제약조건(Deterministic Constraints)을 결합한다. 머신러닝(Machine Learning)은 상호작용 의도(Interaction Intent)를 추정하거나, 후보 기동(Candidate Maneuver)의 순위를 결정하거나, 장면 위험도(Scene Risk)를 평가하거나, 행동 후보를 제안할 수 있다. 동시에 규칙 기반 감독 계층(Rule-Based Supervisory Layer)은 제안된 행동이 운용 및 안전 제약조건을 만족하는지 검사한다. 이를 통해 유연한 추론(Flexible Reasoning)과 명시적이고 시험 가능하며 학습 정책이 쉽게 무시해서는 안 되는 제약조건을 분리할 수 있다.

대표적인 하이브리드 패턴(Hybrid Pattern)은 후보 생성(Candidate Generation) 이후 제약 기반 선택(Constrained Selection)을 수행하는 방식이다. 시스템은 계속 진행(Continue), 감속(Slow), 정지(Stop), 양보(Yield), 추월 또는 통과(Pass), 재경로 설정(Reroute)과 같은 대안을 생성한다. 학습 모델(Learned Model)은 비용(Cost), 확률(Probability), 기대 효용(Expected Utility)을 추정하고, 결정론적 논리(Deterministic Logic)는 충돌 여유(Collision Margin), 제한구역 규칙(Restricted-Area Rules), 차량 한계(Vehicle Limitations), 임무 정책(Mission Policies), 안전 요구사항(Safety Requirements)을 위반하는 후보를 제거한다. 이후 남은 행동이 궤적 생성(Trajectory Generation) 단계로 전달된다.

또 다른 방식은 머신러닝(Machine Learning)이 전체 의사결정 과정을 담당하도록 하는 대신 계층형 계획기(Hierarchical Planner)의 일부로 배치하는 것이다. 상위 임무 논리(High-Level Mission Logic)는 결정론적으로 유지하면서 특정한 모호한 상호작용만 학습 모듈(Learned Module)이 처리할 수 있다. 예를 들어 실외 자율이동로봇(Outdoor AMR)은 규칙으로 정의된 순찰 임무(Patrol Mission)를 수행하면서 학습 기반 예측(Learned Prediction)을 사용하여 보행자가 자신의 이동 경로를 횡단할 가능성을 추정할 수 있다. 이후 계획기는 예측 결과와 명시적인 안전 제약조건을 함께 이용해 행동을 선택한다.

시간적 일관성(Temporal Consistency)은 어떤 아키텍처를 사용하더라도 필수적이다. 센서 추정값이 임계값 근처에서 변동할 때 행동 계획이 여러 대안 사이에서 빠르게 진동해서는 안 된다. 상태 지속(State Persistence), 히스테리시스(Hysteresis), 신뢰도 임계값(Confidence Threshold), 최소 유지시간(Minimum Dwell Time), 전이 가드(Transition Guard)를 이용하여 의사결정을 안정화할 수 있다. 또한 계획기는 실제 상황 변화와 인지, 예측, 위치추정, 통신 과정에서 발생하는 잡음을 구분할 수 있도록 충분한 과거 문맥(Historical Context)을 유지해야 한다.

따라서 불확실성(Uncertainty)은 모듈 경계에서 제거되는 것이 아니라 행동 결정 과정까지 전달되어야 한다. 위치추정 신뢰도(Localization Confidence)가 낮아지면 허용 속도를 감소시키고, 객체 분류(Object Classification)가 불확실하면 안전 여유를 증가시키며, 여러 움직임 예측(Motion Prediction)이 서로 일치하지 않으면 더욱 보수적인 기동을 선택할 수 있다. 이러한 방식은 불확실성을 실제 운용 행동으로 변환하고, 상위 모듈에서 제공되는 정보의 신뢰도가 감소할 때 계획기가 체계적으로 성능을 저하시킬 수 있도록 한다.

실외 자율이동로봇(Outdoor AMR)의 행동 계획은 운용 환경이 부분적으로만 구조화되어 있을 수 있다는 점에서 일반적인 도로 차량의 행동 계획과 차이가 있다. 로봇은 도로(Road), 보도(Sidewalk), 야드(Yard), 적재 구역(Loading Area), 건설 현장(Construction Zone), 비포장 지형(Unpaved Surface)을 연속적으로 이동할 수 있다. 따라서 행동 계층은 기존 교통 상호작용뿐 아니라 지형 주행 가능성(Terrain Traversability), 지역 운용 정책(Local Operating Policies), 임무 우선순위(Mission Priorities), 협소 통로(Narrow Passages), 임시 장애물(Temporary Obstacles), 작업자(Human Workers), 시설별 제약조건(Infrastructure-Specific Constraints)을 함께 고려해야 한다.

아키텍처는 행동 계획(Behavior Planning)과 궤적 생성(Trajectory Generation) 사이의 명확한 경계를 유지해야 한다. 행동 계획은 어떤 기동을 수행해야 하는지 그리고 어떤 제약조건 아래에서 수행해야 하는지를 결정하며, 궤적 생성은 해당 기동을 기하학적·동역학적으로 어떻게 실행할 것인지를 결정한다. 이러한 분리는 동일한 행동 목표(Behavioral Objective) 아래에서 여러 궤적 후보(Trajectory Candidates)를 평가할 수 있게 하며, 행동 논리가 특정 저수준 제어기(Low-Level Controller)에 종속되지 않도록 한다.

안전 감독(Safety Supervision)은 행동이 선택된 이후에도 계속 활성화되어야 한다. 런타임 모니터(Runtime Monitor)는 예측 충돌 위험(Predicted Collision Risk), 정지 거리(Stopping Distance), 위치추정 유효성(Localization Validity), 인지 시스템 상태(Perception Health), 액추에이터 가용성(Actuator Availability), 운용 경계(Operational Boundaries)를 검사할 수 있다. 선택된 행동이 더 이상 안전하지 않거나 필요한 정보를 사용할 수 없게 되면 시스템은 기존 기동을 그대로 완료하는 대신 해당 행동을 무효화하고 대체 행동(Fallback Behavior), 비상 정지(Emergency Stop), 최소위험상태(Minimal-Risk Condition)로 전환할 수 있다.

행동 계획은 예측(Prediction) 모듈과도 명확한 인터페이스를 가져야 한다. 다른 객체가 앞으로 어떻게 움직일지를 고려하지 않는 계획기는 반응적(Reactive)으로 동작할 수밖에 없으며 불필요하게 급격한 결정을 내릴 수 있다. 예측 인지형 계획(Prediction-Aware Planning)은 특정 기동을 확정하기 전에 가능한 미래 상호작용을 평가한다. 반대로 로봇의 움직임에 따라 다른 차량이나 보행자의 행동이 달라질 수 있으므로 계획 자체가 예측에 영향을 줄 수도 있으며, 이러한 상호 의존적인 의사결정 문제는 확률론적 접근(Probabilistic Approach)과 게임이론적 접근(Game-Theoretic Approach)의 필요성을 높인다.

소프트웨어 구현(Software Implementation)은 검증과 현장 디버깅을 위해 계획기의 내부 의사결정 과정(Internal Reasoning)을 외부에서 확인할 수 있도록 구성해야 한다. 유용한 출력에는 현재 행동 상태(Active Behavioral State), 후보 행동(Candidate Behaviors), 거부된 대안(Rejected Alternatives), 상태 전이 원인(Transition Causes), 제약조건 위반(Constraint Violations), 신뢰도 값(Confidence Values), 위험도 추정(Risk Estimates), 타임스탬프(Timestamps)가 포함된다. 이러한 정보를 기록하면 예상하지 못한 사건이 발생했을 때 자율이동로봇이 무엇을 수행했는지뿐 아니라 왜 특정 행동을 선택했는지도 재구성할 수 있다.

검증(Validation)은 개별 상태만 시험하는 것이 아니라 상태 전이(Transition)와 상호작용(Interaction)을 포괄해야 한다. 시나리오 기반 시뮬레이션(Scenario-Based Simulation)을 통해 보행자 움직임, 차량 행동, 장애물 배치, 가시성(Visibility), 위치추정 오차(Localization Error), 지형 상태, 통신 지연(Communication Delay), 센서 불확실성(Sensor Uncertainty)을 변화시킬 수 있다. 특히 여러 조건이 동시에 참이 되거나 추정값이 의사결정 임계값을 반복적으로 넘나드는 상황에서 문제가 발생하기 쉬우므로 행동 전이 경계조건(Boundary Conditions)을 집중적으로 검증해야 한다.

전체 소프트웨어 구조에서 행동 계획(Behavior Planning)은 객체 검출 및 추적(Object Detection and Tracking) 이후, 궤적 생성(Trajectory Generation) 이전에 위치한다. 이후 제어 통합(Control Integration), 안전 및 이중화(Safety and Redundancy), 시스템 수준 안전 검증(System-Level Safety Validation)으로 연결된다. 따라서 행동 계획은 환경 이해(Environmental Understanding)를 실제 실행 가능한 움직임(Executable Motion)과 연결하는 핵심 의미론적 의사결정 계층(Semantic Decision Layer)이며, 동시에 독립적인 안전 메커니즘(Safety Mechanisms)과의 인터페이스를 유지한다.

결국 규칙 기반(Rule-Based), 머신러닝 기반(Machine-Learning-Based), 하이브리드(Hybrid) 행동 계획은 서로를 대체하는 기술 세대라기보다 상호 보완적인 아키텍처 선택지로 이해해야 한다. 규칙은 명시적인 제약조건과 예측 가능한 상태 전이를 제공하고, 학습은 복잡한 상호작용에 대한 적응 능력을 제공하며, 하이브리드 설계는 두 가지 장점을 결합한다. 실제 실외 자율이동로봇(Outdoor AMR)에서는 안전 필수 의사결정 주변에 결정론적 감독(Deterministic Supervision)을 유지하면서 필요한 영역부터 학습 기반 구성요소(Learned Components)를 선택적으로 도입하는 방향으로 아키텍처를 발전시킬 수 있다.

## 06.02. FSM Based Behavior Planning for AMR [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

유한상태기계(Finite-State Machine, FSM)는 자율이동로봇(Autonomous Mobile Robot, AMR)의 행동을 일련의 이산 상태(Discrete State)와 명시적으로 정의된 전이(Transition)로 표현하는 구조화된 방법이다. AMR에서 FSM 기반 행동 계획(FSM-Based Behavior Planning)은 인지(Perception), 위치추정(Localization), 임무 논리(Mission Logic), 이동 실행(Motion Execution) 사이를 연결하는 실용적인 방법을 제공한다. 이 구조는 로봇이 현재 어떤 행동 모드(Behavioral Mode)에 있는지, 어떤 조건에서 상태 전이가 허용되는지, 그리고 전이 이후 어떤 행동을 실행해야 하는지를 정의한다. 첨부된 소프트웨어 트리(Software Tree)에서는 FSM 기반 행동 계획이 Chapter 06의 핵심 구성으로 배치되어 있으며, 일반적인 행동 계획 아키텍처 이후에 위치하고 의사결정 트리(Decision Tree), 규칙 엔진(Rule Engine), 예측 인지형 계획(Prediction-Aware Planning), 그리고 보다 발전된 계획 방법들에 앞서 위치한다.

AMR의 FSM은 IDLE, INITIALIZE, NAVIGATE, APPROACH, AVOID, WAIT, DOCK, CHARGE, COMPLETE, EMERGENCY_STOP과 같은 상태(State)로 로봇의 운용 생명주기(Operational Lifecycle)를 표현할 수 있다. 각각의 상태에는 명확한 목적과 허용된 상태 전이 집합이 정의된다. 예를 들어 유효한 임무(Mission)를 수신하면 IDLE에서 NAVIGATE로 전이하고, 장애물이 즉각적인 충돌 가능성을 발생시키면 NAVIGATE에서 AVOID로 전이하며, 임무에서 도킹(Docking)이 요구되면 NAVIGATE에서 DOCK으로 전이할 수 있다. 이러한 명시적인 구조는 전체 행동을 이해하고 구현하며 시험하고 유지보수하기 쉽게 만든다.

가장 중요한 설계 원칙은 상태 정의(State Definition)와 전이 조건(Transition Condition)을 분리하는 것이다. 상태는 현재 실행되고 있는 행동을 설명하며, 전이는 계획기가 다른 상태를 선택하도록 만드는 사건(Event) 또는 조건(Condition)을 설명한다. 조건은 인지(Perception), 위치추정(Localization), 장애물 추적(Obstacle Tracking), 경로 진행률(Route Progress), 임무 상태(Mission Status), 배터리 상태(Battery State), 통신 가용성(Communication Availability), 안전 모니터링(Safety Monitoring) 등에서 도출될 수 있다. 이러한 개념을 분리하면 개별 행동 사이의 강한 결합(Tight Coupling)을 방지할 수 있으며, 동일한 전이 논리를 다양한 운용 시나리오(Operational Scenario)에서 재사용할 수 있다.

실용적인 FSM은 진입 동작(Entry Action), 지속 동작(Continuous Action), 종료 동작(Exit Action), 전이 가드(Transition Guard), 고장 대응(Failure Response)을 정의해야 한다. NAVIGATE 상태에 진입하면 계획기는 경로 추종(Route Following)을 초기화하고 적절한 궤적 생성 프로세스(Trajectory Generation Process)를 활성화할 수 있다. NAVIGATE에 머무르는 동안에는 장애물, 위치추정 신뢰도(Localization Confidence), 경로 진행률, 임무 조건을 지속적으로 평가한다. 상태를 벗어날 때에는 임시 리소스(Resource)나 명령(Command)을 해제할 수 있다. 전이 가드는 전이가 허용되기 전에 명시적인 조건을 요구함으로써 부적절한 상태 변경을 방지한다.

실외 AMR에서는 인지 이벤트(Perception Event)가 FSM 전이의 주요 트리거가 되는 경우가 많다. 감지된 보행자는 로봇을 YIELD 또는 WAIT 상태로 전환시킬 수 있으며, 정적 장애물(Static Obstacle)은 AVOID 또는 REROUTE 행동을 유발할 수 있다. 장애물이 사라지면 계획기는 이전 내비게이션 상태(Navigation State)로 복귀할 수 있다. 그러나 전이는 단일한 잡음성 검출(Noisy Detection)에 의존해서는 안 된다. 시간적 지속성(Temporal Persistence), 신뢰도 임계값(Confidence Threshold), 객체 추적(Object Tracking), 최소 상태 유지시간(Minimum State Duration)을 사용하면 불안정한 센서 관측으로 인한 빠른 상태 전환을 방지할 수 있다.

위치추정(Localization)은 FSM 행동에 영향을 주는 또 다른 중요한 입력이다. 위치추정 신뢰도가 높으면 로봇은 정상적인 내비게이션 행동(Normal Navigation Behavior)을 수행할 수 있다. 위치추정 성능이 저하되면 계획기는 속도를 낮추거나 복구(Recovery)를 시도하는 보수적인 상태(Conservative State)로 전환할 수 있다. 위치추정을 운용 임계값(Operational Threshold) 이상으로 사용할 수 없게 되면 로봇은 제어된 정지(Controlled Stop) 또는 최소위험 행동(Minimal-Risk Behavior)으로 진입할 수 있다. 이러한 방식은 위치추정 정보를 단순히 독립적인 하위 시스템으로 취급하는 대신 시스템 상태 정보를 명시적인 운용 행동으로 변환할 수 있게 한다.

임무 관리(Mission Management)는 상위 수준 FSM(High-Level FSM)으로 모델링하거나 계층형 상태(Hierarchical State)를 통해 내비게이션 FSM과 통합할 수 있다. 하나의 임무는 충전 스테이션(Charging Station)을 출발하고, 작업 영역(Work Area)으로 이동하고, 검사를 수행하고, 지정된 위치로 돌아온 후, 도킹하는 순차적인 활동으로 구성될 수 있다. 상위 임무 상태(High-Level Mission State)는 현재 목적을 결정하고, 하위 내비게이션 FSM은 지역적인 이동을 처리한다. 이러한 계층적 구성은 임무, 내비게이션, 장애물, 안전 상태의 모든 조합을 하나의 거대한 FSM에 포함시키는 문제를 방지한다.

상태 폭발(State Explosion)은 전통적인 FSM 설계의 주요 한계 중 하나이다. 장애물 유형, 임무 상태, 지형 조건, 위치추정 상태, 배터리 상태, 통신 상태의 모든 조합을 독립적인 상태로 만들면 상태의 수가 빠르게 증가할 수 있다. 그 결과 아키텍처를 검토하고 유지보수하기가 어려워진다. 보다 나은 방법은 상태를 의미적으로 명확하게 유지하고, 모든 조합을 별도의 상태로 인코딩하는 대신 변화하는 정보를 문맥 데이터(Contextual Data) 또는 별도의 감독 조건(Supervisory Condition)으로 표현하는 것이다.

여러 전이 조건이 동시에 발생할 때에는 우선순위 처리(Priority Handling)가 특히 중요하다. AMR이 보행자를 만나는 동시에 임무 웨이포인트(Mission Waypoint)에 접근하고 위치추정 신뢰도까지 낮아질 수 있다. 따라서 계획기는 여러 이벤트가 동시에 유효한 경우에도 결정론적인 우선순위 정책(Deterministic Priority Policy)을 가져야 한다. 안전 필수 전이(Safety-Critical Transition)는 일반적인 임무 전이보다 먼저 평가되어야 하며, 복구 조건(Recovery Condition)이 이미 활성화된 비상 대응(Emergency Response)을 무시하도록 허용해서는 안 된다. 명시적인 우선순위 규칙은 여러 이벤트가 동시에 유효할 때에도 행동을 결정론적으로 만든다.

FSM 행동은 궤적 생성(Trajectory Generation) 및 제어(Control)와도 명확하게 연동되어야 한다. FSM은 원하는 행동(Desired Behavior)을 결정하고, 하위 궤적 계획기(Trajectory Planner)는 실행 가능한 경로와 속도 프로파일(Speed Profile)을 생성한다. 예를 들어 AVOID 상태는 로봇이 장애물 주변을 통과하면서 안전 여유(Safety Margin)를 유지해야 한다고 지정할 수 있지만, 개별 휠 명령(Wheel Command)을 직접 계산할 필요는 없다. 이러한 분리를 통해 동일한 행동 논리를 서로 다른 궤적 생성 및 제어 구현과 함께 사용할 수 있으며 안정적인 소프트웨어 인터페이스를 유지할 수 있다.

안전 행동(Safety Behavior)은 일반적인 내비게이션 상태 중 하나로만 취급하기보다 독립적인 감독 메커니즘(Supervisory Mechanism)으로 표현하는 것이 바람직하다. 임박한 충돌(Imminent Collision), 핵심 액추에이터 고장(Critical Actuator Failure), 심각한 위치추정 손실(Severe Localization Loss), 필수 안전 정보의 손실(Loss of Required Safety Information)과 같은 비상 조건은 EMERGENCY_STOP 또는 다른 최소위험 상태(Minimal-Risk State)로 강제 전환될 수 있다. 안전 메커니즘은 일반적인 내비게이션 결정을 재정의(Override)할 수 있어야 한다. 위험이 제거된 이후에도 이전 상태로 자동 복귀하기보다는 명시적인 검증(Explicit Validation)을 거쳐 복구해야 한다.

FSM 구현은 이벤트 로깅(Event Logging)과 결정론적 재현(Deterministic Replay)을 통해 많은 이점을 얻을 수 있다. 모든 상태 전이는 이전 상태(Previous State), 새로운 상태(New State), 전이를 발생시킨 이벤트(Triggering Event), 관련 가드 조건(Guard Condition), 신뢰도 정보(Confidence Information), 타임스탬프(Timestamp)를 기록해야 한다. 이러한 기록을 통해 엔지니어는 현장 운용 중 AMR이 왜 행동을 변경했는지를 재구성할 수 있다. 로봇이 예상하지 못하게 정지하거나, 대기하거나, 경로를 변경하거나, 내비게이션을 재개하지 못하는 경우에도 전이 이력(Transition History)은 원인을 진단하고 검증하기 위한 직접적인 출발점을 제공한다.

FSM을 시험하기 위해서는 각각의 상태가 개별적으로 정상 작동하는지만 확인해서는 충분하지 않다. 모든 중요한 전이는 정상(Nominal), 경계(Boundary), 성능 저하(Degraded), 충돌(Conflicting) 조건에서 시험해야 한다. 시뮬레이션을 통해 변화하는 장애물 위치, 센서 고장, 위치추정 오류, 통신 지연, 배터리 제약, 임무 중단 등을 주입할 수 있다. 또한 반복적인 상태 전이, 고장 이후의 복구, 동시 이벤트, 의사결정 임계값 주변의 조건도 검증해야 한다. 이는 많은 행동 관련 고장이 특정 상태 내부가 아니라 두 상태 사이의 전이 과정에서 발생하기 때문에 특히 중요하다.

FSM은 결정론적인 행동 프레임워크(Deterministic Behavioral Framework)로 설계하면서도 입력 정보가 점점 더 지능화될 수 있도록 구성해야 한다. 인지(Perception) 및 예측(Prediction) 모델은 객체 유형, 의도(Intent), 위험도(Risk), 미래 움직임(Future Motion)에 대한 학습 기반 추정값(Learned Estimate)을 제공할 수 있지만, FSM은 이러한 추정값을 명시적인 행동 전이로 변환할 수 있다. 이를 통해 머신러닝 구성요소(Machine-Learning Component)는 정보를 제공하고 행동 계층(Behavior Layer)은 운용 논리(Operational Logic)를 적용하는 명확한 분리가 가능하다. 이러한 구조는 해석 가능성(Interpretability)을 향상시키면서도 더 복잡한 환경에 학습 기반 기능을 도입할 수 있는 기회를 유지한다.

실외 AMR에서는 FSM을 지형(Terrain)과 운용 문맥(Operational Context)에 맞게 확장할 수 있다. 도로(Road), 보도(Sidewalk), 야드(Yard), 적재 구역(Loading Area), 건설 구역(Construction Zone), 비포장면(Unpaved Surface)은 서로 다른 속도 제한(Speed Limit), 안전 여유 정책(Clearance Policy), 장애물 대응(Obstacle Response), 임무 규칙(Mission Rule)을 요구할 수 있다. 각각의 환경마다 완전히 다른 계획기를 만드는 대신 FSM은 공통된 행동 구조(Common Behavioral Structure)를 유지하면서 현재 운용 문맥에 따라 매개변수(Parameter)와 전이 가드(Transition Guard)를 조정할 수 있다. 이를 통해 여러 AMR 애플리케이션에서 소프트웨어를 재사용할 수 있으며 불필요한 중복 구현을 줄일 수 있다.

FSM 기반 행동 계획의 실질적인 가치는 명시성(Explicitness)에 있다. 엔지니어는 현재 상태를 확인하고, 허용된 전이를 식별하며, 전이를 발생시킨 조건을 재현하고, 그 이후 어떤 행동이 실행되어야 하는지를 판단할 수 있다. 이러한 특성은 예측 가능한 실행(Predictable Execution), 디버깅(Debugging), 안전성 검토(Safety Review), 구조화된 검증(Structured Validation)이 중요한 AMR의 기초 행동 계층(Foundational Behavioral Layer)에 FSM을 특히 적합하게 만든다. 이후 FSM의 복잡성이 지나치게 증가하는 영역에서는 보다 발전된 방법을 도입할 수 있으며, 그 경우에도 FSM은 임무 실행(Mission Execution), 안전 제약조건(Safety Constraints), 운용 모드 관리(Operational Mode Management)를 위한 감독 프레임워크(Supervisory Framework)로 유용하게 활용될 수 있다.

## 06.03. Decision Tree and Rule Engine for Scenarios [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

의사결정 트리(Decision Tree)와 규칙 엔진(Rule Engine)은 복잡한 환경 및 운용 조건을 명시적인 행동 결정으로 변환하기 위한 구조화된 방법을 제공한다. 자율이동로봇(Autonomous Mobile Robot, AMR)에서는 FSM의 기본적인 상태 전이 개념을 확장하여, 행동을 선택하기 전에 여러 조건을 계층적으로 평가할 수 있도록 한다. 소프트웨어 구조에서 이 주제는 Chapter 06의 FSM 기반 행동 계획 바로 다음에 배치되어 있으며, 보다 표현력이 높은 시나리오 지향 의사결정 메커니즘(Scenario-Oriented Decision Mechanism)으로서의 역할을 나타낸다.

의사결정 트리(Decision Tree)는 의사결정을 조건(Condition)과 분기(Branch)의 계층 구조로 표현한다. 계획기(Planner)는 초기 조건에서 시작하여 연속적인 질문을 평가하고 최종 행동(Terminal Action)에 도달할 때까지 분기를 따라간다. 예를 들어 시스템은 먼저 경로가 차단되었는지를 판단하고, 이후 장애물이 정적(Static)인지 동적(Dynamic)인지 판단하며, 마지막으로 회피를 수행할 수 있는 충분한 여유 공간(Clearance)이 있는지를 평가할 수 있다. 결과적인 분기는 CONTINUE, SLOW, WAIT, AVOID 또는 REROUTE와 같은 행동을 선택할 수 있다.

의사결정 트리의 주요 장점은 시나리오 논리(Scenario Logic)가 시각적이고 논리적으로 구조화된다는 점이다. 각각의 분기는 조건을 나타내고 각각의 리프(Leaf)는 운용 결정을 나타낸다. 이를 통해 엔지니어는 인지(Perception), 위치추정(Localization), 임무(Mission), 로봇 상태(Vehicle Status) 정보의 조합이 어떻게 특정 행동으로 이어지는지를 이해할 수 있다. 서로 관련 없는 다수의 조건문(Conditional Statement)으로 구성된 구조와 비교하면, 잘 설계된 트리는 의사결정 의존관계(Decision Dependency)의 계층을 보다 명확하게 제공한다.

규칙 엔진(Rule Engine)은 이러한 개념을 확장하여 의사결정 지식을 명시적인 규칙(Explicit Rule)의 집합으로 표현한다. 일반적인 규칙은 조건(Condition)과 이에 연결된 행동(Action) 또는 결론(Conclusion)으로 구성된다. 현재 로봇 상태가 조건을 만족하면 해당 규칙이 적용 가능해진다. 여러 규칙이 동일한 상황에 대해 동시에 평가될 수 있으므로, 엔진은 규칙 우선순위(Rule Priority), 충돌 해결(Conflict Resolution), 활성화(Activation), 비활성화(Deactivation), 실행 순서(Execution Order)를 관리하는 메커니즘을 필요로 한다.

시나리오 기반 계획(Scenario-Based Planning)은 재사용 가능한 규칙과 핵심 행동 소프트웨어(Core Behavior Software)를 분리하는 것에서 이점을 얻는다. 예를 들어 보행자 주변에서 속도를 감소시키는 규칙은 여러 임무에서 재사용할 수 있으며, 특정 창고 구역이나 건설 구역에 연결된 규칙은 해당 문맥에 한정할 수 있다. 이러한 분리를 통해 전체 내비게이션 아키텍처를 근본적으로 변경하지 않고도 운용 정책(Operational Policy)을 변경할 수 있다.

규칙 엔진의 입력 조건은 다양한 하위 시스템에서 생성될 수 있다. 인지 시스템은 객체 종류(Object Class), 위치(Position), 속도(Velocity), 신뢰도 값(Confidence Value)을 제공할 수 있다. 위치추정 시스템은 자세(Pose)와 위치추정 품질(Localization Quality)을 제공할 수 있다. 매핑 시스템은 도로 경계(Road Boundary), 제한 영역(Restricted Area), 의미론적 구역(Semantic Zone)을 제공할 수 있다. 임무 관리(Mission Management)는 작업 목표와 우선순위를 제공하며, 로봇 상태(Vehicle Health)는 배터리, 액추에이터, 통신 및 시스템 상태 정보를 제공할 수 있다.

유용한 아키텍처는 원시 센서 정보(Raw Sensor Information)와 의사결정 수준 사실(Decision-Level Fact)을 구분한다. 모든 규칙이 카메라 또는 LiDAR 측정값을 직접 해석하도록 하는 대신, 인지 모듈(Perception Module)이 먼저 pedestrian_detected, path_blocked, obstacle_distance, localization_valid, docking_available과 같은 구조화된 사실(Structured Fact)을 생성할 수 있다. 규칙 엔진은 원시 센서 데이터가 아니라 이러한 사실을 평가한다. 이러한 분리는 결합도를 낮추며, 서로 다른 인지 구현을 사용하더라도 동일한 의사결정 논리를 사용할 수 있게 한다.

규칙은 불확실성(Uncertainty)도 고려해야 한다. obstacle_detected와 같은 조건은 일반적으로 단일 관측에 의해 작동하기보다는 검출 신뢰도(Detection Confidence), 시간적 지속성(Temporal Persistence), 추적 일관성(Tracking Consistency)을 포함해야 한다. 마찬가지로 위치추정 관련 규칙은 정상(Valid), 성능 저하(Degraded), 사용 불가(Unavailable) 상태를 구분할 수 있다. 불확실한 측정값을 제어된 의사결정 수준의 조건으로 변환하면 상위 정보의 잡음으로 인해 불안정한 행동이 발생하는 것을 방지하는 데 도움이 된다.

충분히 복잡한 시스템에서는 규칙 충돌(Rule Conflict)이 불가피하다. 하나의 규칙은 경로가 비어 있기 때문에 정상 주행(Normal Navigation)을 요구할 수 있지만, 다른 규칙은 보행자가 근처에 있기 때문에 감속(Reduced Speed)을 요구할 수 있다. 또 다른 규칙은 안전 모니터(Safety Monitor)가 임박한 위험을 감지했기 때문에 비상 정지(Emergency Stop)를 요구할 수 있다. 따라서 규칙 엔진은 결정론적인 충돌 해결 정책(Deterministic Conflict-Resolution Policy)을 필요로 하며, 안전 관련 제약조건(Safety-Related Constraint)이 일반적인 임무 또는 내비게이션 규칙을 재정의할 수 있어야 한다.

우선순위(Priority)는 명시적인 규칙 수준(Rule Level), 우선순위 값(Precedence Value), 상호 배타적 조건(Mutually Exclusive Condition), 계층적 평가(Hierarchical Evaluation)를 통해 구현할 수 있다. 실용적인 순서는 비상 및 안전 제약조건을 복구 행동(Recovery Behavior), 임무 요구사항(Mission Requirement), 효율성 목표(Efficiency Objective)보다 높은 수준에 배치할 수 있다. 정확한 정책은 AMR의 운용 설계에 따라 달라지지만, 중요한 아키텍처 원칙은 동시에 적용 가능한 규칙이 모호하거나 비결정론적인 행동을 만들어서는 안 된다는 것이다.

의사결정 트리와 규칙 엔진은 서로 결합할 수도 있다. 의사결정 트리는 상위 수준의 시나리오 구조를 제공하고, 각각의 노드(Node)가 보다 상세한 평가를 위해 규칙 집합을 호출하도록 구성할 수 있다. 반대로 규칙 엔진이 현재 운용 문맥에 따라 어떤 의사결정 트리를 활성화할지를 선택할 수도 있다. 이러한 조합을 통해 AMR이 여러 환경과 임무 유형에서 동작하더라도 아키텍처를 체계적으로 유지할 수 있다.

실외 AMR에서는 시나리오 문맥(Scenario Context)에 따라 적절한 의사결정이 크게 달라질 수 있다. 구조화된 도로에서 경로가 차단되면 제어된 대기(Controlled Wait) 또는 재경로 설정(Reroute)이 선택될 수 있지만, 건설 구역에서는 동일한 장애물에 대해 다른 회피 전략(Avoidance Strategy)이 필요할 수 있다. 따라서 지형(Terrain), 노면 상태(Road Surface), 보행자 밀도(Pedestrian Density), 제한구역(Restricted Area), 적재 상태(Payload Condition), 통신 가용성(Communication Availability)이 의사결정 변수가 될 수 있다. 문맥 인지 규칙(Context-Aware Rule)을 사용하면 각 환경마다 완전히 독립적인 계획기를 만들지 않고도 공통 행동 논리를 환경에 맞게 조정할 수 있다.

가능한 경우 임무 논리(Mission Logic)는 지역적인 장애물 대응(Local Obstacle Response)과 분리해야 한다. 상위 수준 규칙은 로봇이 검사 순서(Inspection Sequence)를 완료해야 한다고 지정하고, 하위 수준 규칙은 해당 임무 수행 중 일시적인 장애물에 어떻게 대응할지를 결정할 수 있다. 이러한 분리는 일시적인 지역 이벤트가 전체 임무 목표(Overall Mission Objective)를 불필요하게 변경하는 것을 방지한다. 계층적 의사결정 구조(Hierarchical Decision Structure)는 여러 순차 작업을 포함하고 각각의 작업이 고유한 지역 운용 규칙을 갖는 경우 특히 유용하다.

규칙 엔진은 명확한 진입 조건(Entry Condition), 실행 조건(Execution Condition), 종료 조건(Exit Condition), 무효화 조건(Invalidation Condition)을 유지해야 한다. 활성화된 규칙이 반드시 무기한 유지되어야 하는 것은 아니다. 예를 들어 회피 규칙(Avoidance Rule)은 장애물이 정의된 충돌 영역(Conflict Region)에 진입하면 활성화되고, 경로가 일정 시간 동안 계속 비어 있는 경우에만 비활성화될 수 있다. 이러한 시간적 요구사항(Temporal Requirement)은 행동 진동(Oscillation)을 줄이고 서로 경쟁하는 행동 사이에서 빠른 전환이 발생하는 것을 방지한다.

의사결정 논리는 설명 가능한 출력(Explainable Output)을 생성해야 한다. 시스템은 AVOID와 같은 행동만 반환하는 것이 아니라 어떤 시나리오가 인식되었는지, 어떤 조건이 충족되었는지, 어떤 규칙이 활성화되었는지, 그리고 경쟁 규칙이 왜 거부되었는지를 식별할 수 있어야 한다. 이러한 정보는 디버깅(Debugging), 현장 분석(Field Analysis), 안전성 검증(Safety Validation), 운용 개선(Operational Improvement)에 유용하다. 또한 환경 관측(Environmental Observation)과 결과적인 로봇 행동 사이에 추적 가능한 연결을 제공한다.

규칙 엔진은 궤적 생성(Trajectory Generation)이나 저수준 제어(Low-Level Control)를 직접 대체해서는 안 된다. 주요 책임은 의도된 행동 반응(Intended Behavioral Response)을 결정하는 것이다. AVOID, FOLLOW, WAIT, DOCK 또는 REROUTE와 같은 결정이 선택되면 궤적 생성 계층(Trajectory-Generation Layer)이 실행 가능한 경로와 이동 프로파일(Motion Profile)을 결정한다. 이후 제어 계층(Control Layer)이 해당 궤적을 조향(Steering), 가속(Acceleration), 제동(Braking), 구동(Drive) 명령으로 변환한다. 이러한 분리는 모듈성을 유지하며 각 계층이 독립적으로 발전할 수 있도록 한다.

안전 감독(Safety Supervision)은 의사결정 엔진(Decision Engine)을 재정의할 수 있는 능력을 유지해야 한다. 선택된 규칙이 환경 변화로 인해 안전하지 않게 되면 상위 우선순위의 안전 메커니즘(Safety Mechanism)이 해당 행동을 무효화해야 한다. 충돌 위험(Collision Risk), 위치추정 손실(Loss of Localization), 액추에이터 고장(Actuator Fault), 핵심 센서 고장(Critical Sensor Failure) 또는 기타 안전 조건은 비상 정지(Emergency Stop) 또는 최소위험 행동(Minimal-Risk Behavior)으로 전환하도록 만들 수 있다. 따라서 규칙 엔진은 로봇 움직임에 대한 최종적인 권한자가 아니라 더 큰 안전 아키텍처(Safety Architecture) 안에서 동작해야 한다.

의사결정 트리와 규칙 엔진의 검증(Validation)에는 체계적인 시나리오 커버리지(Scenario Coverage)가 필요하다. 각각의 분기는 정상값(Normal Value)과 경계값(Boundary Value)으로 실행해야 하며, 동시에 활성화되는 규칙들의 조합도 충돌 여부를 확인해야 한다. 시뮬레이션을 통해 장애물 거리(Obstacle Distance), 객체 속도(Object Velocity), 위치추정 신뢰도(Localization Confidence), 통신 상태(Communication State), 임무 상태(Mission Status), 환경 문맥(Environmental Context)을 변화시킬 수 있다. 기록된 현장 시나리오는 이후 재생(Replay)하여 동일한 입력이 예상된 의사결정을 생성하는지 검증할 수 있다.

아키텍처는 규칙의 버전 관리(Versioning)와 통제된 수정(Controlled Modification)을 지원해야 한다. 운용 규칙은 핵심 내비게이션 소프트웨어보다 더 자주 변경될 수 있지만, 통제되지 않은 수정은 예상하지 못한 상호작용을 발생시킬 수 있다. 따라서 규칙에는 식별 가능한 버전(Version), 담당자(Owner), 활성화 조건(Activation Condition), 검증 상태(Validation Status), 배포 기록(Deployment Record)을 부여해야 한다. 변경사항은 실제 운용 AMR에 적용하기 전에 회귀 시나리오(Regression Scenario)를 대상으로 시험해야 한다.

의사결정 트리와 규칙 엔진은 단순한 FSM 행동과 보다 적응적인 계획 접근법(Adaptive Planning Approach) 사이에서 실용적인 중간 계층(Middle Layer)을 제공한다. 명시적인 의사결정 논리를 유지하면서도 훨씬 풍부한 조건 조합과 운용 시나리오를 처리할 수 있다. 그 효과는 규칙의 수 자체보다는 정보 모델(Information Model)의 품질, 계층 구조(Hierarchy), 우선순위 정책(Priority Policy), 시간 처리(Temporal Handling), 충돌 해결(Conflict Resolution), 검증 프로세스(Validation Process)의 품질에 더 크게 좌우된다.

AMR 아키텍처에서 의사결정 트리와 규칙 엔진의 가장 유용한 역할은 시나리오 지식을 결정론적이고 검증 가능한 의사결정 과정으로 구성하는 것이다. 인지(Perception), 위치추정(Localization), 매핑(Mapping), 임무(Mission), 로봇 상태(Vehicle Health) 시스템이 구조화된 사실을 제공하고, 의사결정 계층(Decision Layer)이 명시적인 정책(Explicit Policy)에 따라 이러한 사실을 평가하며, 그 결과 생성된 행동이 궤적 생성(Trajectory Generation)과 제어(Control)로 전달된다. 이러한 아키텍처는 이후 예측(Prediction), 머신러닝(Machine Learning), 또는 보다 발전된 의사결정 방법을 통합하면서도 추적 가능성(Traceability)과 운용 제어(Operational Control)를 유지할 수 있는 강력한 기반을 제공한다.

## 06.04. MCTS and Game Theoretic Behavior Planning [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

몬테카를로 트리 탐색(Monte Carlo Tree Search, MCTS)은 반복적인 시뮬레이션과 통계적 추정을 통해 후보 행동의 트리를 확장하면서 가능한 미래 행동을 평가하는 탐색 기반 의사결정 방법(Search-Based Decision-Making Method)이다. 자율이동로봇(Autonomous Mobile Robot, AMR)의 행동 계획(Behavior Planning)에서는 결정론적인 상태 및 규칙 기반 접근법을 확장하여 여러 미래 행동 시퀀스(Action Sequence)를 명시적으로 고려할 수 있다. 소프트웨어 구조에서 MCTS 및 게임이론 기반 행동 계획(Game-Theoretic Behavior Planning)은 FSM과 의사결정 트리 접근법 이후에 위치하며, 보다 예측 지향적이고 상호작용을 고려하는 의사결정으로 전환되는 단계를 나타낸다.

MCTS 계획기(MCTS Planner)는 의사결정 과정을 행동 상태(Behavioral State)에 해당하는 노드(Node)와 가능한 행동을 나타내는 가지(Branch)로 구성된 트리로 표현한다. 현재 로봇 상태에서 시작하여 계획기는 유망한 가지를 반복적으로 선택하고, 아직 탐색되지 않은 행동을 확장하며, 가능한 미래 결과를 시뮬레이션하고, 방문한 노드의 통계적 가치를 업데이트한다. 충분한 반복이 수행되면 계획기는 고정된 규칙의 순서에만 의존하지 않고 추정된 미래 가치(Future Value)에 따라 행동을 선택할 수 있다.

MCTS의 네 가지 핵심 연산은 선택(Selection), 확장(Expansion), 시뮬레이션(Simulation), 역전파(Backpropagation)이다. 선택 단계에서는 이미 좋은 결과를 보인 행동을 활용(Exploitation)하는 것과 평가 횟수가 적은 행동을 탐색(Exploration)하는 것 사이의 균형을 유지하는 정책(Policy)을 따른다. 확장 단계에서는 새로운 후보 행동이나 상태를 트리에 추가한다. 시뮬레이션 단계에서는 단순화된 모델 또는 정책을 이용하여 해당 가지의 미래 결과를 추정하고, 역전파 단계에서는 그 결과의 가치를 방문한 노드들을 따라 전달한다. 이 과정을 반복하면 의사결정 가치에 대한 추정 품질이 점진적으로 향상된다.

AMR에서 행동 공간(Action Space)은 CONTINUE, SLOW, STOP, YIELD, AVOID, PASS, WAIT, REROUTE, DOCK과 같은 행동을 포함할 수 있다. 계획기는 현재 관측에만 기반하여 의사결정하는 대신 제한된 미래 시간 범위(Future Horizon)에 걸쳐 이러한 대안들을 평가할 수 있다. 따라서 후보 행동은 예상 진행도(Expected Progress), 충돌 위험(Collision Risk), 이동 시간(Travel Time), 에너지 소비(Energy Consumption), 안전 여유(Clearance), 임무 완료(Mission Completion) 및 기타 운용 목표에 따라 평가될 수 있다. 정확한 보상(Reward) 또는 비용(Cost) 구성은 로봇의 운용 설계(Operational Design)를 반영해야 한다.

MCTS는 로봇이 미래 행동이 불확실한 다른 에이전트(Agent)와 상호작용할 때 특히 유용하다. 보행자는 계속 걸어갈 수도 있고, 멈추거나 방향을 변경하거나 로봇의 경로를 횡단할 수도 있다. 다른 차량 역시 가속하거나 양보하거나 충돌 가능 영역(Conflict Region)을 점유할 수 있다. 하나의 결정론적인 미래를 가정하는 대신 계획기는 여러 가능한 결과를 평가하고 가능한 시나리오 전반에서 수용 가능한 성능을 보이는 행동을 선택할 수 있다. 따라서 이 방법은 현재 장면의 즉각적인 기하학적 구조보다 미래 상호작용이 더 중요한 의사결정 문제에 적합하다.

게임이론 기반 행동 계획(Game-Theoretic Behavior Planning)은 여러 의사결정 에이전트 사이의 상호작용을 명시적으로 모델링함으로써 이러한 개념을 확장한다. 로봇과 주변 차량 또는 보행자는 서로의 행동에 영향을 주는 플레이어(Player)로 표현될 수 있다. 계획기는 로봇이 무엇을 할 수 있는지만 고려하는 것이 아니라 다른 에이전트가 어떻게 반응할 수 있는지도 고려한다. 모델링 가정에 따라 이러한 상호작용은 최적 반응(Best Response), 균형(Equilibrium), 협력적 행동(Cooperative Behavior), 경쟁적 행동(Competitive Behavior), 확률적 반응 모델(Probabilistic Response Model)과 같은 개념을 이용하여 구성할 수 있다.

실용적인 AMR 계획기는 주변의 모든 객체를 전략적 플레이어(Strategic Player)로 모델링할 필요는 없다. 전략적 추론(Strategic Reasoning)은 로봇의 의사결정에 실질적인 영향을 미칠 수 있는 에이전트에 선택적으로 적용해야 한다. 예를 들어 횡단 영역(Crossing Region)에 접근하는 보행자는 상호작용 추론(Interaction Reasoning)이 필요할 수 있지만, 정지된 벽은 그렇지 않다. 이러한 선택적 모델링은 계산 요구량(Computational Requirement)을 줄이고 미래 결과에 영향을 줄 수 있는 에이전트에 의사결정 문제를 집중시킨다.

예측(Prediction)과 게임이론적 추론(Game-Theoretic Reasoning)은 명시적인 불확실성 모델(Uncertainty Model)을 통해 연결되어야 한다. 보행자에게 여러 가지 가능한 미래 궤적(Future Trajectory)이 있다면 각각의 궤적에 확률 또는 신뢰도 추정값을 할당할 수 있다. 계획기는 이러한 가능한 미래들에 걸쳐 각 로봇 행동의 예상 결과를 평가할 수 있다. 불확실성이 높은 경우 시스템은 가장 높은 명목 효율(Nominal Efficiency)을 갖는 행동보다 더 큰 안전 여유(Safety Margin) 또는 더 낮은 최악 결과(Worst-Case Consequence)를 갖는 행동을 선택할 수 있다.

MCTS의 계산 비용(Computational Cost)은 중요한 아키텍처 고려사항이다. 행동 계획기는 제한된 의사결정 지연시간(Decision Latency) 안에서 동작해야 하며, 동시에 인지(Perception), 예측(Prediction), 위치추정(Localization), 궤적 생성(Trajectory Generation), 제어(Control)가 계속 실행되어야 한다. 따라서 탐색 깊이(Search Depth), 분기 계수(Branching Factor), 시뮬레이션 횟수(Simulation Count), 예측 시간 범위(Prediction Horizon), 상태 표현(State Representation)을 적절하게 제한해야 한다. 점진적 확장(Progressive Widening), 행동 가지치기(Action Pruning), 휴리스틱 초기화(Heuristic Initialization), 병렬 시뮬레이션(Parallel Simulation), 학습된 가치 함수(Learned Value Function)를 활용하면 유용한 의사결정 품질을 유지하면서 불필요한 탐색을 줄일 수 있다.

MCTS와 궤적 생성(Trajectory Generation) 사이의 인터페이스는 명확하게 정의되어야 한다. MCTS는 일반적으로 저수준 조향 명령(Low-Level Steering Command)이나 휠 명령(Wheel Command)을 직접 생성하기보다 행동 의도(Behavioral Intention)를 선택하거나 순위를 정하는 역할을 수행해야 한다. AVOID 또는 YIELD와 같은 행동이 선택되면 궤적 생성 계층(Trajectory-Generation Layer)이 기하학적 및 동역학적으로 실행 가능한 궤적을 생성할 수 있다. 이러한 궤적은 탐색 과정에서 후보 결과로 다시 평가될 수 있으며, 이를 통해 행동 계획기가 선택한 행동이 실제 로봇에서 실행 가능한지를 고려할 수 있다.

안전 제약조건(Safety Constraint)은 제한되지 않은 탐색 공간(Unrestricted Search Space) 외부에서 관리하거나 탐색 공간 내부의 하드 제약조건(Hard Constraint)으로 적용해야 한다. MCTS는 추정 보상(Estimated Reward)이 높다는 이유만으로 충돌 제약조건(Collision Constraint), 운용 경계(Operational Boundary), 로봇 한계(Vehicle Limitation), 비상 조건(Emergency Condition)을 위반하는 행동을 선택해서는 안 된다. 안전 감독(Safety Supervision)은 실행 전에 유효하지 않은 가지를 제거할 수 있으며 실제 환경이 변화하면 이미 선택된 행동을 재정의할 수 있다. 이를 통해 최적화(Optimization)와 협상할 수 없는 안전 요구사항(Non-Negotiable Safety Requirement)을 분리할 수 있다.

실외 AMR에서는 일반적인 도로보다 환경이 덜 구조화되어 있기 때문에 게임이론적 문제가 더욱 복잡해질 수 있다. 로봇은 보행자, 서비스 차량, 지게차, 건설 장비, 게이트, 협소 통로, 임시 장애물 주변에서 동작할 수 있다. 도로 교통 규칙은 현장별 운용 정책(Site-Specific Operational Policy) 및 지형 제약조건(Terrain Constraint)과 동시에 적용될 수 있다. MCTS는 이러한 요소를 미래 행동 평가에 포함할 수 있으며, 게임이론 모델은 로봇이 이용할 수 있는 선택지에 영향을 미치는 특정 에이전트와의 상호작용을 표현할 수 있다.

계층형 계획(Hierarchical Planning)은 상호작용을 고려하는 탐색의 복잡성을 줄일 수 있다. 상위 수준의 임무 계획기(High-Level Mission Planner)는 검사 영역(Inspection Zone)이나 도킹 스테이션(Docking Station)에 도달하는 것과 같은 현재 목표를 결정하고, MCTS는 제한된 수의 행동 대안을 대상으로 지역적인 탐색을 수행할 수 있다. 이를 통해 탐색 트리가 장기적인 임무 의사결정과 모든 단기 장애물 상호작용을 하나의 공간에서 혼합하는 것을 방지할 수 있다. 결과적으로 전략적 임무 관리(Strategic Mission Management)와 지역적 상호작용 행동 계획(Local Interactive Behavior Planning)을 분리하면서도 두 수준 사이에서 정보를 전달할 수 있다.

유용한 평가 함수(Evaluation Function)는 하나의 보상에 의존하기보다 여러 차원을 결합해야 한다. 임무 목표를 향한 진행도(Progress)는 충돌 확률(Collision Probability), 최소 여유 거리(Minimum Clearance), 시간 비용(Time Cost), 에너지 소비(Energy Consumption), 이동의 부드러움(Motion Smoothness), 경로 이탈(Route Deviation), 운용 제약조건과 결합할 수 있다. 하드 안전 위반(Hard Safety Violation)은 소프트 최적화 목표(Soft Optimization Objective)와 다르게 처리해야 한다. 이러한 구분을 통해 이동 효율의 향상이 필수 안전 제약조건을 위반하는 행동을 보상하는 상황을 방지할 수 있다.

행동 계획기는 자신의 예측과 시뮬레이션에 대한 신뢰성도 고려해야 한다. 부정확한 객체 예측(Object Prediction), 불량한 위치추정(Poor Localization), 불완전한 환경 모델(Incomplete Environmental Model)에 기반한 탐색 결과는 잘못된 확신(False Confidence)을 만들 수 있다. 따라서 신뢰도 값(Confidence Value), 모델 불확실성(Model Uncertainty), 예측 불일치(Prediction Disagreement), 시뮬레이션 유효성(Simulation Validity)을 가지 평가에 포함할 수 있다. 불확실성이 정의된 임계값을 초과하면 계획기는 WAIT, SLOW, STOP 또는 검증된 최소위험 대응(Minimal-Risk Response)과 같은 보수적인 행동으로 전환할 수 있다.

MCTS와 게임이론 기반 계획은 여러 에이전트와 미래 상태 사이의 상호작용에 의존하기 때문에 시나리오 기반 검증(Scenario-Based Validation)이 필요하다. 시뮬레이션에서는 보행자 의도(Pedestrian Intent), 차량 반응(Vehicle Response), 장애물 움직임(Obstacle Motion), 위치추정 오류(Localization Error), 통신 지연(Communication Delay), 환경 제약조건(Environmental Constraint)을 변화시킬 수 있다. 반복 시험을 통해 충돌 회피(Collision Avoidance), 임무 진행(Mission Progress), 의사결정 안정성(Decision Stability), 계산 지연시간(Computational Latency), 행동 일관성(Behavior Consistency)을 측정할 수 있다. 중요한 엣지 케이스(Edge Case)에는 갑작스럽게 방향을 변경하는 에이전트, 동시 충돌 상황, 불완전한 관측, 그리고 여러 행동의 예측 가치가 서로 유사한 상황이 포함된다.

탐색 기반 행동 결정(Search-Based Behavior Decision)을 이해하기 위해서는 로깅(Logging)이 필수적이다. 시스템은 선택된 행동(Selected Behavior), 후보 대안(Candidate Alternative), 추정 가치(Estimated Value), 관련 예측 가정(Prediction Assumption), 안전 제약조건, 탐색 통계(Search Statistics), 최종 실행 결과(Final Execution Result)를 기록해야 한다. 이를 통해 엔지니어는 예상하지 못한 의사결정이 인지, 예측, 탐색 구성, 보상 설계, 안전 개입(Safety Intervention) 중 어느 요소에서 발생했는지를 분석할 수 있다. 이러한 추적 가능성(Traceability)은 탐색 기반 행동이 결정론적인 규칙 평가보다 복잡해질수록 더욱 중요해진다.

MCTS와 게임이론 기반 계획은 기존의 모든 결정론적 행동 계획을 대체하는 방법이라기보다 이를 보완하고 확장하는 방법으로 이해해야 한다. FSM과 규칙 엔진(Rule Engine)은 명시적인 운용 정책, 안전 제약조건, 예측 가능한 상태 관리를 위해 여전히 유용하며, MCTS는 다양한 미래 행동을 평가하고 게임이론적 추론은 의사결정 에이전트 사이의 상호작용을 처리할 수 있다. 계층형 아키텍처(Layered Architecture)는 시나리오의 복잡도에 따라 이러한 메커니즘을 조합할 수 있다.

실외 AMR에서 실질적인 목표는 가능한 모든 미래를 탐색하는 것이 아니라, 제한된 계산 자원과 안전 프레임워크 안에서 가장 중요한 미래 의사결정을 탐색하는 것이다. 인지와 예측은 구조화된 정보를 제공하고, 탐색 계층(Search Layer)은 후보 행동을 평가하며, 게임이론적 추론은 선택된 상호작용 상황을 처리하고, 안전 감독은 이용 가능한 행동을 제한하며, 궤적 생성은 선택된 행동을 실행 가능한 움직임으로 변환한다. 이러한 아키텍처는 결정론적인 행동 논리에서 보다 예측 지향적이고 상호작용을 인식하는 자율성(Interaction-Aware Autonomy)으로 발전할 수 있는 경로를 제공하면서도 운용 제어(Operational Control)와 검증 요구사항(Validation Requirement)을 유지할 수 있다.

## 06.05. Prediction Aware Behavior Planning [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

예측 인지형 행동 계획(Prediction-Aware Behavior Planning)은 주변 에이전트와 환경 조건이 시간에 따라 어떻게 변화할 수 있는지를 고려함으로써 기존 행동 계획(Behavior Planning)을 확장한다. 현재 장면에만 반응하는 대신, 계획기는 예측된 미래 상태(Future State)를 사용하여 정의된 계획 시간 범위(Planning Horizon) 동안 적절성을 유지할 수 있는 행동을 선택한다. Chapter 06에서는 이 주제가 MCTS 및 게임이론 기반 행동 계획(MCTS and Game-Theoretic Behavior Planning) 다음에 위치하며, 행동 의사결정과 미래 움직임 예측(Future Motion Prediction) 및 상호작용 인지 자율성(Interaction-Aware Autonomy)을 연결한다.

예측 인지형 계획(Prediction-Aware Planning)의 기본 입력은 가능한 미래 상태를 표현한 정보이다. 객체 추적(Object Tracking)은 위치와 속도를 제공할 수 있으며, 예측 모듈(Prediction Module)은 가능한 미래 궤적(Future Trajectory), 의도(Intent), 행동 확률(Behavior Probability)을 추정한다. 예를 들어 도로 횡단 구역에 접근하는 보행자는 계속 전진하거나, 정지하거나, 방향을 변경하거나, 로봇의 경로로 진입하는 여러 가능한 미래를 가질 수 있다. 행동 계획기는 이러한 예측을 사용하여 계속 진행(Continue), 감속(Slow), 양보(Yield), 대기(Wait), 재경로 설정(Reroute) 중 어떤 행동이 적절한지를 평가한다.

미래가 불확실한 경우 예측은 하나의 결정론적인 궤적(Deterministic Trajectory)으로 취급해서는 안 된다. 실용적인 시스템은 관련 확률 또는 신뢰도 값(Confidence Value)과 함께 여러 가설(Hypothesis)을 유지할 수 있다. 이후 계획기는 하나의 예측 궤적이 반드시 발생한다고 가정하는 대신 이러한 대안들을 대상으로 후보 행동(Candidate Behavior)을 평가할 수 있다. 이를 통해 로봇은 불확실성을 명시적으로 고려하고 다양한 가능한 미래 결과에서도 수용 가능한 행동을 선택할 수 있다.

예측 인지형 계획기(Prediction-Aware Planner)는 일반적으로 현재 자차 상태(Ego State), 인식된 객체(Perceived Object), 지도 문맥(Map Context), 임무 목표(Mission Objective), 예측된 에이전트 움직임(Predicted Agent Motion)을 결합한다. 자차 상태는 위치, 속도, 가속도 및 차량 제약조건을 제공하고, 장면 모델(Scene Model)은 객체 위치, 분류 및 동적 상태를 제공한다. 지도 정보는 경로 구조, 의미론적 영역(Semantic Region), 운용 제약조건을 제공한다. 예측 계층(Prediction Layer)은 시간적 정보를 추가하여 계획기가 이미 발생한 충돌뿐 아니라 미래에 발생할 수 있는 충돌도 추론할 수 있도록 한다.

계획 시간 범위(Planning Horizon)는 로봇의 속도, 환경 복잡도(Environment Complexity), 의사결정 지연시간(Decision Latency)에 따라 선택해야 한다. 지나치게 짧은 시간 범위는 몇 초 후 위험해질 상호작용을 발견하지 못할 수 있으며, 지나치게 긴 시간 범위는 예측 불확실성과 불필요한 계산량을 증가시킬 수 있다. 따라서 계획기는 의사결정에 충분히 유용한 예측 정보를 제공할 수 있는 제한된 시간 범위를 가져야 한다. 이 시간 범위는 속도, 시나리오 유형(Scenario Type), 예측 움직임의 불확실성에 따라 변경될 수도 있다.

충돌까지의 시간(Time-to-Collision, TTC)과 충돌 영역 분석(Conflict-Region Analysis)은 예측 인지형 의사결정에 유용한 개념이다. 단순히 현재 장애물이 로봇의 경로 안에 있는지를 판단하는 대신, 계획기는 로봇과 다른 에이전트가 대략 동일한 시간에 동일한 영역을 점유할 것으로 예상되는지를 판단할 수 있다. 따라서 로봇에서 몇 미터 떨어진 보행자라도 보행자의 예측 궤적이 로봇의 미래 경로와 교차한다면 가까이에 있는 정적 객체보다 더 중요한 요소가 될 수 있다.

예측 인지형 계획은 정적 장애물(Static Obstacle)과 상호작용 장애물(Interactive Obstacle)을 구분해야 한다. 정적 장애물은 일반적으로 로봇에 대응하여 행동을 변경하지 않지만, 보행자, 자전거, 차량, 다른 로봇은 로봇의 움직임에 반응할 수 있다. 이러한 상호작용은 선택된 로봇 행동이 다른 에이전트의 미래 행동을 변화시킬 수 있는 피드백 관계(Feedback Relationship)를 만든다. 따라서 예측은 보장된 미래 사건이 아니라 조건부 가능한 결과(Conditional Possibility)로 해석해야 한다.

예를 들어 AMR이 보행자 횡단 구역에 접근할 때 적극적인 계속 주행(Aggressive Continuation)은 보행자가 멈추거나 방향을 변경하게 만들 수 있지만, 제어된 감속(Controlled Slowdown)은 보행자가 자연스럽게 횡단하도록 할 수 있다. 계획기는 예측된 궤적과 행동 반응(Behavioral Response)을 이용하여 이러한 가능성을 평가할 수 있다. 목표는 반드시 사람의 정확한 행동을 예측하는 것이 아니라, 인간과의 상호작용에 존재하는 불확실성 아래에서 충분히 안전하고 효율적인 행동을 식별하는 것이다.

예측 품질(Prediction Quality)은 계획 인터페이스(Planning Interface)에서 명시적으로 표현해야 한다. 유용한 정보에는 궤적 신뢰도(Trajectory Confidence), 예측 시간 범위(Prediction Horizon), 다중 모달 가설(Multimodal Hypothesis), 불확실성 추정(Uncertainty Estimate), 모델 유효성(Model Validity)이 포함된다. 예측 신뢰도가 높으면 계획기는 보다 구체적인 행동 결정을 사용할 수 있다. 반대로 신뢰도가 낮거나 여러 예측이 크게 불일치하면 안전 여유(Safety Margin)를 증가시키거나 SLOW, WAIT, STOP과 같은 보수적인 행동을 선택할 수 있다.

예측과 행동 계획 사이의 관계에는 시간적 일관성(Temporal Consistency)도 포함되어야 한다. 새로운 센서 관측이 들어오면 각 계획 주기마다 예측이 변경될 수 있다. 계획기가 예측 궤적의 작은 변화가 발생할 때마다 즉시 행동을 변경하면 로봇은 행동 사이에서 진동(Oscillation)할 수 있다. 필터링(Filtering), 히스테리시스(Hysteresis), 예측 지속성(Prediction Persistence), 최소 행동 지속시간(Minimum Behavior Duration)을 사용하여 의사결정을 안정화하면서도 실제 안전 필수 변화(Safety-Critical Change)가 발생하면 신속하게 대응할 수 있다.

예측 인지형 계획은 결정론적 규칙(Deterministic Rule), 확률 모델(Probabilistic Model), 최적화(Optimization), MCTS, 게임이론적 추론(Game-Theoretic Reasoning), 학습 기반 정책(Learned Policy)을 사용하여 구현할 수 있다. 규칙 기반 시스템은 예측된 충돌 조건을 사용하여 YIELD 또는 STOP을 발생시킬 수 있다. 확률론적 계획기는 여러 궤적 가설에 걸쳐 예상 결과를 평가할 수 있다. MCTS는 미래 행동 시퀀스를 시뮬레이션할 수 있고, 게임이론 모델은 다른 의사결정 에이전트가 로봇의 행동에 어떻게 반응할지를 고려할 수 있다. 따라서 이러한 방법들은 서로 배타적인 대안이 아니라 상호 통합될 수 있다.

평가 함수(Evaluation Function)는 안전 제약조건(Safety Constraint)과 최적화 목표(Optimization Objective)를 구분해야 한다. 충돌 위험(Collision Risk), 최소 여유 거리(Minimum Clearance), 비상 제약조건(Emergency Constraint)은 하드 제한(Hard Restriction)으로 작동해야 하며, 진행도(Progress), 이동 시간(Travel Time), 에너지 소비(Energy Consumption), 경로 이탈(Route Deviation), 이동 부드러움(Motion Smoothness)은 안전 영역(Safe Region) 안에서 최적화할 수 있다. 이러한 구분은 높은 효율성을 가진 행동의 예측 보상이 허용할 수 없는 안전 위험을 수치적으로 상쇄한다는 이유만으로 선택되는 것을 방지한다.

행동 계획기는 자신의 예측과 시뮬레이션의 신뢰성도 고려해야 한다. 부정확한 객체 예측(Object Prediction), 불량한 위치추정(Poor Localization), 불완전한 환경 모델(Incomplete Environmental Model)에 기반한 탐색 결과는 잘못된 확신(False Confidence)을 만들 수 있다. 따라서 신뢰도 값, 모델 불확실성, 예측 불일치(Prediction Disagreement), 시뮬레이션 유효성을 가지 평가(Branch Evaluation)에 포함할 수 있다. 불확실성이 정의된 임계값을 초과하면 계획기는 WAIT, SLOW, STOP 또는 검증된 최소위험 대응(Minimal-Risk Response)과 같은 보수적인 행동으로 전환할 수 있다.

실외 AMR은 일반적인 도로보다 구조화 정도가 낮은 환경에서 예측 인지형 계획을 수행해야 한다. 로봇은 야드(Yard), 캠퍼스(Campus), 적재 구역(Loading Area), 건설 구역(Construction Zone)에서 보행자, 지게차, 서비스 차량, 건설 장비, 자전거, 임시 장벽(Temporary Barrier), 다른 이동 객체를 만날 수 있다. 이러한 객체의 이동 패턴은 환경에 따라 크게 달라질 수 있다. 따라서 운용 문맥(Operational Context)은 예측을 해석하는 방식과 계획기가 선택하는 행동 반응 모두에 영향을 주어야 한다.

예측은 임무 수준 행동(Mission-Level Behavior)과도 연결되어야 한다. 일시적인 보행자 상호작용이 로봇의 전체 임무를 반드시 변경해야 하는 것은 아니다. 검사 로봇(Inspection Robot)이 지정된 검사 지점(Inspection Point)으로 이동하는 중 보행자를 만났다면 지역 계획기(Local Planner)는 일시적으로 양보하거나 대기하면서 상위 수준의 임무는 그대로 유지할 수 있다. 상호작용이 해결되면 로봇은 임무를 재개할 수 있다. 이러한 분리는 단기 예측이 장기 목표(Long-Term Objective)를 불필요하게 방해하는 것을 방지한다.

위치추정 오류(Localization Error)와 예측 오류(Prediction Error)는 서로 상호작용할 수 있다. 로봇의 추정 위치가 불확실하면 예측된 충돌 위치 역시 불확실해질 수 있다. 마찬가지로 잘못된 객체 연관(Object Association)이나 속도 추정(Velocity Estimation)은 잘못된 미래 궤적을 생성할 수 있다. 따라서 행동 계획기는 예측 모듈을 완벽한 미래 정보의 원천으로 취급하는 대신 인지, 추적, 위치추정, 예측에서 발생하는 불확실성을 함께 고려해야 한다.

계획기는 예측(Prediction), 행동 선택(Behavior Selection), 궤적 생성(Trajectory Generation), 제어(Control) 사이에 명확한 인터페이스를 제공해야 한다. 예측은 가능한 미래 상태를 제공하고, 행동 계획은 의도된 대응을 선택하며, 궤적 생성은 실행 가능한 경로와 속도 프로파일을 구성하고, 제어는 해당 궤적을 실행한다. 이러한 분리를 통해 전체 자율주행 스택(Autonomy Stack)을 다시 설계하지 않고도 다양한 예측 및 계획 알고리즘을 교체하거나 개선할 수 있다. 또한 각 인터페이스에서 명확하게 정의된 입력, 출력, 타임스탬프, 신뢰도 정보를 제공할 수 있기 때문에 시험도 용이해진다.

안전 감독(Safety Supervision)은 예측 인지형 의사결정을 재정의할 수 있는 능력을 유지해야 한다. 예측 결과 장애물이 이동하여 사라질 가능성이 높다고 판단하더라도 실제 객체가 갑자기 방향을 변경할 수 있다. 따라서 로봇은 예측된 행동에만 의존할 수 없다. 런타임 안전 검사(Runtime Safety Check)는 실제 장면과 예측된 충돌 상태를 지속적으로 평가해야 한다. 기존에 선택된 행동이 안전하지 않게 되면 시스템은 해당 행동을 거부하고 검증된 대체 행동(Fallback Behavior) 또는 최소위험 행동으로 전환할 수 있어야 한다.

검증(Validation)은 예측 정확도와 행동 결과를 모두 대상으로 해야 한다. 예측 모델이 평균 궤적 오차 측면에서 양호한 성능을 보이더라도 희귀한 상호작용에서 문제가 되는 행동을 유발할 수 있다. 따라서 시나리오 기반 시험(Scenario-Based Testing)에서는 보행자 의도, 차량 속도, 예상하지 못한 정지, 방향 변경, 가림(Occlusion), 센서 잡음(Sensor Noise), 위치추정 오류, 통신 지연 등을 변화시켜야 한다. 평가는 예측 성능 지표뿐 아니라 충돌 회피, 의사결정 안정성, 불필요한 정지, 임무 진행도, 계산 지연시간도 측정해야 한다.

현장 운용 데이터(Field Operation Data)는 예측 인지형 계획을 개선하기 위한 중요한 자료를 제공한다. 기록된 궤적은 예측 모델이 불확실하거나 부정확한 가설을 반복적으로 생성하는 상황을 보여줄 수 있으며, 행동 로그(Behavior Log)는 계획기가 불필요하거나 지나치게 보수적인 행동을 선택한 사례를 식별할 수 있다. 이러한 사례는 회귀 시나리오(Regression Scenario)로 변환하여 이후 예측 모델, 계획 정책(Planning Policy), 의사결정 임계값(Decision Threshold)의 변경사항을 평가하는 데 사용할 수 있다.

실용적인 예측 인지형 아키텍처(Prediction-Aware Architecture)는 예측을 미래에 대한 확정적인 설명이 아니라 의사결정을 지원하는 정보(Decision-Support Information)로 취급해야 한다. 계획기는 여러 가능한 미래를 현재 상태 정보, 안전 제약조건, 임무 목표, 로봇 능력과 결합한다. 이후 가장 중요한 예측 결과에 적합한 행동을 선택하면서 불확실성이 과도해질 경우를 위한 대체 행동(Fallback)을 유지한다.

따라서 행동 계획은 명시적인 FSM과 규칙 기반 의사결정에서 점점 더 예측 지향적이고 상호작용을 고려하는 방법으로 발전할 수 있다. FSM은 구조화된 행동 상태를 제공하고, 규칙 엔진은 명시적인 시나리오 논리를 제공하며, MCTS는 미래 탐색을 제공하고, 예측 인지형 계획은 시간적 변화를 행동 선택에 직접 통합한다. 이러한 메커니즘은 현재 객체가 어디에 있는지만이 아니라 객체가 어디로 이동할 수 있으며 그 움직임이 미래의 로봇 행동에 어떤 영향을 줄 수 있는지를 고려해야 하는 실외 AMR을 위한 기반을 제공한다.

## 06.06. LLM Assisted Behavior Planning for Complex Scenes [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

대규모 언어 모델 지원 행동 계획(LLM-Assisted Behavior Planning)은 언어 처리 능력을 갖춘 모델을 활용하여 복잡한 장면(Complex Scene), 관계(Relationship), 지시사항(Instruction), 문맥적 제약조건(Contextual Constraint)을 해석함으로써 자율 의사결정 스택(Autonomous Decision-Making Stack)에 의미론적 추론(Semantic Reasoning)을 도입한다. LLM은 결정론적 계획기(Deterministic Planner)를 대체하기보다 풍부한 장면 설명을 구조화된 행동 제안(Structured Behavioral Suggestion)으로 변환하는 상위 수준 추론 구성요소(High-Level Reasoning Component)로 동작할 수 있다. 실외 자율이동로봇(Outdoor AMR)에서는 고정된 규칙만으로 효율적으로 표현하기 어려운 상황에서 특히 유용하다.

LLM은 제한되지 않은 원시 센서 스트림(Raw Sensor Stream)이 아니라 구조화된 환경 표현(Structured Environment Representation)을 입력으로 받아야 한다. 인지(Perception), 추적(Tracking), 위치추정(Localization), 매핑(Mapping), 예측(Prediction), 임무(Mission) 모듈은 센서 관측을 기호적 또는 텍스트 기반의 장면 정보(Scene Information)로 변환할 수 있다. 이후 객체, 위치, 예측 움직임, 의미론적 구역(Semantic Zone), 경로 상태(Route Condition), 운용 제한(Operational Restriction), 로봇 상태(Robot Status)를 간결한 장면 설명으로 표현하여 모델에 필요한 의사결정 문맥(Decision Context)을 제공할 수 있다.

장면 표현(Scene Representation)은 로봇이 협소 통로(Narrow Passage)에 접근하고 있으며, 보행자가 입구 근처에 서 있고, 서비스 차량(Service Vehicle)이 동일한 영역으로 이동하고 있으며, 계획된 경로가 해당 통로를 지나간다는 내용을 기술할 수 있다. LLM은 이러한 사실 사이의 관계를 추론하고 SLOW, YIELD, WAIT, REROUTE와 같은 후보 행동(Candidate Behavior)을 제안할 수 있다. 그러나 최종 행동은 결정론적 검증(Deterministic Validation)과 안전 제약조건(Safety Constraint)의 적용을 받아야 한다.

이러한 의미론적 능력(Semantic Capability)은 장면의 의미가 단순한 기하학적 관계가 아니라 문맥에 따라 결정될 때 특히 중요하다. 임시 장벽(Temporary Barrier), 작업자의 제스처(Worker Gesture), 유지보수 작업(Maintenance Activity), 적재 작업(Loading Operation), 비정상적인 객체 배치(Unusual Object Arrangement)는 모두 완전하게 규칙으로 작성하기 어려운 운용 제약조건을 의미할 수 있다. LLM은 객체 의미(Object Semantics), 임무 문맥(Mission Context), 현장 규칙(Site Rule), 자연어 지시사항(Natural-Language Instruction)을 결합하여 기존의 기하학 기반 계획(Geometric Planning)을 보완하는 상위 수준 해석을 생성할 수 있다.

LLM 지원 계획(LLM-Assisted Planning)은 자연어 임무 지시(Natural-Language Mission Instruction)와 실행 가능한 로봇 행동 사이의 인터페이스를 제공할 수도 있다. 운영자는 로봇에게 적재 구역을 검사하고, 작업자를 방해하지 않으며, 게이트가 사용 중이면 대기하고, 작업을 완료한 후 충전 스테이션으로 복귀하도록 요청할 수 있다. LLM은 이러한 지시를 기존 임무 및 행동 계획기가 처리할 수 있는 구조화된 임무 목표(Structured Mission Objective), 행동 제약조건(Behavioral Constraint), 조건부 행동(Conditional Action)으로 분해할 수 있다.

아키텍처는 의미론적 추론(Semantic Reasoning)과 이동 필수 실행(Motion-Critical Execution)을 명확하게 분리해야 한다. LLM은 상황을 해석하고, 관련 제약조건을 식별하고, 후보 행동을 생성하거나 대안을 설명할 수 있지만, 궤적 생성(Trajectory Generation)과 저수준 제어(Low-Level Control)는 전용 결정론적 또는 검증된 모듈에서 유지되어야 한다. 이러한 분리는 자유 형식 언어 생성(Free-Form Language Generation)이 적절한 검증 없이 직접 조향, 가속, 제동 또는 휠 명령을 생성하는 것을 방지한다.

따라서 유용한 LLM 출력은 제한되지 않은 자연어 대신 제약된 스키마(Constrained Schema)를 따라야 한다. 출력에는 인식된 시나리오 유형(Scenario Type), 관련 객체(Relevant Entity), 추론된 관계(Inferred Relationship), 후보 행동, 행동 제약조건, 신뢰도 정보(Confidence Information), 간략한 추론 요약(Reasoning Summary)이 포함될 수 있다. 구조화된 출력(Structured Output)을 사용하면 하위 소프트웨어가 값을 검증하고, 지원되지 않는 행동을 거부하며, 후보를 비교하고, 기반 언어 모델이 변경되더라도 안정적인 인터페이스를 유지할 수 있다.

그라운딩(Grounding)은 LLM이 현재 환경에서 지원되지 않는 그럴듯한 내용을 생성할 수 있기 때문에 필수적이다. 계획에 사용되는 모든 장면 관련 주장은 인지, 지도, 임무 데이터, 예측 또는 승인된 운용 지식(Approved Operational Knowledge)이 제공하는 정보와 연결되어야 한다. 모델이 이러한 정보원을 통해 확인할 수 없는 객체, 규칙 또는 조건을 언급하는 경우 행동 계획기는 이를 환경적 사실(Environmental Fact)로 받아들이지 않고 지원되지 않는 정보(Unsupported Information)로 처리해야 한다.

검색 증강 생성(Retrieval-Augmented Generation, RAG)은 프롬프트(Prompt)에 직접 포함하기에는 지나치게 방대하거나 자주 변경되는 운용 지식을 제공할 수 있다. 현장 절차(Site Procedure), 임무 매뉴얼(Mission Manual), 제한구역 정책(Restricted-Zone Policy), 도킹 지침(Docking Instruction), 장비 상호작용 규칙(Equipment Interaction Rule), 검증된 행동 지침(Validated Behavioral Guideline)을 현재 문맥에 따라 검색할 수 있다. LLM은 선택된 정보를 기반으로 추론하고, 시스템은 어떤 운용 문서가 행동 제안에 사용되었는지를 보여주는 출처 추적 정보(Provenance)를 유지할 수 있다.

복잡한 장면 추론(Complex-Scene Reasoning)은 예측(Prediction)과 의미론적 이해(Semantic Understanding)를 결합함으로써 더욱 향상될 수 있다. 기존 예측 모듈은 지게차가 공유 영역(Shared Region)에 진입할 가능성이 높다고 판단하고, 의미론적 정보는 해당 영역이 현재 활성화된 적재 구역(Active Loading Zone)임을 식별할 수 있다. LLM은 이러한 사실을 함께 해석하여 단순히 기하학적으로 충돌이 없는 경로를 계산하는 대신 로봇이 해당 구역 외부에서 대기하도록 제안할 수 있다. 따라서 의미론적 문맥은 예측된 움직임에 부여되는 의미에 영향을 줄 수 있다.

많은 복잡한 장면은 여러 관측에 걸쳐 전개되기 때문에 시간적 추론(Temporal Reasoning)이 필요하다. 작업자가 게이트에 접근하고, 게이트를 열고, 로봇 방향으로 신호를 보내는 상황은 하나의 독립된 프레임이 아니라 연속적인 사건 시퀀스(Event Sequence)이다. LLM 지원 계층은 요약된 사건 이력(Event History)을 입력받아 장면 상태의 변화를 추론할 수 있다. 그러나 상황이 변경된 후에도 오래된 가정이 행동에 계속 영향을 주지 않도록 시간적 결론은 추적된 관측(Tracked Observation)과 연결되어야 한다.

LLM 추론은 명시적인 행동 경계(Behavioral Boundary) 안에서 동작해야 한다. 시스템은 CONTINUE, SLOW, STOP, YIELD, WAIT, AVOID, REROUTE, DOCK, REQUEST_ASSISTANCE, MINIMAL_RISK와 같은 허용된 행동 어휘(Allowed Action Vocabulary)를 정의할 수 있다. 모델은 임의의 행동을 새롭게 생성하는 대신 허용된 행동 중에서만 선택하거나 순위를 결정한다. 추가 제약조건을 통해 속도 등급(Speed Class), 금지 영역(Prohibited Region), 필수 안전 여유(Required Clearance), 항상 결정론적인 안전 대응을 발생시키는 조건을 정의할 수 있다.

안전 감독기(Safety Supervisor)는 LLM과 독립적으로 유지되어야 한다. 모델이 CONTINUE를 권고하더라도 충돌 여유(Collision Margin), 위치추정 유효성(Localization Validity), 운용 경계(Operational Boundary), 시스템 상태 요구사항(System-Health Requirement)이 위반되면 하위 안전 논리가 해당 행동을 거부해야 한다. 비상 정지(Emergency Stop)와 최소위험 행동(Minimal-Risk Behavior)은 성공적인 LLM 추론에 의존해서는 안 된다. 이러한 아키텍처는 언어 모델을 안전 필수 이동에 대한 최종 권한자가 아니라 자문형 추론 구성요소(Advisory Reasoning Component)로 취급한다.

지연시간(Latency) 역시 중요한 고려사항이다. 대규모 모델은 FSM, 규칙 엔진(Rule Engine), 기존 최적화 모듈보다 훨씬 많은 계산량을 요구할 수 있다. 따라서 AMR은 LLM을 최고 주파수의 제어 루프(High-Frequency Control Loop) 내부에 직접 배치하지 않는 것이 바람직하다. 의미론적 추론은 상대적으로 낮은 행동 주파수(Behavioral Frequency)에서 실행하고, 인지, 충돌 검사(Collision Checking), 궤적 추종(Trajectory Tracking), 안전 모니터링은 실시간 이동 제어에 적합한 주기로 계속 실행할 수 있다.

엣지 및 온프레미스 배포(Edge and On-Premise Deployment)는 클라우드 접근이 바람직하지 않거나 불가능한 경우 통신 의존성을 줄이고 운용 데이터를 보호할 수 있다. 소형 언어 모델(Smaller Language Model)은 일반적인 장면 해석을 로컬에서 처리하고, 대형 모델은 오프라인 분석(Offline Analysis), 어려운 시나리오 검토(Scenario Review), 정책 생성(Policy Generation)을 지원할 수 있다. 모델 선택에서는 추론 품질(Reasoning Quality), 지연시간, 메모리 소비(Memory Consumption), 가속기 가용성(Accelerator Availability), 통신 신뢰성(Communication Reliability), 모델 사용 불가 시의 운용 영향을 함께 고려해야 한다.

하이브리드 아키텍처(Hybrid Architecture)는 LLM 추론을 FSM 및 규칙 엔진과 결합할 수 있다. LLM은 비정상적인 장면을 분류하거나 행동 해석(Behavioral Interpretation)을 제안하고, FSM은 운용 상태(Operational State)를 유지하며, 규칙 엔진은 명시적인 제약조건을 검증할 수 있다. 예를 들어 LLM이 적재 작업으로 인해 정상 통행이 일시적으로 차단되었다고 판단하면 규칙 엔진은 해당 구역 진입이 금지되어 있음을 확인하고, FSM은 로봇을 NAVIGATE에서 WAIT 상태로 전환할 수 있다.

LLM은 행동 설명(Behavior Explanation)과 디버깅(Debugging)을 지원할 수도 있다. 계획 스택이 구조화된 장면 사실, 선택된 규칙, 예측 결과, 후보 행동을 제공하면 언어 모델은 시스템이 왜 감속하거나, 대기하거나, 재경로를 설정하거나, 지원을 요청했는지에 대한 사람이 이해할 수 있는 설명을 생성할 수 있다. 이러한 설명은 근거 없이 사후적으로 생성되어서는 안 되며 기록된 증거(Logged Evidence)를 기반으로 해야 한다. 이를 통해 운영자와 엔지니어는 검증된 원인과 모델의 해석을 구분할 수 있다.

의미론적 불확실성(Semantic Uncertainty)이 검증된 한계를 초과하는 장면에서는 사람의 개입(Human Intervention)을 사용할 수 있어야 한다. 모델이 비정상적인 건설 현장 배치, 서로 충돌하는 사람의 지시, 알려지지 않은 객체, 모호한 운용 규칙을 충분한 신뢰도로 해석할 수 없다면 자율적인 결정을 강제하는 것보다 REQUEST_ASSISTANCE를 선택하는 것이 적절할 수 있다. 계획기는 로봇을 안전한 상태로 유지하면서 관련 장면 정보와 후보 해석을 권한이 있는 운영자(Authorized Operator)에게 제공할 수 있다.

LLM 지원 계획의 검증(Validation)은 단순히 언어 품질(Language Quality)을 평가하는 것 이상을 요구한다. 시험에서는 시스템이 허용된 행동을 선택하는지, 운용 제약조건을 준수하는지, 제공된 정보에 근거하여 동작하는지, 상충하는 지시를 처리하는지, 불확실성을 인식하는지, 모델이 실패했을 때 안전하게 전환하는지를 측정해야 한다. 시나리오 시험군(Scenario Suite)에는 비정상적인 객체, 불완전한 설명, 오해를 유발하는 문맥 단서(Contextual Cue), 예측 오류, 상충하는 규칙, 모델의 검증된 운용 영역(Validated Operational Domain) 밖의 상황을 포함해야 한다.

언어 처리 능력을 갖춘 시스템은 운영자, 데이터베이스, 지도 또는 검색된 문서에서 텍스트를 입력받을 수 있으므로 적대적 입력(Adversarial Input)과 잘못된 형식의 입력(Malformed Input)도 고려해야 한다. 외부 텍스트는 자동으로 신뢰할 수 있는 명령으로 취급하는 것이 아니라 데이터로 취급해야 한다. 입력 필터링(Input Filtering), 출처 권한 검증(Source Authorization), 구조화된 인터페이스, 규칙 검증(Rule Validation), 권한 경계(Permission Boundary)를 사용하면 신뢰할 수 없는 콘텐츠가 안전 정책을 변경하거나 계획기가 승인된 행동 집합 밖의 행동을 실행하도록 만드는 것을 방지할 수 있다.

운용 로깅(Operational Logging)은 모델에 제공된 구조화 입력, 검색된 지식(Retrieved Knowledge), 모델 버전(Model Version), 생성된 권고안(Generated Recommendation), 신뢰도 또는 불확실성 정보, 검증 결과(Validation Result), 최종 선택 행동(Selected Final Behavior), 안전 재정의(Safety Override)를 기록해야 한다. 이를 통해 장면 이해와 실행 사이의 추적 가능성(Traceability)을 확보할 수 있다. 이후 모델이나 프롬프트를 업데이트할 때 기록된 시나리오를 이용하여 행동이 어떻게 변경되었으며 그 변화가 검증된 요구사항과 일치하는지를 평가할 수 있다.

따라서 LLM 지원 행동 계획은 더 큰 하이브리드 자율 아키텍처(Hybrid Autonomy Architecture) 안의 의미론적 추론 계층(Semantic Reasoning Layer)으로 사용할 때 가장 효과적이다. 인지와 예측은 물리적 장면을 설명하고, 지도와 임무 시스템은 운용 문맥을 제공하며, LLM은 복잡한 관계를 해석하여 구조화된 행동을 제안한다. 결정론적 모듈은 이러한 제안을 검증하고, 궤적 생성은 승인된 결정을 실행 가능한 움직임으로 변환한다. 이 전체 과정에서 안전 감독은 독립적으로 유지된다.

복잡한 실외 AMR 장면에서 이러한 아키텍처는 가능한 모든 문맥 조합을 수작업으로 규칙화해야 하는 한계를 넘어설 수 있는 방법을 제공한다. FSM은 예측 가능한 운용 상태를 유지하고, 규칙 엔진은 명시적인 정책을 강제하며, 예측 모델(Prediction Model)은 미래 움직임을 추정하고, 탐색 방법(Search Method)은 대안을 평가하며, LLM은 비정상적이거나 문맥 정보가 풍부한 상황에 대한 의미론적 해석을 제공한다. 이러한 기술의 결합은 실제 배포 가능한 자율 시스템(Deployable Autonomous System)에 필요한 결정론적 인터페이스(Deterministic Interface), 추적 가능성, 제한된 권한(Bounded Authority), 독립적인 안전 메커니즘을 유지하면서 행동의 유연성을 향상시킬 수 있다.

## 06.07. Intersection and Roundabout Behavior Planning [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

교차로 및 회전교차로 행동 계획(Intersection and Roundabout Behavior Planning)은 자율주행 차량 또는 실외 자율이동로봇(Outdoor Autonomous Mobile Robot, AMR)이 도로 형상(Road Geometry), 통행 우선권 제약조건(Right-of-Way Constraint), 임무 목표(Mission Objective)를 준수하면서 여러 교통 참여자(Traffic Participant)와 움직임을 조정해야 하는 상황을 다룬다. 단순한 차로 추종(Lane Following)과 달리 이러한 환경에는 서로 교차하거나 합류하는 궤적이 존재하여 일시적인 충돌 영역(Conflict Region)이 형성된다. 따라서 행동 계획기는 기동을 선택하기 전에 공간적 관계와 미래 상호작용의 시간적 관계를 함께 추론해야 한다.

교차로(Intersection)는 접근 차로(Approach Lane), 정지선(Stop Line), 횡단보도(Crosswalk), 회전 경로(Turning Path), 우선순위 관계(Priority Relationship), 충돌 영역의 집합으로 표현할 수 있다. 지도 및 인지 정보는 기하학적·의미론적 구조를 제공하고, 위치추정(Localization)은 이러한 요소에 대한 로봇의 상대 위치를 결정한다. 행동 계획기는 이 정보와 감지된 차량, 보행자, 자전거 및 기타 이동 객체를 결합하여 APPROACH, STOP, YIELD, WAIT, ENTER, TURN 또는 PROCEED 중 어떤 행동을 수행해야 하는지를 결정한다.

행동 계획은 일반적으로 로봇이 실제 교차로에 도달하기 전에 시작된다. APPROACH 상태에서 계획기는 교차로 유형을 식별하고, 예정된 경로(Intended Route)를 확인하며, 적용 가능한 교통 또는 운용 규칙을 평가하고, 필요한 경우 속도를 감소시킨다. 또한 진입 전에 어떤 영역이 비어 있어야 하는지를 결정한다. 이러한 사전 추론은 로봇이 충돌 지점에 도달한 후에야 판단을 시작할 경우 편안하고 안전하게 감속할 수 있는 거리가 부족해질 수 있기 때문에 중요하다.

충돌 영역(Conflict Region)은 궤적이 교차하거나 합류하는 둘 이상의 교통 참여자가 점유할 가능성이 있는 영역이다. 계획기는 다른 객체가 현재 해당 영역을 점유하고 있는지만 평가해서는 안 되며, 로봇이 해당 영역을 통과할 예정인 시간 동안 다른 객체가 그 영역에 진입할 것으로 예측되는지도 평가해야 한다. 따라서 충돌까지의 시간(Time-to-Conflict), 예측 도착 시간(Predicted Arrival Time), 점유 시간 구간(Occupancy Interval), 불확실성 여유(Uncertainty Margin)는 순간적인 거리만 사용하는 것보다 유용한 의사결정 정보를 제공할 수 있다.

정지 제어 교차로(Stop-Controlled Intersection)에서 행동 계획기는 정지, 관측, 우선순위 평가, 진입을 조정해야 한다. 로봇이 정지선에 도달했다고 해서 자동으로 진행할 수 있는 것은 아니다. 시스템은 요구되는 정지 조건이 충족되었는지를 확인하고, 다른 교통 참여자를 평가하며, 충분한 시간적·공간적 간격이 존재하는지를 판단해야 한다. 가림(Occlusion)이나 불완전한 인지로 인해 장면의 불확실성이 유지된다면 계획기는 진입을 결정하는 대신 계속 대기할 수 있다.

비신호 교차로(Unsignalized Intersection)에서는 통행 우선권과 에이전트 상호작용(Agent Interaction)에 대한 명시적인 추론이 필요하다. 차량이나 로봇은 서로 다른 방향에서 서로 다른 속도와 의도를 가지고 접근할 수 있으며, 보행자는 차량 흐름과 독립적으로 횡단보도에 진입할 수 있다. 결정론적 규칙 계층(Deterministic Rule Layer)은 적용 가능한 우선순위 제약조건을 정의하고, 예측(Prediction)은 미래 움직임을 추정할 수 있다. 행동 계획기는 이 두 정보를 결합하여 법적 또는 운용상의 우선권과 다른 참여자가 예상대로 행동할 것이라는 가정을 혼동하지 않도록 해야 한다.

신호 제어 교차로(Signal-Controlled Intersection)는 의사결정 과정에 교통신호 상태(Traffic-Signal State)와 신호 단계 시간(Phase Timing)을 추가한다. 계획기는 로봇이 수행하려는 움직임과 관련 신호를 연결하고, 교차로에 진입하여 완전히 빠져나갈 수 있는 충분한 시간이 남아 있는지를 판단해야 한다. 신호 인지만으로는 충분하지 않으며 정지 차량, 보행자, 막힌 출구(Blocked Exit), 예상하지 못한 장애물도 고려해야 한다. 따라서 진행 허용 신호(Permissive Signal)는 진입을 고려할 수 있도록 허용하지만 충돌 및 점유 검사를 제거하지는 않는다.

가림(Occlusion)은 교차로 주변에서 특히 중요하다. 주차 차량, 건물, 식생(Vegetation), 기반시설 또는 다른 교통 참여자가 접근 중인 객체를 가릴 수 있다. 계획기는 관측되지 않은 영역을 비어 있다고 해석하는 대신 제한된 가시성(Limited Visibility)을 불확실성으로 표현해야 한다. 접근 속도 감소, 단계적 관측(Staged Observation), 보수적인 간격 수락(Conservative Gap Acceptance), 일시 정지를 사용하면 로봇이 충돌 영역에 진입하기 전에 이전까지 보이지 않았던 참여자를 감지할 수 있는 시간을 늘릴 수 있다.

회전교차로(Roundabout)는 차량이 중앙 교통섬(Central Island) 주변을 지속적으로 순환하고 진입하는 에이전트가 기존 교통 흐름에 합류한다는 점에서 다른 상호작용 구조를 가진다. 행동 계획기는 예정된 진입로와 출구를 식별하고, 순환 교통(Circulating Traffic)을 관찰하며, 수용 가능한 간격(Acceptable Gap)을 추정하고, 회전교차로에 합류한 후 적절한 순환 경로를 유지하고 계획된 출구로 빠져나가야 한다. 이러한 행동은 단일한 진입 결정이 아니라 지속적인 경로 및 상호작용 문맥을 요구하는 연속적인 과정이다.

회전교차로에 접근하는 동안 계획기는 순환 중인 차량을 평가하고 차량의 궤적이 로봇의 예정된 진입과 충돌할지를 예측한다. 현재 순간에 기하학적으로 비어 있는 간격이라도 로봇이 양보선(Yield Line)에 도달하기 전에 사용할 수 없게 될 수 있다. 따라서 예측 인지형 간격 평가(Prediction-Aware Gap Assessment)는 상대 속도(Relative Speed), 도착 시간(Arrival Time), 불확실성, 로봇의 가속 능력(Acceleration Capability)을 고려한다. 선택된 진입은 순간적인 여유 공간만을 기준으로 하는 것이 아니라 합류 과정 전체에서 실행 가능해야 한다.

간격 수락(Gap Acceptance)은 교차로와 회전교차로 모두에서 핵심적인 행동 계획 문제이다. 계획기는 로봇이 다른 교통 참여자에게 위험한 반응을 강요하지 않으면서 진입, 횡단, 회전 또는 합류할 수 있을 만큼 충분한 시간적·공간적 간격이 존재하는지를 결정한다. 수락 임계값(Acceptance Threshold)은 로봇 크기, 가속 능력, 적재 상태(Payload), 노면 상태(Surface Condition), 가시성, 위치추정 신뢰도(Localization Confidence), 주변 에이전트의 예측 행동에 따라 달라질 수 있다.

보행자와 자전거는 움직임이 빠르게 변할 수 있고 이동 경로가 횡단 구역 주변에서 차량 궤적과 자주 교차하기 때문에 특별한 주의가 필요하다. 횡단보도 근처에 서 있는 보행자는 계속 정지해 있거나 횡단을 시작할 수 있으며, 자전거는 단순한 거리 측정만으로 예상한 것보다 빠르게 충돌 영역에 접근할 수 있다. 추적 이력(Tracking History), 의도 예측(Intent Prediction), 불확실성 여유, 명시적인 횡단보도 의미정보(Crosswalk Semantics)를 사용하면 계획기가 계속 진행, 감속, 양보 또는 대기 중 적절한 행동을 결정하는 데 도움이 된다.

교차로 행동은 시간적으로 안정적이어야 한다. 지속성(Persistence)과 히스테리시스(Hysteresis)가 없다면 객체 예측의 작은 변화로 인해 계획기가 ENTER와 WAIT 사이를 반복적으로 전환할 수 있다. 양보 또는 정지 결정이 선택되면 명확하게 정의된 해제 조건(Release Condition)을 사용하여 언제 진행을 재개할 수 있는지를 결정해야 한다. 마찬가지로 진입 결정은 지속적으로 모니터링해야 하지만 일반적인 예측 잡음(Prediction Noise)으로 인해 로봇이 안전하게 기동을 시작한 이후 불필요한 행동 진동이 발생해서는 안 된다.

로봇이 횡단 또는 합류를 시작한 이후에는 확정 개념(Commitment Concept)이 유용하다. 확정 이전에는 계획기가 대기, 양보, 진입 중 하나를 선택할 수 있다. 그러나 충돌 영역에 진입한 이후 갑작스럽게 결정을 되돌리는 것은 기동을 완료하는 것보다 더 큰 위험을 발생시킬 수 있다. 따라서 계획기는 특정 지점 또는 상태 이후에는 선호 행동이 간격 선택(Gap Selection)에서 안전한 기동 완료(Safe Completion)로 변경되도록 정의해야 한다. 동시에 독립적인 비상 메커니즘(Emergency Mechanism)은 즉각적인 위험에 계속 대응할 수 있어야 한다.

복잡한 교차로에서는 여러 에이전트가 서로 영향을 미치기 때문에 상호작용 인지형 계획(Interaction-Aware Planning)이 필요할 수 있다. 어떤 차량은 로봇이 양보할 것으로 예상하여 감속할 수 있고, 동시에 로봇은 해당 차량이 계속 진행할 것으로 예측하여 대기할 수 있다. 이러한 상호적인 가정(Reciprocal Assumption)은 불필요한 교착상태(Deadlock)를 발생시킬 수 있다. 예측 인지형(Prediction-Aware), 게임이론적(Game-Theoretic), 탐색 기반(Search-Based) 추론은 대안적인 반응을 평가할 수 있으며, 결정론적 규칙은 통행 우선권, 운용 제약조건, 안전 경계를 유지한다.

교착상태 처리(Deadlock Handling)는 혼합 환경에서 저속으로 운행하는 실외 AMR에 특히 중요하다. 협소한 교차 지점이나 공유 교차로에서 여러 로봇, 차량 또는 보행자가 서로를 기다리면서 무기한 정지할 수 있다. 행동 계획기는 일정 시간 동안 진행이 충분하지 않은 상태를 감지하고, 사전에 정의된 규칙에 따라 우선순위를 유지하거나, 양보하거나, 대체 경로(Alternate Route)를 선택하거나, 검증된 자율 운용 한계(Validated Autonomy Limit) 내에서 상황을 해결할 수 없는 경우 지원 요청(Request Assistance)을 수행하는 통제된 해결 정책을 실행할 수 있다.

궤적 생성(Trajectory Generation)과의 인터페이스는 행동 의도(Behavioral Intent)와 실행 가능한 움직임(Executable Motion)의 차이를 유지해야 한다. 행동 계획기는 YIELD, ENTER, TURN_LEFT, MERGE, CIRCULATE, EXIT와 같은 행동을 선택하고 목표 영역(Target Region), 속도 범위(Speed Range), 필수 안전 여유(Required Clearance), 점유 금지 영역(Prohibited Occupancy) 등의 관련 제약조건을 제공할 수 있다. 이후 궤적 생성기는 로봇의 기하학적 구조와 주변 장애물을 고려하면서 이러한 제약조건을 만족하는 동역학적으로 실행 가능한 경로와 속도 프로파일을 생성한다.

안전 감독(Safety Supervision)은 교차로 및 회전교차로 기동 전체에서 독립적으로 유지된다. 행동 계획기가 진입을 선택한 이후에도 런타임 모니터링(Runtime Monitoring)은 충돌 위험, 정지 능력(Stopping Capability), 위치추정 유효성(Localization Validity), 예상하지 못한 객체 움직임, 운용 경계를 지속적으로 평가해야 한다. 선택된 행동을 뒷받침하던 가정이 더 이상 유효하지 않으면 시스템은 물리적으로 가능한 범위에서 해당 기동을 수정하거나 거부하고 적절한 대체 행동(Fallback Behavior) 또는 최소위험 대응(Minimal-Risk Response)으로 전환해야 한다.

검증(Validation)은 개별적인 정상 상황만이 아니라 여러 시나리오의 조합을 대상으로 해야 한다. 시뮬레이션에서는 접근 속도, 교통 밀도(Traffic Density), 신호 상태, 보행자 움직임, 자전거 움직임, 가림, 예측 오류, 위치추정 불확실성, 도로 형상, 노면 상태를 변화시켜야 한다. 회전교차로 시험에서는 순환 속도(Circulating Speed), 진입 간격(Entry Gap), 다중 출구(Multiple Exit), 차로 점유(Lane Occupancy), 동시 진입 에이전트도 추가로 변화시켜야 한다. 특히 간격 수락의 경계조건과 WAIT에서 ENTER로 전환되는 상태에 주의를 기울여야 한다.

로깅(Logging)은 교차로 유형, 예정된 경로, 관련 교통 제어(Traffic Control), 감지된 교통 참여자, 예측 궤적, 충돌 영역, 수락 또는 거부된 간격, 선택된 행동, 전이 원인(Transition Cause), 안전 개입(Safety Intervention)을 기록해야 한다. 이러한 기록을 통해 로봇이 왜 대기하고, 진입하고, 양보하거나, 기동을 중단했는지를 재구성할 수 있다. 또한 인지, 예측, 매핑 또는 행동 정책(Behavioral Policy)이 업데이트될 때 회귀 시험(Regression Testing)에 재사용할 수 있는 시나리오를 제공한다.

따라서 교차로 및 회전교차로 행동 계획은 지도 의미정보(Map Semantics), 인지, 예측, 통행 우선권 논리(Right-of-Way Logic), 시간적 추론(Temporal Reasoning), 안전 감독을 하나의 조정된 행동 프로세스로 통합한다. 목표는 단순히 기하학적으로 비어 있는 경로를 찾는 것이 아니라 해당 경로를 언제, 그리고 어떤 조건에서 안전하고 일관되게 사용할 수 있는지를 결정하는 것이다. 실외 AMR에서 이러한 기능은 구조화된 환경 및 혼합 교통 환경(Mixed-Traffic Environment)의 예측 인지형 행동 계획과 하위 궤적 생성 사이를 연결하는 중요한 기능을 제공한다.

## 06.08. Emergency Stop and Minimal Risk Condition Behavior

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

비상 정지(Emergency Stop) 및 최소위험상태(Minimal Risk Condition, MRC) 행동은 자율주행 차량 또는 실외 자율이동로봇(Outdoor Autonomous Mobile Robot, AMR)이 정상 운용을 계속하는 것이 더 이상 안전하다고 보장할 수 없는 상황에서 어떻게 대응해야 하는지를 정의한다. 비상 정지는 즉각적인 감속 또는 움직임 종료가 필요한 위험에 대응하고, MRC는 시스템이 합리적으로 가능한 범위에서 위험을 최소화한 후 도달하는 통제된 상태를 의미한다. 이러한 메커니즘은 일반적인 임무 및 행동 의사결정보다 상위에 위치하는 안전 필수 계층(Safety-Critical Layer)을 구성한다.

비상 정지와 MRC의 차이를 구분하는 것은 중요하다. 비상 정지는 주로 임박한 충돌(Imminent Collision)이나 핵심 액추에이터 고장(Critical Actuator Failure)과 같은 즉각적인 위험에 의해 발생하는 신속한 보호 행동(Protective Action)이다. MRC는 위험을 최소화하기 위해 도달한 결과 상태 또는 의도적으로 선택된 운용 상태이다. 환경에 따라 MRC는 현재 위치에서 정지하거나, 더 안전한 장소까지 저속으로 이동하거나, 적재물을 안전하게 고정하거나, 통제된 정지 상태를 유지하는 형태가 될 수 있다.

비상 행동(Emergency Behavior)은 여러 독립적인 정보원에서 발생할 수 있다. 충돌 모니터링(Collision Monitoring)은 불충분한 정지 거리를 감지할 수 있고, 인지(Perception)는 예상하지 못한 장애물을 식별할 수 있으며, 위치추정(Localization)은 신뢰할 수 없는 상태가 될 수 있다. 또한 차량 진단(Vehicle Diagnostics)은 조향, 제동, 추진, 전원 또는 통신 고장을 감지할 수 있다. 외부 비상 정지 장치와 권한이 있는 운영자의 명령도 추가적인 트리거(Trigger)를 제공할 수 있다. 안전 아키텍처는 하나의 인지 또는 계획 구성요소에만 의존하지 않고 이러한 정보원을 결합해야 한다.

위험 심각도(Hazard Severity)와 사용 가능한 대응 시간(Response Time)에 따라 적절한 반응이 결정된다. 충돌이 임박한 경우 시스템에는 검증된 최대 감속(Maximum Validated Deceleration)을 명령하고 정지할 수 있는 시간만 남아 있을 수 있다. 반면 위치추정 신뢰도 저하 또는 부분적인 센서 손실과 같이 성능 저하가 점진적으로 발생하면 로봇은 먼저 속도를 줄이고, 안전 여유(Safety Margin)를 확대하고, 사용할 수 있는 행동을 제한하면서 운용 지속이 허용 가능한지를 평가할 수 있다. 이는 정상 운용에서 성능 저하 상태와 최소위험상태로 점진적으로 전환되는 과정을 형성한다.

정지 행동(Stopping Behavior)은 STOP을 순간적으로 실행되는 논리적 행동으로 취급하는 대신 로봇의 물리적 동역학(Physical Dynamics)을 고려해야 한다. 차량 질량, 적재물(Payload), 속도, 도로 경사(Road Gradient), 타이어-노면 마찰(Tire-Road Friction), 액추에이터 지연(Actuator Delay), 제동 능력(Braking Capability), 노면 상태가 실제 가능한 정지 거리를 결정한다. 저마찰 노면의 내리막길을 주행하는 고하중 실외 AMR은 건조한 평지에서 저속으로 움직이는 무적재 로봇보다 훨씬 긴 정지 거리가 필요할 수 있다. 따라서 안전 의사결정은 동적 정지 능력(Dynamic Stopping Capability)을 반영해야 한다.

유용한 안전 아키텍처(Safety Architecture)는 정상 행동 계획(Nominal Behavior Planning)과 독립적인 안전 감독(Safety Supervision)을 분리한다. 정상 계획기는 CONTINUE, YIELD, AVOID, MERGE 또는 기타 임무 관련 행동을 선택할 수 있으며, 안전 감독기(Safety Supervisor)는 그 결과로 발생하는 움직임이 검증된 안전 한계 내에 유지되는지를 지속적으로 평가한다. 이러한 한계가 위반되면 감독기는 정상 의사결정을 재정의(Override)하고 일반 계획기가 상황을 다시 판단할 때까지 기다리지 않고 통제된 감속, 비상 제동(Emergency Braking), 또는 MRC로의 전환을 요구할 수 있다.

최소위험 행동(Minimal-Risk Behavior)은 문맥 의존적(Context-Dependent)이어야 한다. 창고 통로나 개방된 야드(Open Yard)에서는 즉시 정지하는 것이 적절할 수 있지만, 로봇이 도로 교차로, 철도 건널목(Railway Crossing), 급경사 또는 기타 노출된 충돌 영역에 위치한 경우에는 즉시 정지가 추가적인 위험을 만들 수 있다. 충분한 제어 권한(Control Authority)과 시간이 남아 있다면 시스템은 정지하기 전에 위험 영역을 벗어나야 할 수 있다. 따라서 MRC 전략은 계속 이동할 때의 위험과 최종 정지 위치에서 발생하는 위험을 모두 고려해야 한다.

MRC로의 전환은 NORMAL, DEGRADED, FALLBACK, MINIMAL_RISK_MANEUVER, MRC, EMERGENCY_STOP과 같은 명시적인 안전 상태(Safety State)를 통해 구성할 수 있다. 이러한 상태는 점차 제한되는 행동을 제어하기 위한 결정론적 프레임워크(Deterministic Framework)를 제공한다. 일시적인 센서 잡음으로 인해 불필요한 비상 전환이 발생하지 않으면서 실제 안전 필수 고장은 즉각적인 우선순위를 갖도록 각 상태의 진입 조건(Entry Condition)과 종료 조건(Exit Condition)을 명확하게 정의해야 한다.

완전한 시스템 정지가 필요하지 않은 경우에는 성능 저하 운용(Degraded Operation)이 유용하다. 하나의 중복 센서(Redundant Sensor) 손실, 일시적인 통신 성능 저하, 위치추정 신뢰도 감소, 제한된 환경 가시성(Environmental Visibility)은 보다 엄격한 제약조건 아래에서 운용을 계속할 수 있도록 허용할 수 있다. 로봇은 속도를 낮추고, 안전 여유를 확대하고, 복잡한 기동을 비활성화하고, 운용 영역을 제한하거나, 알려진 안전 위치(Safe Location)로 복귀할 수 있다. 이러한 성능 저하 모드는 해당 능력과 한계가 명시적으로 검증된 경우에만 사용해야 한다.

위치추정 고장(Localization Failure)은 행동 계획과 궤적 계획이 로봇이 도로, 장애물, 운용 경계에 대해 어디에 위치하는지를 아는 것에 의존하기 때문에 특별한 주의가 필요하다. 위치추정 불확실성(Localization Uncertainty)이 정의된 임계값을 초과하면 시스템은 점진적으로 속도를 줄이고 대체 위치추정 소스(Alternative Localization Source)를 이용하여 복구를 시도할 수 있다. 신뢰할 수 있는 위치를 복원할 수 없다면 계획기는 제한되지 않은 내비게이션을 방지하고 적절한 최소위험 대응(Minimal-Risk Response)으로 전환해야 한다.

인지 성능 저하(Perception Degradation)도 유사한 대응을 발생시킬 수 있다. 카메라 가림(Camera Obstruction), LiDAR 고장, 환경 오염(Environmental Contamination), 폭우, 안개, 눈부심(Glare), 어둠 또는 센서 불일치(Sensor Disagreement)는 주변 장면에 대한 신뢰도를 낮출 수 있다. 시스템은 관측되지 않은 영역이 안전하다고 가정하는 대신 불충분한 인지를 증가된 불확실성으로 처리해야 한다. 남아 있는 센싱 능력에 따라 속도 제한, 확대된 정지 여유(Stopping Margin), 센서 중복성(Sensor Redundancy), 대체 인지(Fallback Perception), 최종적인 MRC 전환을 적용할 수 있다.

액추에이터 및 추진계 고장(Actuator and Propulsion Failure)은 원하는 기동을 실행할 수 있는 로봇의 물리적 능력이 감소할 수 있기 때문에 서로 다른 대체 전략(Fallback Strategy)을 요구한다. 조향 성능 저하(Steering Degradation)는 정상적인 경로 추종을 방해할 수 있고, 추진계 고장은 로봇이 교차로나 경사로를 벗어나지 못하게 할 수 있다. 특히 제동 성능 저하(Brake Degradation)는 정지 능력에 직접 영향을 미치므로 매우 중요하다. 따라서 안전 감독기는 남아 있는 제어 권한을 판단하고 사용 가능한 액추에이터로 실제 실행할 수 있는 행동만 선택해야 한다.

통신 손실(Communication Loss)이 모든 AMR 아키텍처에서 동일한 대응을 자동으로 발생시켜서는 안 된다. 자율적인 로컬 운용(Local Autonomous Operation)을 위해 설계된 로봇은 지속적인 네트워크 연결 없이도 제한된 임무를 안전하게 계속할 수 있지만, 원격 감독 시스템(Remotely Supervised System)은 통신이 끊기면 사전에 정의된 대체 상태로 전환해야 할 수 있다. 적절한 행동은 어떤 안전 기능이 로컬에서 계속 사용 가능한지, 그리고 운용 지속을 위해 외부 승인이나 감독이 필요한지에 따라 결정된다.

비상 정지 명령(Emergency-Stop Command)은 감지에서 작동(Actuation)까지 짧고 결정론적인 경로를 가져야 한다. 상위 수준 계획, 클라우드 통신, LLM 추론 또는 계산 비용이 높은 예측에 지나치게 의존하면 허용할 수 없는 지연이 발생할 수 있다. 따라서 안전 필수 트리거(Safety-Critical Trigger)는 제한된 지연시간(Bounded Latency)을 갖는 검증된 통신 경로와 실행 메커니즘을 사용해야 한다. 상위 수준 소프트웨어는 사건을 기록하고 설명하며 조정할 수 있지만, 즉각적인 보호 행동은 필수적이지 않은 추론 구성요소에 의존해서는 안 된다.

비상 제동이 시작된 이후에도 시스템은 환경과 로봇 상태를 계속 모니터링해야 한다. 정지 명령이 내려졌다고 해서 로봇이 물리적으로 정지했다는 것이 보장되지는 않으며, 특히 경사로, 미끄러운 노면 또는 액추에이터 성능 저하 상황에서는 더욱 그렇다. 휠 속도(Wheel Speed), 관성 측정(Inertial Measurement), 위치 변화(Position Change), 브레이크 상태(Brake State) 및 기타 사용 가능한 피드백을 통해 감속이 예상대로 진행되고 있는지를 확인할 수 있다. 예상된 반응을 달성하지 못하면 추가적인 보호 메커니즘으로 대응 수준을 높여야 할 수 있다.

MRC에 도달한다는 것은 차량 속도를 0으로 만드는 것뿐 아니라 전체 로봇 시스템을 안정화(Stabilization)하는 것을 포함해야 한다. 구동 토크(Drive Torque)를 비활성화하거나 통제하고, 브레이크를 유지하며, 매니퓰레이터(Manipulator)를 안전한 상태로 배치하고, 적재물을 고정하며, 위험한 액추에이터의 동작을 차단하고, 경고 표시기(Warning Indicator)를 활성화해야 할 수 있다. 가능한 경우 진단을 수행하기 위한 충분한 전원과 통신을 유지해야 한다. 정확한 안전 상태 구성(Safe-State Configuration)은 로봇 플랫폼과 운용 환경에 따라 달라진다.

비상 상태 또는 MRC로부터의 복구(Recovery)는 해당 상태로 진입하는 것보다 더 신중하게 처리해야 한다. 원래의 트리거가 사라졌다는 이유만으로 로봇이 자동으로 정상 운용을 재개해서는 안 된다. 시스템은 관련 센서, 위치추정, 액추에이터, 통신, 안전 모니터(Safety Monitor), 환경 조건이 허용 가능한 상태로 복원되었는지를 확인해야 한다. 특정 고장은 움직임을 재개하기 전에 운영자 확인(Operator Acknowledgment), 유지보수 점검(Maintenance Inspection), 시스템 리셋(System Reset), 명시적인 승인(Explicit Authorization)을 요구할 수 있다.

대체 운용(Fallback Operation) 중에는 인간-기계 상호작용(Human-Machine Interaction)이 중요하다. 운영자는 트리거, 현재 안전 상태, 로봇 위치, 남아 있는 시스템 능력(Remaining System Capability), 원격 개입(Remote Intervention) 허용 여부를 설명하는 간결한 정보를 받아야 한다. 일반적인 시스템 오류와 같은 모호한 메시지는 운용 측면에서 거의 도움이 되지 않는다. 구조화된 진단 설명(Structured Diagnostic Explanation)을 통해 권한이 있는 운영자는 로봇을 계속 정지 상태로 유지할지, 수동으로 회수할지, 통제된 재시작 절차를 수행할지를 판단할 수 있다.

움직임이 계속 필요한 경우 궤적 생성(Trajectory Generation)은 최소위험 기동(Minimal-Risk Maneuver)을 지원해야 한다. 일반적인 이동 효율을 최적화하는 대신 궤적 생성기는 저속, 큰 안전 여유, 낮은 곡률(Low Curvature), 짧은 정지 거리, 안전 정지 영역(Safe Stopping Region)으로의 접근을 우선할 수 있다. 행동 계획기는 안전 목표와 제약조건을 제공하고, 궤적 생성기는 물리적으로 실행 가능한 대체 경로가 존재하는지를 판단한다. 검증된 대체 궤적이 존재하지 않는다면 정지가 유일하게 허용 가능한 행동으로 유지될 수 있다.

비상 및 MRC 행동은 결정론적 검증(Deterministic Verification)이 가능하도록 설계해야 한다. 각각의 트리거는 정의된 상태 전이, 허용 행동(Permitted Action), 시간 요구사항(Timing Requirement), 복구 조건(Recovery Condition)과 연결되어야 한다. 최대 성능 저하 속도(Maximum Degraded Speed), 최소 정지 여유(Minimum Stopping Margin), 위치추정 불확실성 한계, 대응 기한(Response Deadline)과 같은 안전 관련 파라미터는 구성 관리(Configuration Control)의 대상이 되어야 한다. 작은 파라미터 변경도 안전 행동에 큰 영향을 줄 수 있으므로 이러한 값을 변경하면 회귀 시험(Regression Testing)이 필요하다.

시뮬레이션(Simulation)은 실제 로봇에서 반복적으로 재현하기에는 위험하거나 비용이 높은 고장을 시험하는 효과적인 방법을 제공한다. 시험에서는 갑작스러운 장애물, 센서 손실, 위치추정 드리프트(Localization Drift), 액추에이터 성능 저하, 브레이크 지연(Brake Delay), 통신 중단, 미끄러운 노면, 과도한 적재물, 상충하는 계획기 명령 등을 주입할 수 있다. 평가에서는 감지 지연시간(Detection Latency), 개입 지연시간(Intervention Latency), 정지 거리, 잔여 위험(Residual Risk), 최종 MRC 위치, 안전하지 않은 정상 행동의 성공적인 차단 여부를 측정해야 한다.

시뮬레이션은 제동 마찰, 액추에이터 지연, 타이어 거동, 기계적 고장, 환경 변동성을 완벽하게 재현할 수 없으므로 물리적 검증(Physical Validation)도 필요하다. 통제된 시험장(Proving Ground) 시험을 통해 속도, 적재량, 경사도, 노면 상태에 따른 정지 성능을 검증할 수 있다. 이후 시험 결과를 사용하여 시뮬레이션 모델과 안전 여유를 보정할 수 있다. 검증된 운용 범위(Validated Operational Envelope)는 비상 및 최소위험 전략이 충분한 성능을 제공하는 것으로 확인된 조건을 정의해야 한다.

이벤트 로깅(Event Logging)은 안전 개입 이전의 조건, 트리거 정보원(Triggering Source), 타임스탬프(Timestamp), 로봇 상태, 선택된 대체 행동, 액추에이터 명령, 측정된 반응, 최종 MRC 상태, 복구 시퀀스(Recovery Sequence)를 보존해야 한다. 동기화된 로그(Synchronized Log)를 통해 안전 메커니즘이 올바르게 대응했는지, 그리고 상위 인지, 예측, 계획 또는 하드웨어가 사건 발생에 영향을 미쳤는지를 재구성할 수 있다. 이러한 기록은 회귀 시험을 위한 중요한 시나리오도 제공한다.

따라서 비상 정지 및 최소위험상태 행동은 일반적인 자율 시스템이 더 이상 허용 가능한 운용을 보장할 수 없을 때 작동하는 최종 보호 행동 계층(Final Protective Behavioral Layer)을 제공한다. 정상 계획(Nominal Planning)은 임무 달성을 시도하고, 검증된 성능 저하 모드(Degraded Mode)는 제한된 기능을 유지하며, 대체 행동(Fallback Behavior)은 고장이 발생할 때 위험 노출을 줄이고, 비상 개입(Emergency Intervention)은 즉각적인 위험을 처리한다. 최종적으로 형성된 MRC는 시스템을 진단하고, 복구하거나, 안전하게 정상 운용으로 복귀시킬 수 있는 통제된 상태를 제공한다.

## 06.09. Behavior Planning Simulation Validation Methodology

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

행동 계획 시뮬레이션 검증(Behavior Planning Simulation Validation)은 자율주행 차량 또는 실외 자율이동로봇(Outdoor Autonomous Mobile Robot, AMR)이 정상, 성능 저하, 비정상 및 안전 필수 상황에서 적절한 행동을 선택하는지를 체계적으로 판단하는 방법을 제공한다. 행동 계획(Behavior Planning)은 인지(Perception)와 예측(Prediction)을 궤적 생성(Trajectory Generation) 및 제어(Control)와 연결하므로, 검증에서는 개별 알고리즘이 올바르게 실행되는지만 확인해서는 안 된다. 전체 의사결정 시퀀스(Decision Sequence)가 안전하고 일관되며 설명 가능하고 운용상 허용 가능한지를 판단해야 한다.

검증 과정은 시스템의 의도된 행동(Intended Behavior)을 정의하는 것에서 시작한다. 각 운용 시나리오(Operational Scenario)는 관련 환경 조건, 교통 참여자(Traffic Participant), 로봇 상태, 임무 목표(Mission Objective), 적용 규칙, 예상 행동 제약조건(Behavioral Constraint)을 식별해야 한다. 하나의 정확한 궤적을 지정하기보다는 행동 수준 요구사항(Behavior-Level Requirement)을 통해 CONTINUE, SLOW, YIELD, WAIT, AVOID, REROUTE, STOP 또는 최소위험상태(Minimal-Risk Condition)로의 전환과 같은 허용 가능한 결과를 정의할 수 있다.

시나리오 설계(Scenario Design)는 로봇의 운용 설계 영역(Operational Design Domain, ODD)에서 도출되어야 한다. 도로 구조, 보행자 밀도, 교통 조건, 지형, 날씨, 조도, 통신 가용성, 위치추정 품질(Localization Quality), 운용 속도는 로봇이 동작하도록 설계된 조건을 정의한다. 시뮬레이션 시나리오는 이러한 영역을 체계적으로 샘플링하고 경계까지 확장하여 정상 조건뿐 아니라 운용 한계(Operational Limit) 주변에서도 행동을 평가할 수 있도록 해야 한다.

유용한 시나리오 표현(Scenario Representation)은 기능적(Functional), 논리적(Logical), 구체적(Concrete) 수준을 구분한다. 기능적 시나리오(Functional Scenario)는 로봇 앞을 보행자가 횡단하는 것과 같이 상황을 개념적으로 설명한다. 논리적 시나리오(Logical Scenario)는 보행자 속도, 초기 거리, 로봇 속도, 가시성, 횡단 각도 등의 파라미터 범위를 정의한다. 구체적 시나리오(Concrete Scenario)는 이러한 범위에서 특정 값을 설정한다. 이러한 계층 구조를 사용하면 적은 수의 행동 개념으로 많은 재현 가능한 시뮬레이션 사례를 생성할 수 있다.

많은 행동 실패는 명확한 정상 상황보다 의사결정 경계(Decision Boundary) 근처에서 발생하므로 파라미터 변화(Parameter Variation)가 필수적이다. 장애물이 명확하게 멀리 있거나 명확하게 위험한 경우 계획기가 올바르게 동작하더라도 CONTINUE와 YIELD를 구분하는 임계값 근처에서는 불안정해질 수 있다. 따라서 시뮬레이션은 거리, 상대 속도(Relative Velocity), 충돌까지의 시간(Time-to-Collision), 예측 신뢰도(Prediction Confidence), 위치추정 불확실성(Localization Uncertainty) 등의 파라미터를 전환 임계값 주변에서 변화시켜 행동 진동(Oscillation), 늦은 의사결정 또는 과도한 보수성을 식별해야 한다.

시나리오 조합(Scenario Combinatorics)은 빠르게 증가하기 때문에 모든 경우를 완전하게 시험하는 것은 어렵다. 페어와이즈 또는 조합 샘플링(Pairwise or Combinatorial Sampling), 경계값 분석(Boundary-Value Analysis), 중요도 샘플링(Importance Sampling), 위험 기반 우선순위화(Risk-Based Prioritization), 무작위 생성(Randomized Generation)을 활용하면 의미 있는 커버리지(Coverage)를 유지하면서 탐색 공간을 줄일 수 있다. 발생 확률이 낮더라도 결과의 심각도가 높은 사건에는 추가적인 주의를 기울여야 한다. 따라서 검증 노력은 시나리오 발생 빈도와 잠재적 심각도를 모두 반영해야 한다.

폐루프 시뮬레이션(Closed-Loop Simulation)은 행동 계획 검증에서 특히 중요하다. 개루프 시험(Open-Loop Test)에서는 기록된 센서 입력을 재생하지만 로봇의 의사결정이 환경에 영향을 미치지 못한다. 폐루프 시뮬레이션에서는 선택된 행동이 로봇 궤적을 변화시키고, 이것이 주변 에이전트와 이후 관측에 다시 영향을 준다. 이러한 피드백(Feedback)은 양보, 합류, 교차로 협상(Intersection Negotiation), 보행자 상호작용, 장애물 회피처럼 참여자들이 서로 반응하는 상황을 평가하는 데 필수적이다.

에이전트 행동 모델(Agent Behavior Model)은 단순한 결정론적 궤적 이상의 특성을 표현해야 한다. 보행자, 차량, 자전거, 지게차 및 다른 로봇에는 속도, 공격성(Aggressiveness), 반응시간(Reaction Time), 규칙 준수 정도(Compliance), 의도(Intent)가 서로 다른 여러 행동 모델을 적용할 수 있다. 일부 에이전트는 예상되는 규칙을 따르는 반면 다른 에이전트는 현실적인 범위 안에서 예상하지 못한 행동을 수행할 수 있다. 이러한 다양성은 모든 시뮬레이션 참여자가 계획기에 내재된 가정을 완벽하게 따르기 때문에 검증이 인위적으로 쉬워지는 것을 방지한다.

예측 인지형 행동 계획(Prediction-Aware Behavior Planning)은 예측 자체와 그 결과로 발생하는 의사결정을 모두 검증해야 한다. 궤적 예측 오류(Trajectory Prediction Error)가 반드시 안전하지 않은 행동을 발생시키는 것은 아니며, 수치적으로 정확한 예측이라도 불확실성을 잘못 처리하면 부적절한 의사결정으로 이어질 수 있다. 따라서 검증에서는 예측 성능 지표를 충돌 위험, 불필요한 양보, 기회 상실(Missed Opportunity), 불안정한 행동 전환, 충분한 안전 여유 유지 실패 등의 행동 결과와 연결해야 한다.

고장 주입(Fault Injection)은 정상적인 환경 변화 이상의 시험으로 검증 범위를 확장한다. 시뮬레이션은 카메라 손실, LiDAR 성능 저하, 위치추정 드리프트(Localization Drift), 지연된 객체 추적(Delayed Object Track), 지도 불일치(Map Inconsistency), 통신 손실, 액추에이터 성능 저하(Actuator Degradation), 손상된 임무 정보(Corrupted Mission Information), 타이밍 지연(Timing Delay)을 의도적으로 주입할 수 있다. 목적은 행동 계획기가 유효하지 않은 가정을 기반으로 정상 운용을 계속하는 대신 검증된 성능 저하(Degraded), 대체(Fallback), 비상(Emergency), 최소위험 행동으로 전환하는지를 확인하는 것이다.

행동 계획은 여러 모듈에서 동기화된 정보에 의존하므로 타이밍 고장(Timing Fault)에 특별한 주의가 필요하다. 인지, 예측, 위치추정, 지도, 차량 상태 정보는 서로 다른 주기와 지연시간으로 도착할 수 있다. 시뮬레이션에서는 오래된 메시지(Stale Message), 타임스탬프 오류(Timestamp Error), 처리 지연(Processing Delay), 업데이트 손실(Dropped Update), 비동기 데이터(Asynchronous Data)를 발생시킬 수 있다. 검증에서는 지나치게 오래되거나 일관되지 않은 정보가 안전 필수 의사결정에 영향을 주기 전에 계획기가 이를 감지하는지를 확인해야 한다.

행동의 정확성(Behavioral Correctness)은 여러 지표를 사용하여 측정해야 한다. 안전 관련 지표에는 충돌 발생, 최소 여유 거리(Minimum Clearance), 충돌까지의 시간, 정지 여유(Stopping Margin), 규칙 위반, 최소위험상태로의 성공적인 전환 등이 포함될 수 있다. 운용 지표에는 임무 완료(Mission Completion), 이동 시간, 불필요한 정지, 경로 효율(Route Efficiency), 에너지 소비, 승객 또는 적재물의 승차감 및 안정성, 대체 행동 발생 빈도가 포함될 수 있다. 계산 지연시간(Computational Latency)과 의사결정 주기 안정성(Decision-Cycle Stability)도 기록해야 한다.

하나의 지표만으로는 충분하지 않다. 전혀 움직이지 않는 계획기는 우수한 충돌 통계를 달성할 수 있지만 임무를 수행하지 못하며, 공격적인 계획기는 높은 효율성을 달성하더라도 허용할 수 없는 안전 여유를 가질 수 있다. 따라서 검증에는 필수 안전 제약조건(Mandatory Safety Constraint)과 성능 목표(Performance Objective)를 구분하는 다차원 수용 프레임워크(Multidimensional Acceptance Framework)가 필요하다. 안전 위반은 효율 향상으로 상쇄되어서는 안 된다.

커버리지 지표(Coverage Metric)는 시뮬레이션 캠페인이 실제로 무엇을 시험했는지를 설명해야 한다. 시나리오 커버리지(Scenario Coverage)는 도로 유형, 상호작용 유형, 기상 조건, 객체 조합, 고장 모드를 추적할 수 있다. 파라미터 커버리지(Parameter Coverage)는 샘플링된 범위와 경계 영역을 측정한다. 행동 커버리지(Behavioral Coverage)는 계획기 상태, 상태 전이, 규칙, 대체 모드, 선택된 행동을 기록한다. 코드 커버리지(Code Coverage)는 보조적인 정보를 제공할 수 있지만 높은 소프트웨어 커버리지만으로 충분한 행동 검증이 입증되는 것은 아니다.

상태 전이 커버리지(State-Transition Coverage)는 FSM 및 하이브리드 행동 계획기(Hybrid Behavior Planner)에 특히 유용하다. 검증에서는 각 상태에 진입할 수 있는지만 확인하는 것이 아니라 중요한 상태 전이가 의도된 조건에서 발생하는지도 확인해야 한다. NORMAL-to-DEGRADED, NAVIGATE-to-YIELD, WAIT-to-ENTER, FALLBACK-to-MRC와 같은 전이는 정상 입력, 경계 입력, 상충 입력(Conflicting Input), 유효하지 않은 입력을 사용하여 시험해야 한다. 예상하지 못했거나 도달할 수 없는 전이는 아키텍처 또는 구현상의 결함을 나타낼 수 있다.

규칙 기반 계획기(Rule-Based Planner)는 규칙 충돌 및 우선순위 시험(Conflict and Priority Testing)이 필요하다. 특히 복잡한 교통 또는 혼합 사용 환경(Mixed-Use Environment)에서는 여러 규칙이 동시에 활성화될 수 있다. 시뮬레이션은 임무 진행, 장애물 회피, 보행자 우선권, 위치추정 성능 저하, 비상 조건이 제어 권한을 놓고 경쟁하는 상황을 생성해야 한다. 검증 결과는 결정론적인 우선순위(Deterministic Precedence)가 유지되고 안전 필수 규칙이 낮은 우선순위의 최적화 목표보다 우선하는지를 확인해야 한다.

탐색 기반(Search-Based), 확률 기반(Probabilistic), 학습 지원형(Learning-Assisted) 계획기는 재현성 제어(Repeatability Control)가 필요하다. 랜덤 시드(Random Seed), 모델 버전(Model Version), 추론 설정(Inference Setting), 탐색 예산(Search Budget), 시뮬레이션 초기화(Simulation Initialization), 환경 파라미터를 기록해야 한다. 동일하거나 통계적으로 동등한 시나리오를 반복 실행하면 의사결정 변동성(Decision Variance)을 확인할 수 있다. 비결정론적 행동이 예상되는 경우 동일한 행동 시퀀스를 요구하기보다 검증된 분포와 안전 경계(Safety Bound)를 기반으로 수용 기준을 정의해야 한다.

LLM 지원 행동 계획(LLM-Assisted Behavior Planning)은 추가적인 검증 차원을 도입한다. 시험에서는 모델이 구조화된 장면 정보(Structured Scene Information)에 근거하여 동작하는지, 승인된 행동만 선택하는지, 검색된 운용 규칙을 준수하는지, 모호한 지시를 처리하는지, 검증된 능력 범위를 벗어난 조건을 인식하는지를 확인해야 한다. 환각으로 생성된 객체나 정책(Hallucinated Object or Policy)이 실행 가능한 사실이 되어서는 안 된다. 언어 모델의 출력과 관계없이 결정론적 검증(Deterministic Validation)과 독립적인 안전 감독(Independent Safety Supervision)은 계속 활성화되어야 한다.

시나리오 변형(Scenario Mutation)은 취약한 행동을 발견하는 효율적인 방법을 제공한다. 의미 있는 시나리오가 식별되면 객체 위치, 속도, 타이밍, 가시성, 지도 형상(Map Geometry), 예측 불확실성에 작은 변화를 적용할 수 있다. 사소한 변화가 지나치게 큰 행동 변화를 발생시키면 해당 사례는 민감한 의사결정 경계를 나타낼 수 있다. 따라서 자동화된 변형(Automated Mutation)은 개별 현장 사건이나 수동으로 설계된 시나리오를 더 큰 회귀 시험군(Regression Family)으로 확장할 수 있다.

실제 현장에서 기록된 데이터(Recorded Real-World Data)도 시뮬레이션 워크플로에 포함해야 한다. 어려운 상호작용, 예상하지 못한 정지, 아차사고(Near Miss), 인지 실패, 운영자 개입(Operator Intervention)이 포함된 현장 로그를 시뮬레이션 시나리오로 재구성할 수 있다. 이후 엔지니어는 원래 조건을 변화시켜 해당 행동이 하나의 특정 사건에만 해당하는지 아니면 더 광범위한 취약성을 나타내는지를 판단할 수 있다. 이를 통해 실제 배포 경험과 가상 검증(Virtual Validation) 사이의 피드백 루프가 형성된다.

회귀 시험(Regression Testing)은 한 시나리오에서의 개선이 이전에 검증된 행동을 손상시키지 않도록 보장한다. 계획기, 규칙, 예측 모델, 지도 처리(Map Processing), 파라미터가 크게 변경될 때마다 대표적인 회귀 시험군을 실행해야 한다. 안전 필수 사건과 이전에 발견된 실패 사례는 이 시험군에 영구적으로 유지되어야 한다. 자동화된 비교를 통해 선택된 행동, 타이밍, 안전 여유, 대체 행동 활성화의 변화를 식별할 수 있다.

대규모 시뮬레이션(Large-Scale Simulation)을 사용하면 병렬 컴퓨팅 자원에서 수천 또는 수백만 개의 시나리오 변형을 실행할 수 있다. 이러한 규모는 광범위한 파라미터 공간을 탐색하고 희귀한 조합을 발견하는 데 유용하지만, 시뮬레이션의 양만으로 검증 품질이 보장되는 것은 아니다. 시나리오 관련성(Scenario Relevance), 모델 충실도(Model Fidelity), 파라미터 커버리지, 오라클 품질(Oracle Quality), 실패 분석이 여전히 필수적이다. 대규모 실행은 정교하게 설계된 표적 시험(Targeted Test)을 대체하는 것이 아니라 보완해야 한다.

시뮬레이션 결과는 최종적으로 물리적 검증(Physical Validation)과 연결되어야 한다. 시뮬레이터 모델에는 센싱, 차량 동역학, 마찰, 에이전트 행동, 지연시간, 환경 조건에 대한 근사가 포함되어 있다. 따라서 선택된 시나리오는 소프트웨어 인 더 루프(Software-in-the-Loop, SIL), 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL), 통제된 시험장 시험(Controlled Proving-Ground Testing), 제한적인 실제 환경 운용(Limited Real-World Operation)으로 단계적으로 확장되어야 한다. 시뮬레이션과 실제 행동 사이의 차이는 모델을 재보정하고 안전 여유를 개선하는 데 사용해야 한다.

추적 가능성(Traceability)은 요구사항, 시나리오, 결과, 소프트웨어 버전을 서로 연결한다. 각각의 중요한 행동 요구사항(Behavioral Requirement)은 검증 시나리오와 측정 가능한 수용 기준(Acceptance Criteria)에 연결되어야 한다. 시험 기록에는 시나리오 구성, 시뮬레이터 버전, 계획기 버전, 지도 데이터, 모델 버전, 랜덤 시드, 출력, 성능 지표, 합격/불합격 결과를 보존해야 한다. 이를 통해 엔지니어는 실패를 재현하고 특정 행동 능력을 뒷받침하는 검증 증거를 확인할 수 있다.

따라서 행동 계획 시뮬레이션 검증은 하나의 시험 단계가 아니라 반복적으로 검증 증거를 축적하는 과정(Iterative Evidence-Building Process)이다. 요구사항은 예상 행동을 정의하고, 시나리오 모델은 대표적인 상황을 생성하며, 파라미터 탐색(Parameter Exploration)은 의사결정 경계를 노출하고, 고장 주입은 시스템 복원력(Resilience)을 평가하며, 성능 지표는 결과를 정량화하고, 회귀 시험은 기존 능력을 보존하며, 물리적 시험은 시뮬레이션 가정을 확인한다. 이러한 활동을 결합함으로써 행동 계획기가 의도된 운용 영역에서 예측 가능하게 동작하고 정상 자율 운용이 신뢰할 수 없는 상태가 되었을 때 안전하게 전환할 수 있다는 체계적인 검증 증거를 구축할 수 있다.

## 06.10. Outdoor AMR Warehouse Yard Behavior Planning Case

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

물류창고 야드(Warehouse Yard)에서 운용되는 실외 자율이동로봇(Outdoor Autonomous Mobile Robot, AMR)은 구조화된 물류 규칙(Structured Logistics Rules)과 개방 환경에서 발생하는 동적 상호작용(Dynamic Interaction)을 함께 처리해야 하므로 대표적인 행동 계획(Behavior Planning) 사례를 제공한다. 통제된 통로를 따라 이동하는 실내 AMR과 달리 실외 로봇은 트럭, 지게차, 작업자, 임시 장애물, 상하차 작업, 변화하는 노면 상태, 부분적으로 구조화된 교통 환경을 마주한다. 행동 계획은 지속적인 안전 평가(Safety Assessment)와 임무 진행을 조정해야 한다.

일반적인 임무는 플릿 또는 창고 관리 시스템(Fleet or Warehouse Management System)이 저장 건물, 상하차 도크(Loading Dock), 대기 구역(Staging Area), 충전소(Charging Station), 컨테이너 구역(Container Zone) 등의 위치 사이에 운송 작업을 할당하면서 시작된다. 임무 계층(Mission Layer)은 목적지와 운용 제약조건을 지정하고, 행동 계획기는 경로를 따라 이동하면서 발생하는 상황에 로봇이 어떻게 대응해야 하는지를 결정한다. 따라서 경로 추종(Route Following)은 단순한 웨이포인트 추종(Waypoint Tracking)이 아니라 문맥 의존적 행동(Context-Dependent Behavior)의 연속 과정이 된다.

물류창고 야드는 서로 다른 영역을 어떻게 사용해야 하는지를 나타내는 의미론적 구역(Semantic Zone)을 이용하여 표현할 수 있다. 주행 차로(Travel Lane), 보행자 횡단구역(Pedestrian Crossing), 상하차 구역(Loading Zone), 제한 구역(Restricted Region), 주차 구역(Parking Area), 교차로(Intersection), 도킹 위치(Docking Location), 속도 제한 구간(Speed-Limited Section)은 단순한 기하학적 자유 공간(Geometric Free Space) 이상의 행동 문맥을 제공한다. 지도 의미정보(Map Semantics)를 통해 계획기는 기하학적으로 유사한 두 영역이라도 운용 목적과 접근 규칙이 다르면 서로 다른 행동이 필요하다는 것을 이해할 수 있다.

정상 주행 중 계획기는 전방 경로를 지속적으로 평가하면서 NAVIGATE 또는 CRUISE 행동을 유지할 수 있다. 목표 속도(Target Speed)는 지역 속도 제한, 정지 능력(Stopping Capability), 적재물(Payload), 노면 상태(Road Surface), 가시성(Visibility), 주변 교통 상황에 의해 제한된다. 환경이 안정적으로 유지될 때 계획기는 불필요한 상태 전환을 피해야 하지만, 다른 차량, 작업자, 장애물 또는 운용 이벤트가 상황을 변화시키면 신속하게 상태를 전환할 수 있도록 충분한 상황 인식(Situation Awareness)을 유지해야 한다.

보행자 상호작용(Pedestrian Interaction)은 물류창고 야드에서 가장 중요한 사례 중 하나이다. 작업자는 지정된 보행로를 횡단하거나, 주차된 트럭 뒤에서 갑자기 나타나거나, 컨테이너 사이를 이동하거나, 상하차 작업에 집중하면서 차량 주행 차로로 진입할 수 있다. 계획기는 보행자 감지, 추적(Tracking), 예측 움직임(Predicted Motion), 횡단보도 의미정보(Crosswalk Semantics), 가시성, 정지 거리를 결합해야 한다. 위험 수준에 따라 로봇은 보수적인 불확실성 여유(Uncertainty Margin)를 유지하면서 CONTINUE, SLOW, YIELD, WAIT 또는 STOP을 선택할 수 있다.

지게차(Forklift)는 빠르게 가속하거나 후진하고, 급격하게 회전하며, 가시성이 제한된 상하차 구역 주변에서 운용될 수 있기 때문에 다른 형태의 상호작용 패턴을 만든다. 또한 포크(Fork)와 운반 중인 적재물은 실제 점유 공간(Occupied Space)을 변화시킬 수 있다. 따라서 AMR은 지게차를 단순한 점 장애물(Point Obstacle)로 취급해서는 안 된다. 추적 이력, 방향(Orientation), 운동 상태(Motion State), 운용 구역, 예측 점유 영역(Predicted Swept Area)을 활용하면 활성화된 자재 취급 작업 주변에서 양보, 추월, 대기 또는 우회 여부를 보다 신뢰성 있게 결정할 수 있다.

대형 트럭은 상당한 가림(Occlusion)과 기동 불확실성(Maneuvering Uncertainty)을 발생시킨다. 주차된 트레일러(Trailer)는 보행자나 소형 차량을 가릴 수 있으며, 후진하는 트럭은 도킹 과정에서 여러 차로를 점유할 수 있다. 가려진 영역에 접근할 때 계획기는 보이지 않는 공간이 비어 있다고 가정하지 않고 속도를 낮추며 정지 여유(Stopping Margin)를 확대해야 한다. 트럭이 능동적으로 기동하고 있다면 AMR은 임시 배제 영역(Temporary Exclusion Region)을 설정하고 예측된 충돌 가능성이 사라질 때까지 대기할 수 있다.

물류 야드 내부의 교차로는 일반적인 교통신호 없이 운영되는 경우가 많다. 대신 현장 규칙(Site Rule), 표시된 정지선(Stop Line), 주도로 우선권(Main-Lane Priority), 차량 유형 또는 운용 절차에 따라 우선순위가 정의될 수 있다. 행동 계획기는 이러한 결정론적 규칙(Deterministic Rule)을 실제 교통에 대한 인지 및 예측과 결합해야 한다. AMR이 명목상의 우선권을 가지고 있더라도 다른 차량이 예상된 우선순위를 위반하거나 잘못 이해할 것으로 예측되면 충돌 영역(Conflict Region)에 진입해서는 안 된다.

상하차 도크(Loading Dock)와 대기 구역(Staging Area)에서는 임무 상태(Mission State)와 밀접하게 조정되는 행동이 필요하다. 로봇은 장비 상태와 운용 승인(Operational Authorization)에 따라 APPROACH, ALIGN, WAIT_FOR_CLEARANCE, DOCK, LOAD, UNDOCK, DEPART를 수행해야 할 수 있다. 기하학적으로 접근 가능한 도크라도 작업자, 지게차, 팔레트, 차량 또는 작동 중인 상하차 장비가 필요한 기동 영역을 점유하고 있다면 사용 가능한 것으로 판단해서는 안 된다. 따라서 운용 준비 상태(Operational Readiness)는 행동 선택의 일부가 된다.

팔레트, 컨테이너, 안전 콘(Safety Cone), 유지보수 장비, 고장 차량, 건설 자재 등은 정적 지도에 표현되지 않은 위치에 나타날 수 있으므로 임시 장애물(Temporary Obstacle)이 흔하게 발생한다. 계획기는 먼저 국부 회피(Local Avoidance)를 이용하여 장애물을 안전하게 통과할 수 있는지를 판단해야 한다. 경로가 실질적으로 차단된 경우 동일한 실행 불가능 경로를 반복적으로 시도하는 대신 WAIT, 국부 재계획(Local Replan) 요청, 대체 경로(Alternate Route) 선택 또는 플릿 관리 시스템에 장애물 보고 등의 행동을 수행할 수 있다.

물류창고 야드에는 두 차량이 동시에 안전하게 통과하기 어려운 협소 통로(Narrow Passage)도 존재한다. 행동 계획기는 진입하기 전에 통로 폭, 차량 크기, 진행 방향, 우선순위 규칙, 사용 가능한 대기 공간(Waiting Space)을 평가할 수 있다. 한 참여자가 제한된 영역(Constrained Region)에 진입하기로 확정하면 다른 참여자는 양보해야 할 수 있다. 명시적인 확정 조건(Commitment Condition)과 해제 조건(Release Condition)을 사용하면 두 차량이 반복적으로 전진과 정지를 반복하거나 교착상태(Deadlock)에 빠지는 것을 방지할 수 있다.

적재 상태(Payload Condition)는 실외 AMR의 행동에 영향을 준다. 무거운 화물을 적재한 로봇은 더 긴 정지 거리, 낮은 가속 성능, 서로 다른 안정성 한계(Stability Limit), 경사로나 회전에서 더욱 엄격한 제약조건을 가질 수 있다. 따라서 행동 계획은 차량 동역학이 항상 동일하다고 가정하지 않고 적재 상태 정보를 입력받아야 한다. 이를 통해 속도 선택, 간격 수락(Gap Acceptance), 추종 거리(Following Distance), 경사 주행, 회전 행동, 비상 정지 여유(Emergency Stopping Margin)를 현재 로봇의 물리적 구성에 맞게 조정할 수 있다.

노면 및 기상 조건(Surface and Weather Condition)은 이러한 제약조건을 더욱 변화시킬 수 있다. 비, 눈, 결빙, 고인 물, 자갈, 손상된 포장도로, 경사로, 불규칙한 노면은 접지력(Traction)을 감소시키거나 위치추정 및 인지 불확실성을 증가시킬 수 있다. 계획기는 속도를 줄이고, 추종 및 정지 여유를 확대하며, 특정 영역을 회피하거나, 검증된 성능 저하 모드(Validated Degraded Mode)를 선택할 수 있다. 환경 적응(Environmental Adaptation)은 통제되지 않은 즉흥적 대응에 의존하지 않고 사전에 정의된 운용 한계 내에서 수행되어야 한다.

위치추정 품질(Localization Quality)은 개방된 야드와 창고, 컨테이너 또는 대형 차량으로 둘러싸인 장소 사이에서 달라질 수 있다. GNSS 기반 위치추정은 다중경로(Multipath)나 신호 차단으로 인해 성능이 저하될 수 있으며, LiDAR, 카메라 또는 지도 기반 위치추정(Map-Based Localization)은 보완적인 정보를 제공할 수 있다. 위치추정 불확실성이 증가하면 계획기는 행동의 공격성(Behavioral Aggressiveness)을 낮추고 정밀 위치에 의존하는 기동을 제한해야 한다. 신뢰할 수 있는 위치추정이 지속적으로 손실되면 결국 대체 행동(Fallback Behavior) 또는 최소위험 행동(Minimal-Risk Behavior)을 실행해야 한다.

플릿 관리(Fleet Management)와의 통신은 중요하지만 즉각적인 충돌 안전(Immediate Collision Safety)과는 분리되어야 한다. 플릿 시스템은 임무 할당, 공유 자원 예약(Shared Resource Reservation), 교통 조정, 차단 경로 보고, 충전 또는 도킹 대기열 관리를 수행할 수 있다. 그러나 네트워크 지연(Network Latency)이나 일시적인 통신 손실이 발생하더라도 로컬 행동 계획(Local Behavior Planning)은 보행자, 차량, 장애물에 대응할 수 있어야 한다. 안전 필수 반응(Safety-Critical Reaction)은 로봇 수준에서 계속 사용 가능해야 한다.

다중 로봇 운용(Multi-Robot Operation)은 자원 및 교통 조정 문제를 추가한다. 여러 AMR이 동일한 교차로, 협소 차로, 충전기 또는 상하차 스테이션을 동시에 요청할 수 있다. 플릿 수준 스케줄링(Fleet-Level Scheduling)은 우선순위와 예약을 할당할 수 있으며, 온보드 행동 계획(Onboard Behavior Planning)은 해당 예약을 실행하는 것이 로컬 환경에서 안전한지를 검증한다. 예약이 존재한다는 이유로 로봇이 점유되었거나 위험한 영역에 진입해서는 안 되며, 로컬 안전 개입(Local Safety Intervention)은 조정 명령을 재정의할 수 있어야 한다.

대표적인 임무에서 AMR은 창고를 출발하여 보행자 구역을 횡단하고, 공유 도로를 이동하며, 지게차에 양보하고, 시야를 가리는 트럭을 통과한 뒤, 교차로를 협상하고, 대기 구역에 진입하여 상하차 지점에 도킹해야 할 수 있다. 각 구간에서는 서로 다른 행동 규칙(Behavioral Rule)이 활성화되지만 동일한 임무 목표는 유지된다. 이는 행동 계획이 상위 수준 물류 오케스트레이션(High-Level Logistics Orchestration)과 하위 수준 궤적 생성 사이에서 중간 추론 계층(Intermediate Reasoning Layer)의 역할을 수행하는 이유를 보여준다.

계획기는 하이브리드 아키텍처(Hybrid Architecture)를 이용하여 이러한 사례를 구현할 수 있다. 유한상태기계(Finite-State Machine, FSM)는 지속적인 임무 및 기동 상태를 제공하고, 규칙 엔진(Rule Engine)은 현장 정책과 안전 우선순위를 적용하며, 예측은 동적 참여자를 평가하고, 탐색 또는 최적화(Search or Optimization)는 선택된 상호작용 상황을 해결한다. 학습 지원형(Learning-Assisted) 또는 LLM 지원형(LLM-Assisted) 구성요소는 의미론적 해석이나 상위 수준 제안을 제공할 수 있지만 실행 가능한 행동은 검증된 행동과 독립적인 안전 감독(Independent Safety Supervision)에 의해 제한되어야 한다.

궤적 생성(Trajectory Generation)과의 인터페이스는 행동 의사결정을 움직임 제약조건(Motion Constraint)으로 변환한다. YIELD 결정은 정지 영역(Stop Region)과 최대 접근 속도를 지정할 수 있으며, PASS는 안전 여유 요구사항(Clearance Requirement)과 허용 통로(Permitted Corridor)를 정의할 수 있다. DOCK은 목표 자세(Target Pose)와 정렬 제약조건(Alignment Constraint)을 제공하고, REROUTE는 다른 경로 구간을 요청한다. 이러한 분리를 통해 행동 계획은 조향, 가속 또는 개별 휠 명령을 직접 제어하지 않고 행동 의도(Behavioral Intent)를 표현할 수 있다.

독립적인 안전 감독은 명령된 행동이 실행 가능하고 안전한지를 지속적으로 확인한다. 최소 여유 거리(Minimum Clearance), 예측 충돌 위험(Predicted Collision Risk), 정지 능력, 위치추정 유효성(Localization Validity), 액추에이터 상태(Actuator Condition), 속도 제한, 운용 경계를 실행 중 지속적으로 모니터링할 수 있다. 선택된 행동을 뒷받침하던 가정이 유효하지 않게 되면 안전 감독기는 속도를 낮추고, 로봇을 정지시키고, 기동을 거부하거나, 검증된 대체 행동 및 최소위험상태(Minimal Risk Condition, MRC)를 시작할 수 있다.

시뮬레이션 검증(Simulation Validation)은 물류창고 야드의 다양한 운용 조건을 재현해야 한다. 시나리오에서는 보행자의 갑작스러운 출현, 지게차 움직임, 트럭 후진, 차단된 차로, 상하차 작업, 협소 통로 조우, 적재량, 노면 마찰(Surface Friction), 기상 조건, 위치추정 성능 저하, 통신 손실, 다중 로봇 혼잡(Multi-Robot Congestion)을 변화시킬 수 있다. 특히 정지, 양보, 간격 수락, 기동 확정(Commitment) 임계값 주변의 파라미터 스윕(Parameter Sweep)은 불안정하거나 지나치게 공격적인 행동을 식별하는 데 유용하다.

다른 참여자가 AMR의 행동에 반응할 수 있기 때문에 폐루프 시뮬레이션(Closed-Loop Simulation)이 중요하다. 지게차 운전자는 로봇이 양보하면 속도를 줄일 수 있고, 다른 AMR은 예약을 감지한 후 대기할 수 있으며, 차량은 예상과 달리 교차로를 계속 통과할 수도 있다. 따라서 에이전트 행동 모델(Agent Behavior Model)은 서로 다른 반응시간과 규칙 준수 수준(Compliance Level)을 포함해야 한다. 또한 고장 주입(Fault Injection)을 통해 인지, 위치추정, 통신 또는 액추에이터 성능 저하가 의도된 대체 대응을 발생시키는지 검증할 수 있다.

운용 지표(Operational Metric)는 충돌 회피만을 측정해서는 안 된다. 임무 완료율(Mission Completion Rate), 이동 시간, 대기 시간, 불필요한 정지, 최소 여유 거리, 정지 여유, 규칙 준수(Rule Compliance), 교착상태 발생 빈도, 우회 빈도(Rerouting Frequency), 에너지 소비, 대체 행동 활성화(Fallback Activation), 도킹 성공률(Docking Success)을 사용하면 보다 폭넓게 시스템을 평가할 수 있다. 안전 지표(Safety Metric)는 필수 제약조건으로 유지되어야 하며, 효율성 지표(Efficiency Metric)는 이러한 제약조건 안에서 시스템이 실제로 유용한 물류 작업을 수행할 수 있는지를 평가한다.

현장 운용(Field Operation)에서 얻어진 새로운 검증 증거는 지속적으로 검증 과정에 다시 반영되어야 한다. 예상하지 못한 정지, 아차사고(Near Miss), 차단 경로, 운영자 개입(Operator Intervention), 비정상적인 지게차 행동, 위치추정 문제, 어려운 도킹 상황을 시뮬레이션에서 재구성하고 시나리오 변형(Scenario Mutation)을 통해 확장할 수 있다. 문제가 수정된 이후에는 이러한 사례를 영구적인 회귀 시험(Regression Test)으로 추가함으로써 실제 물류창고 운용 경험이 연속적인 소프트웨어 릴리스에 걸쳐 행동 계획기를 강화하도록 할 수 있다.

물류창고 야드 사례는 실외 AMR 행동 계획이 임무 문맥(Mission Context), 의미론적 지도(Semantic Map), 인지, 예측, 차량 상태, 현장 규칙, 환경 조건, 플릿 조정(Fleet Coordination), 독립적인 안전 감독을 어떻게 통합하는지를 보여준다. 계획기는 이러한 입력을 안정적인 행동 의도(Stable Behavioral Intent)로 변환하고, 궤적 생성기는 실제 실행 가능한 움직임을 결정한다. 이러한 계층적 접근 방식(Layered Approach)을 통해 실외 AMR은 실제 물류창고 야드의 동적이고 부분적으로 구조화된 환경에 안전하게 적응하면서 생산적인 물류 임무를 수행할 수 있다.
