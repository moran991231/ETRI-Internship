2022-07-11 Mon ~
# Abstract
internet AI 시대에서 embodied AI 로의 패러다임 변화가 일어났다.
전문적으로 감별된 큰 데이터셋으로부터 학습하는것 대신에, 정말 인간처럼 자기중심적인 관점으로 환경에서 상호작용을 통해 학습한다.
이런 embodied AI는 Artificial General Intelligence을 추구한다.
이 논문은 시뮬레이터와 관련해서 embodied AI 분야의 백과사전이다.
9개의 embodied AI 시뮬레이터를 7개의 특징에 따라 평가한다.  
9개의 시뮬레이터(후술)  


embodied AI의 3대 과제  
* Visual Exploration
* Visual Navagation
* Embodied Question Answering (QA)
위 3대 과제에 대한 최신 접근법과 평가방법, 데이터셋에 대해 다룬다.
그리고 연구 목적에 부합하는 시뮬레이터를 고를 수 있도록 돕는다.


# I. Introduction
인터넷 ai에서 임바디드 ai로의 패러다임 전환.
general-purpose ai system을 만들고 싶다.
embodied ai는 환경과의 상호작용을 통해서 지능을 얻는다.
아직은 기존 지능의 개념 (비전, 언어, 추론)을 통합하는 것에 지나지않는다.  
그래도 embodied ai에 대한 욕구가 계속 자라서 이를 위한 시뮬레이터가 꽤 발달했다. 물리적으로 현실세계를 곧잘 반영한다.
현실 세계에 적용하기 전에 시뮬레이션 해볼 수 있고, 
task-based dataset으로 시뮬레이션하면 현실에서는 귀찮고 인력이 많이 드는 테스트도 가상공간에서 할 수 있다.
이런 분야에 대해 연구는 있었지만 나온 지 오래 되었다.  

* 9개의 시뮬레이터
    * DeepMind Lab
    * AI2-THOR
    * CHALET
    * VirtualHome
    * VRKitchen
    * HabitatSim
    * iGibson
    * SAPIEN
    * ThreeDWorld

위 9개의 시뮬레이터는 물리엔진, Python API, 인공 agent를 포함한다.

Visual Exploration (이하 VE)는 Visual Navigation (이하 VN)에 포함될 수 있다. 
embodied QA는 (비전-언어 navigation 능력을 갖춘)복합적인 QA모듈을 필요로 한다.
> QA: 언어를 활용하는데 visual QA분야가 인공지능 분야에서 꽤 인기있으므로  embodied AI가 embodied QA를 지향하는 것은 자연스럽다.


* 일차 7개의 특징
    * Environment
    * Physics
    * Object type
    * Object Property
    * Controller
    * Action
    * Multiple agents

* 이차 특징 3요소
    * Realism: Environment, Physics
    * Scalability: Object type
    * Interactivity: Object property, Controller, Action, Multiple agents


# II. Simulators for Embodied AI
9개 시뮬레이터 벤치마크

## A. Embodied AI Simulator
7개의 특징
### 1) Environment
* game-based scene  
3d asset으로 구성됨. 3D 애셋은 물리적 특징이 잘 구현되어있고, 객체 분리/분류도 잘 되어 있다.
연결된 객체간의 움직일 수 있는 관절같은 것을 쉽게 모델링할 수 있다. (ex. PartNet의 3D모델)
* world-based scene  
현실세계 스캔으로 구성됨. 3D메쉬는 현실세계 반영도가 높다. 에이전트의 행동을 시뮬레이션으로부터 현실세계로 옮기기에 용이하다. 용량이 커서 많은 자원이 필요하다.

### 2) Physics
* basic physics features  
충돌(collision), 강체 역학, 중력 모델링.
보통 game-based scene들이 basic physics feature를 가진다.
상호작용하는 navigation-based task를 다루는 시뮬레이터에서는 보통 basic도 충분하다.
* advanced physics features  
천, 유체, 연체 역학.
복잡한 물리환경이 인공 에이전트의 결정을 어떻게 수립하는지를 이해하기 위한 ThreeDWorld같은 시뮬레이터는 advanced physcs featues를 가진다.

### 3) Object type
* dataset driven environment  
모으기 어렵고 비싸지만 퀄리티가 어느정도 보장된다. 기존 3D 객체 데이터셋을 import한다.
* asset driven environment
아무나 올릴 수 있어서 얻기 쉽지만 퀄리티를 보장하기 어렵다. game-based env는 보통 애셋을 사용하는데 애셋 스토어에서 모델을 가져온다.

### 4) Object Property
* interact-able objects  
충돌과 같은 물체와의 상호작용만 가능
* multiple-state objects  
물체가 어떻게 반응하고 그들의 상태를 바꾸는지 안다. 예를들어 사과를 쪼개면 사과의 상태가 변한다.

### 5) Controller
유저와 시뮬레이터 사이의 제어 인터페이스
* direct Python API Controller  
* virtual robot controller  
로보틱스 embodiment는 ROS 인터페이스처럼 현실 세계의 로봇(ex. 터틀봇)과 가상 상호작용을 가능케 한다.  
iGibson과 AI2-THOR는 자회사의 로봇에 대한 가상로봇 컨트롤러르 탑재한 시뮬레이터이다.
* virtual reality controller  
더 몰입적인 인간-컴퓨터 상호작용을 제공하며 실세계 물건들을 배치할 수 있다.

### 6) Action
인공 에이전트의 행동 능력은 단순한 탐색부터 고지능 인간-컴퓨터 액션까지 다양하다.
* navigation  
제일 낮은 티어. 가장 기본적인 embodied AI 시뮬레이터의 특징. 가상 공간을 탐색할 수 있는 능력
* atomic action  
인공 에이전트가 객체애 대한 간단한 행동들을 수행하며 많은 시뮬레이터가 이정도는 지원한다.
* human-computer interaction
vr controller의 결과이다. 사람이 가상 에이전트가 학습하고 상호작용하는 것을 실시간으로 제어할 수 있다.  

대규모 navigation-based 시뮬레이터들은 navigation, atomic action과 더불어 ROS를 지원한다. (ex. AI2_THOR, iGibson, Habitat-Sim)

### 7) Multiple agents
멀티 에이전트를 지원하는 시뮬레이터가 드물다. AI2-THOR, iGibson, ThreeDWorld가 지원한다.
어려운 주제라서 연구가 많이 없다.  
* avatar-based  
ThreeDWorld가 그 예시이다. 인공 에이전트와 시뮬레이션 아바타가 상호작용한다.
* user-based  
AI2-THOR가 그 예이다. 2개의 학습 네트워크의 역할이 주어지고 서로 다른 인공 에이전트가 시뮬레이션에서 일반적인 작업을 수행하기 위해 상호작용한다.



## B. Comparison of Embodied AI Simulators
* 시뮬레이터를 평가할 두 번째 특징 세 가지
    * Realism: Environment, Physics  
    환경은 현실 세계의 물리적 외관을 모델링하는데, 물리 (엔진)은 현실세계의 복잡한 물리적 성질을 모델링한다.
    * Scalability: Object type  
    실세계의 3D 스캔 데이터셋을 모으거나 3D 애셋을 사면서 확장할 수 있다.
    * Interactivity: Object property, Controller, Action, Multiple agents  
    위 특징 세 가지를 모두 가진 시뮬레이터 (AI-THOR, iGibson, Habitat-Sim)은 다양한 embodied AI 연구작업에 활발히 사용된다.  


### 표 II.
평가요소들: Environment configuration, technical specification, rendering performance, simulation engine  
환경설정은 시뮬레이터 창작자가 만든 애플리케이션에 의존도가 높다. technical specification과 rendering performance은 시뮬레이션 엔진에 더 많은 영향을 받는다.


# III. Research in Embodied AI
3대 과제 VE, VN, eQA의 최신 접근법, 평가법, 데이터셋에 대해서 다룬다.  
embodied AI 연구가 최근에 많아진 이유: 인식과학과 심리적 관섬에서 임바디드 가설은 지능은 환경과의 상호작용으로 성장하고 결과적으로 감각운동적인 활동도 그렇게 성장한다고 제안한다.
직관적으로 사람은 (전문가가 엄선하고 대부분 무작위적이고 수동적인) 인터넷 AI로 혼자 배우는게 아니라 능동적인 인식, 움직임, 상호작용과 소통으로 학습한다. 

eAI의 연구는 미지의 환경에 대한 매핑, 탐색, 센서 노이즈에 대한 내성과 같은 더 진보한 로보틱 기능: 일반화를 가져올것이다. 미지의 환경에 대한 일반화와 관련된 학습으로 로봇의 기능이 더 향상된다.
eAI는 유연성과 성능이 더 좋다. 깊이?, 언어 그리고 오디오 같은 다양한 양식이 학습-기반 접근법을 통해 쉽게 통합될 수 있기 때문이다.

eAI과제 유형 3가지 (VE, VN, eQA) 

## A. Visual Exploration
일반적으로 에이전트가 모션과 인식을 통해서 3D 환경에 대한 정보를 모으고 환경에 대한 내부 모델을 갱신한다. 이것은 다음 단계인 visual navigation에 도움이 될 수 있다. VE를 가능한 효율적으로 하는 것이 목표이다. VE 모델의 형태는 topological graph map, sementic map, occupancy map, spatial memory 등 이다.

시각 exploration은  navigation (1)전에 하거나 (2) 동시에 이루어진다. 

* VE가 VN 전에 이루어지면 유용한 경로 계획에 필요한 내부 메모리가 미리 빌드된다. 에이전트는 navigate하기 전에는 한정된 예산 안에서 자유롭게 돌아다닐 수 있다.
* VE와 VN이 동시에 이루어지면 에이전트가 처음보는 테스트 환경을 탐사할 때 맵을 빌드해서 다음작업?(VE+VN?)과 더욱 통합된다.

전통적인 로보틱스 맵을 빌드하기 위해서 exploration은 수동/능동적이고 동시 위치결정 매핑 (SLAM)을 통해서만 이루어졌다. SLAM 연구는 잘 진행되었으나, 순수하게 기하학적 접근은 개선의 여지가 있다.(부족하다.) 센서에 의존하므로 측정노이즈에 민감하고 광범위한 파인튜닝이 필요하다.  
반대로 학습 기반 접근법(RGB and/or 깊이 센서 사용)은 노에즈에 더 강력하다. 그리고 인공 에이전트가 의미적인 이해하고 지식을 일반화한다. 사람 작업에 대한 의존이 줄어서 효율적이다!
내부 모델을 잘 만드들어야 에이전트의 성능이 개선된다.

### 1) Approaches
일반적으로 non-baseline 접근법은 전형적으로 observed Markov 결정 프로세스(POMDPs)로 정형화된다. 이런 접근법은 POMDPs 보상 펑션으로 볼 수 있다.

#### a) Baselines
몇 가지 baseline이 있다.
* random-action: 에이전트 샘플이 모든 액션에 대해 균일한 분포를 가진다.
* forward-action: 앞으로 가는 액션을 선택한다. forward-action+에서는 에이전트는 앞으로 가다가 부딪히면 왼쪽으로 돈다.
* frontier-exploration: free 스페이스와 탐험되지 않은 공간 사이의 edge 방문을 반복한다.
#### b) Curiosity
에이전트는 예측하기 힘든 상태를 탐색한다. 예측 에러가 강화 학습의 보상 시그널로 사용된다. 에이전트 외부의 보상(환경으로부터의 보상)보다는 내재적인 보상(호기심 해결!)에 집중한다. 외부 보상이 거의 없는 경우 활용하기 좋다.

* forward-dynamics model  
Loss: L(s'\_(t+1), s\_(t+1))을 최소화함.  
s'\_(t+1): 에이전트가 상태 s\_t에서 액션 a\_t를 취했을 때 예상된 다음 상태  
s_(t+1): 실제 다음 상태  
Curiosity 접근법을 실제로적용하려면 Proximal Policy Optimization(PPO) 등을 고려해야함[79]  
최근 연구에서 의미적인 지도를 그리기 위해 사용되는 추세  
forward-dynamics 모델은 높은 예측 에러(= 큰 보상)의 확률성을 사용하므로 큰 문제가 된다. (noisy-TV 문제나 에이전트의 액션에 따르는 노이즈 때문에 발생한다.)  

* inverse-dynamics model  
이전상태 s\_(t-1)에서 현재 상태 s\_(t)로 오기 위한 에이전트가 취한 행동 a\_(t-1)을 추정해서 에이전트는 어떤 행위로 환경을 통제할 수 있는지 이해할수 있다. 이런 방식은 환경때문에 확률성을 다루려고 하는데 에이전트의 행위에 의해 결과가 발생하는 확률성을 다루는 것은 불충분하다. 예를들어 에이전트가 랜덤으로 채널을 변경하는 티비 리모콘을 사용하면, 특별한 진전 없이 보상만 쌓인다. (원하는 채널을 못 트는데 계속 보상(=계속 틀림)만 챙긴다는 뜻인것 같음)  

더 어려운 문제들을 해결하기 위해 최근에 제시된 방법들이 있다.
* Random Distillation Network [82]  
랜덤하게 초기화된 신경망의 출력을 예측하는 것인데, 신경망은 입력에 대해 deterministic한 함수이다. (확률을 가지고 랜덤한 함수가 아님)  

* Exploration by Disagreement [81]  
에이전트가  `forward-dynamics models` 앙상블의 예측들 사이에 최대 불일치 혹은 분산을 갖는 행동공간을 탐색하도록 장려되는 불일치에 의한 탐색이다.
모델은 평균에 수렴하게 되는데, 모델은 조합(ensemble)의 분산을 줄이고, 확률성 트랩에 갇혀버리는 것을 예방한다. 


#### c) Coverage
에이전트가 직접적으로 관찰하는 타겟의 수를 최대화하도록 노력한다.  
에이전트는 자기중심적인 관찰(제한된 시야? 관점?)을 하므로 3D장애물에 기반해서 탐색해야 한다.  
* 고전 기법과 `학습-기반 기법`을 합쳐 만든 기법이 있다. [44]  
이것은 학습된 SLAM 모듈로 경로 플래너를 분석한다. 이 모듈은 학습의 end-to-end 정책에 수반된 높은 샘플 복잡성를 피하기 위해 공간 지도를 유지한다. 이 기법은 실세계에서 로봇을 일반화할 수 있도록 물리적 현실성을 개선하기 위해 노이즈 모델을 포함한다.

* Scene memory transformer  
자신의 policy network 전반에`Transformer model[83]`를 채택한 자기 중심 메커니즘을 이용한다. scene memory는 마주치는 모든 관찰을 내장하고 저장해서 유도 편향이 필요한 지도와 같은 메모리에 비해 더 큰 유연성과 확정성을 제공한다.



#### d) Reconstruction
에이전트는 관찰한 뷰로부터 다른 뷰를 재생성한다. 과거 연구는 360도 파노라마와 CAD 모델[84][85]의 픽셀별 복원에 집중했는데, 사람이 찍은 사진 데이터셋이 엄선되었다.
최근 연구는 eAI를 위해 이 복원 접근법을 채택했는데, 모델이 에이전트의 자기중심적 관찰과 센서의 컨트롤로부터(i.e. active perception) 장면 복원을 수행해야 하므로 더 복잡하다.
최근 연구[45]에서 에이전트는 자기중심적 RGB-D 관찰을 하며 가시영역을 벗어난 점유 상태를 재구성하고 정확한 사용지도로부터 시간에 따른 예측을 집계하여 정확한 점유 지도를 형성한다.
사용 예상은 각 VxV 영역 내 픽셀이 탐사되고 사용될 확률이 할당하는 픽셀별 분류 작업이다.

`coverage 접근법`과 비교해서, 사용(점유)상태를 예측하는 것은 에이전트가 직접 관찰하지 않는 지역을 다룰 수 있게 한다.

또 다른 최근 연구[22]는 픽셀별 복원보다 의미적 복원에 더 집중한다.
에이전트는 의미적 개념이 샘플된 쿼리 지역에 존재하는지 예측하도록 설계되었다.

* K-means approach 
쿼리 위치에 대한 진짜 복원의 개념은 특징 표현에 가장 가까운 J개 군집 중심이다. Agent는 샘플된 쿼리 시야에서 예측하여 진짜 복원하여 뷰를 얻으면 보상을 받는다.


### 2) Evaluation Metrics
* 타겟이 방문한 횟수  
영역(area), 흥미로운 물건과 같은 다른 타겟의 종류도 고려된다.
`area visited metric`은 다양한 방식이 있다. m^2단위의 절대범위, 장면상에서 탐사된 영역의 비율(%)

* 다음 작업에 끼친 영향  
VE의 성능은 VN과 같은 다음 작업에의 영향으로 측정될 수 있다.
이런 메트릭은 최근 연구에서 많이 보인다.

VE의 출력을 사용하는 다음 작업 예시
    * Image Navigation [26], [73]
    * Point Navaigation [11], [44]
    * Object Navigation [53], [54], [56]


### 3) Datasets
인기있는 데이터셋: 
* photorealitic RGB 데이터셋: Matteport3D, Gibson V1  
깊이, sementic segmentation  
Habitat Sim에서는 위 데이터셋을 쓰는데 다른 에이전트를 설정하고 많은 센서를 사용하는 추가 기능이 있다.
Gibson V1은 상호작용이나 실제 로봇 iGibson제어와 같은 특징들을 덧붙일 수 있다.
섹션 II에서 말했다시피 최근에 나온 시뮬레이터는 모두 시각적 탐사에 사용될 수 있다.




## B. Visual Navigation
VN에서 에이전트는 외부 사전 명령이나 자연어 명령이 있거나 없거나? 목표물을 향해 3D 환경을 탐사한다.
이 작업을 위한 목표물로는 점, 객체, 이미지[88],[89], 영역[11] 등 다양한 종류가 있다.
우리는 목표뮬로 점이나 객체을 골라 쓸건데, 가장 기초적이고 흔한 목표물이다. 우선순위? 탐색, 비전-언어-탐사 그리고 EQA처럼 더 복잡한 VN 작업을 빌드하기 위해그것들은 나아가 지각적 입력과 언어 설명서와 합쳐질 수 있다.
지점 탐사하에서, 에이전트는 객체 탐사에서 특정 지점을 찾아가도록 과제를 받고, 에이전트는 특정 클래스의 객체를 찾아가는 과제를 받는다.

* classic navigation approaches  
보통 국소화, 매핑, 경로 계획, 운동같은 수작업 하위 요소들로 구성되어 있다.
VN은 사례별 수작업을 줄이기 위해 이런 탐사 시스템을 데이터로부터 학습하기를 목표로 삼는다. 그래서 qa같은 데이터에 따라 처리하는 학습 방법으로 다음 작업이 더 좋은 성능을 내도록 쉽게 통합하려 한다.

* learning-based approaches
RGB and/or 깊이 센서를 사용하므로 센서 노이즈 측정에 더 강하다. 환경에 대한 의미적 이해를 통합할 수 있다. 에이전트가 이전에 본환경에 대한 지식을 일반화할 수 있게 하고 새로운 환경을 비지도 방식으로 이해하는 것을 돕는다. 인력이 덜 든다.

* hybrid approach  
양 세계의 최선을 합치는 것을 목표로 한다.


<br/>

* Challenge(2020)  
에이전트의 시야는 RGB-D
    * iGibson Sim2Real Challenge    
     point navigation, 73개의 고퀄리티 Gibson 3D 장면들로 훈련. 실제 아파트를 재구성한 Castro scene은 훈련, 개발, 테스트에 사용된다.  
    3가지 시나리오 (1)환경에 장애물이 없거나 (2) 에이전트랑 상호작용하는 장애물이 있거나 (3) 다른 움직이는 에이전트가 있다.
    * Habitat Challenge  
    point / object navigation  
    Gibson 데이터셋 일부분의 Gibson 3D 장면들은 지점 찾아가기를 위해 사용되는 반면, 90 Matterport3D 장면과 [11], [34]은 훈련, 검증, 테스트에서 객체 찾아가기작업에 사용된다.  
    * RoboTHOR Challenge은 객체 찾아가기 작업만 있다. 훈련과 평가는 3페이즈로 나뉜다. (1) 훈련 및 평가 (2) 시뮬레이션 아파트와 해당 실제 아파트에 대해 평가 (3) 실제 아파트에 대해 평가


최근 몇 년 동안 연구의 증가와 함께, 구현된 AI의 진전을 벤치마킹하고 가속화하기 위한 기본 포인트 탐색 및 객체 탐색 작업에서 시각적 탐색에 대한 챌린지도 조직되었다[38].

### 1) Categories
#### Point Navigation

#### Object Navigation

#### Vision-and-Laungage Navigation (VLN)


### 2) Evaluation Metrics

* VN
1. (정규화된 역) 경로 길이로 가중화된 성공(SPL): 수식 있음
2. 성공률: 제한 예산 안에 에이전트가 목표에 도달한 에피소드 비율
3. 예측된 경로와 성공한 에피소드에서 계산된 최단경로 길이의 비율
4. 성공/길찾기 오차 길이 (에이전트의 최종 위치와 가까운 객체나 목표 위치주변 성공 스레시홀드 경계사이의 거리)


* VLN
1. oracle success rate  
에이전트가 궤도 상에서 목표와 가장 가까운 지점들에 멈추는 비율.
2. trajectory length 궤도 길이  
VLN에서는 SPL이 제일 좋은 메트릭이다.목표뿐만아니라 사용된 경로를 고려한다.


* Vision-dialog navigtion
성공률이랑 오라클 성공률도 하지만 다른 메트릭도 있다.
1. goal progress  
에이전트가 목표를 향한 평균 진행?
2. oracle path success rate  
최단 경로를 따라 목표와 가장 가까운 지점에서 에이전트가 멈추는 성공 비율


### 3) Datsets
* VN  
VE와 유사한 데이터셋 사용.  
Matterport3D와 Gibson V1이 가장 인기있다. Gibson V1이 더 작고, 짧은 에피소드(시작지점에서 목표지점까지 GDSP가 낮다)들이 있다.
AI2-THOR 시뮬레이터/데이터셋도 쓰인다.

* VLN  
좀 다른 유형의 데이터셋 필요.  
Matterport3D 시뮬레이터에 딸린 Room-to-Room(R2R)데이터셋 (평균 29단어로 이루어진 21567 개의 길찾기 명령어)

* vision-dialog navigation  
Cooperative Vision-and-Dialog Navagation (CVDN) 데이터셋  
2050개의 사람끼리 대화와 Matterport3D 시뮬레이터 상에서의 7000개의 궤적






## C. Embodied Question Answering

eQA는 요즘에 범용 지능으로 성장하고 있다.
QA는 물리적 구체화 상태에서 QA를 수행하는 것은 많은 AI 능력을 필요로 한다. (너무 힘든 작업...)

> QA에 필요한 AI 기능
> * 시각 인식
> * 언어 이해
> * 질의응답
> * 상식 추론
> * 태스크 플래닝
> * goal-driven navigation
 

### 1) Categories
eQA는 navigation task와 qa task로 나뉜다.

* PACMAN[61]  
네비게이션 모듈에 구조 계층을 구성하는 기능
플래너에서 행위와 방향을 결정하고, 컨트롤러가 각 행위에 따라 얼마나 멀리 움직일지 결정함

Navigation 모듈과 QA 모듈은 따로 개발되고 `REINFORCE`로 합친다.
[62], [63]은 더 고성능 마스터 정책은 의미적 하위 목표가 하위정책으로 실행되게 하는 신경 모듈식 제어(NMC)로 팩맨 모델을 더 개선했다.

* Multi-target embodied QA(MT-EQA)  
더 복잡한 eQA작업으로, 여러 타겟을 가지는 질문을 연구한다. ("`침실`의 `사과`는 `거실`의 `오렌지`보다 `더 크니`?", 위치찾기, 물건 찾기, 비교하기)

* Interactive Questin Answering  
AI2-THOR환경에서 구현된 QA작업을 다루는 또 다른 작업이다.  
IQA는 EQA의 확장판. 
객체와 상호작용하는 에이전트가 특정 질문에 답하기 위해서 꼭 필요하다. ("`냉장고`에 `달걀`이 있니? 라는 질문에 답하기 위해서는 냉장고를 열어야 한다.)

[64]는 계층적 상호작용 메모리 연결망 (HIMN)을 제안한다. 
각 하위작업의 복잡성을 줄이는 동시에 이것은 다양한 시간 스케일?을 통틀어서 시스템 동작, 학습과 추론을 도와주는 제어부의 계층이다.  

> * 자기중심적 공간 게이트 반복 유닛(GRU):  
    환경의 공간 의미적 정보를 얻기위한 메모리 유닛으로서 반응한다.

> * 플래너 모듈은 다른 모듈에 대한 제어권을 가질 것이다. 모듈 목록: 
>     * navigator  
>     목표로 가는 최단경로를 찾기 위해 A* 탐색을 돌린다.
>     * scanner  
>     새 이미지를 찾기 위해 회전을 수행한다.
>     * manipulator  
>     환경의 상태를 변화시키기 위해 행동을 취한다.
>     * answerer  
>     AI 에이전트에게 보낸 질문을 답변한다.

> * [65]: Multi-agent embodied question answering in interactive environments,
> [65]는 다중 에이전트 관점에서 IQA를 연구했는데, 일부 에이전트는 질문에 답하기 위해 협력해서 상호작용하는 장면을 탐색했다.
> [65]는 여러 에이전트와 공유되어 3D장면을 먼저 재구성하고 QA를 수행할 수 있도록 다층 구조로 되어있고 장면 메모리로써의 의미적 메모리를 제안했다. 





### 2) Evaluation Metrics
EQA와 IQA는 두 하위 작업을 포함한다.
* Navigation
* Question Answering

Navigation 성능 평가 방법
1. 네비게이션 끝단에서 타겟까지의 거리 (=navigation error)
2. 시작점부터 마침점까지 움직일 때? 타겟까지의 거리 변화 (=gall process)
3. 에피소드 임의지점에서 타겟까지의 최단거리
4. 최대 에피소드 길이에 도달하기 전에 답변하려고 에이전트가 navigation을 종료하는 에피소드의 비율(%) (? 환경 탐색할 때 제한된 step을 초과하지 않고 질문에 대한 정보를 모두 찾아서 탐색을 종료하는 비율 )
5. 에이전트가 타겟 객체가 있는 방 (탐색을?) 종료하는 질문의 비율(%)
6. 에이전트가 타겟 객체가 있는방에 한 번이라도 방문하는 질문의 비율(%)
7. 타겟 객체 집합의 교집합? 타겟 객체에 대한 합집합의 교차점? Intersection over Union for target obejct (IoU)
8. IoU 기반 정답률
9. 에피소드 길이 (=trajectory length)

1, 2, 9번은 VN 작업의 평가기준으로도 쓰인다.


### 3) Datsets
* EQA데이터셋은 House3D(SUNCG[33]데이터셋의 일부)기반이다.   
(Replica 데이터셋[107]과 비사한 방과 레이아웃을 종합한 SUNCG)  
House3D는 SUNCG 정적인 환경을 가상환경으로 변환하는데, 에이전트는 물리적 제약(벽을 뚫고 지나가지 못함)을 가지고 탐사(NAVIGATE)한다. 
에이전트의 language grounding(자연어-기계어 매핑?), 상식 추론과 탐사 능력을 테스트하기 위해서 [61]은 CLEVER의 일련의 프로그램 기능을 사용한다. CLEVER는 객체와 객체의 특성(색, 존재, 위치, relative preposition?)에 관한 질문과 답변을 합성한다.
750개의 환경에서 5000개의 질문이 있고 7개의 방 종류와 45가지의 객체가 있다.

* MT-EQA[63]  
객체의 특성 (색, 크기, 거리)을 비교하는 6가지의 구성 질문이 있다.

* IQA [64]  
저자가 75000개의 객관식 질문으로 구성된 대규모 데이터셋인 IQUAD V1에 주석을 달았다. EQA 데이터셋과 유사하게, IQUAD V1은 객체 존재, 개수 세기 및 공간 관계에 대한 질문이었다.

# IV. Insight and Challenges
시뮬레이터 - 데이터셋 - 연구과제간의 상관관계를 확립하고, 현재 embodied AI 분야의 도저과제를 다룬다.

# Conclusion
embodied AI 시뮬레이터와 연구의 트렌드와 차이를 알아보았다.
9개의 시뮬레이터를 7가지의 특성으로 평가했다.
시뮬레이터의 현실성, 확장성, 상호작용성의 여부를 확인했다.

3대 과제 VE, VN, EQA는 피라미드를 이루며 과제 접근볍, 평가방식, 데이터셋을 살펴보았다.
이 논ㄴ문은 eAI 연구 카테고리를 다루며 현존하는 접근법들 리뷰했다.
시뮬레이터-데이터셋-연구 과제의 관계를 밝혓다.
