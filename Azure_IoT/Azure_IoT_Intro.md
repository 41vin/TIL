# Azure IoT란?

세 가지 구성요소가 존재함

1. **IoT 디바이스**: IoT 디바이스는 인터넷에 연결된 센서 또는 작동기가 있는 전자 회로기판으로 구성됨. IoT 디바이스에는 인터넷에 연결되는 몇가지 형식의
라디오 메커니즘(ex. Bluetooth 또는 Wi-fi)등이 포함됨. 압력 센서, 온도 및 습도 센서, 가속도계는 모두 IoT 디바이스의 에임. 이러한 디바이스는 자신이 위치한
특정 환경에 대한 정보를 캡쳐함. 예를 들어 가속도계는 엘리베이터가 정지 상태인지 움직이고 있는지 보여줄 수 있음.

2. **통신**: 디바이스는 백 엔드 서비스와 양방향으로 통신할 수 있음. 디바이스에서 서비스로 데이터를 보낼 수 있고, 서비스에서 신호를 받아 데이터 수집의 빈도를 높일 수도 있음.
IoT 디바이스와의 통신에는 몇 가지 추가 고려사항이 있음. 예를 들어 IoT 디바이스는 전원이 제한적이고 연결 속도가 느리거나 연결이 일시적일 수 있음.

3. **백 엔드 서비스**: 백 엔드 서비스는 디바이스에서 원격 분석 데이터를 대규모로 수신하고 해당 데이터의 처리 및 저장 방법을 결정함.
   또한 디바이스와 통신하고 디바이스 상태를 제어하고 모니터링하며 디바이스 수명 주기를 관리함.

## Azure IoT를 통해 다음을 수행할 수 있음

1. 고객,운영,제품/자산, 직원 간의 데이터 사일로를 줄여 **비즈니스 프로세스**를 혁신
    - 데이터 사일로: 기업 내부에서 발생한 데이터가 각자 분리되어 간극이 생긴 상태
2. Azure IoT Central을 사용하여 최소한의 클라우드 전문 지식(SaaS 솔루션)으로 관리되는 IoT용Software as a Service를 배포합니다.

3. 일반적인 IoT 시나리오를 위한 산업별 IoT 솔루션을 사용자 지정합니다(Azure IoT 솔루션 가속기를 통한 PaaS 솔루션).

4. Azure IoT Edge를 사용하여 클라우드에서 에지 디바이스까지 인텔리전스를 확장합니다.

5. Azure IoT Hub를 사용하여 수십억 개의 IoT 자산을 연결, 모니터링, 제어합니다.

6. Azure Digital Twins를 사용하여 물리적 공간 또는 자산의 디지털 모델을 만듭니다.

7. Azure Time Series Insights를 사용하여 시계열 IoT 데이터에서 실시간으로 인사이트를 탐색하고 얻습니다.

8. Azure Sphere를 사용하여 매우 안전한 MCU 탑재 디바이스를 빌드 및 연결합니다.

9. Azure Maps를 사용하여 데이터에 지리 공간적 컨텍스트를 제공합니다.

10. 예측 유지 관리와 같은 기존 애플리케이션을 넘어 가능성의 예술을 탐색합니다.

11. 디바이스와 클라우드 간에 자동화된 피드백 루프를 생성하여 솔루션의 효율성을 높입니다.

## Azure IoT 작동 방식

### 클라우드 컴퓨팅 서비스 요약

IaaS(Infrastructure as a Service): 인터넷을 통해 가상화된 컴퓨팅 리소스를 제공, 일반적으로 데이터 센터에 배포된 하드웨어 리소스

- Azure IoT 제품 포트폴리오는 솔루션, 즉 **PaaS 솔루션**과 **SaaS 솔루션**을 만드는 두 가지 경로를 제공함.

## Platform as a Service 솔루션

Azure IoT 솔루션 가속기는 특정 업계 vertical 부문을 위한 사용자 지정 가능한 PaaS 솔루션임. IoT 솔루션 가속기는 수직 부문 내에 신속하게 배포할수 있는 이미 빌드된 솔루션을 제공함.
IoT 솔루션 가속기의 예로는 원격 모니터링, 연결된 factory, **예측 유지 관리** 및 디바이스 시뮬레이션이 있음.

## Software as a Service 솔루션

SaaS 솔루션은 디바이스 모델이 몇 개 되지 않고 시나리오의 예측 가능성이 높으며 IoT/IT 기능이 제한된 조직에 적합함. Azure IoT Central은 최소의 IoT 경험만 있으면 빠르게 시작할 수 있는 관리되는 SaaS
솔루션. IoT Central은 IoT 요구 사항이 간단하고 클라우드 개발 전문 지식이 별로 필요하지 않은 경우에 적합함.

## Platform as a Service 기술

### Azure PaaS 기술

1. 디바이스 지원
  -   Azure IoT는 다양한 디바이스를 지원하는 다양한 수단을 제공함. 여기에는 Azure IoT 디바이스 SDK, Azure IoT 인증된 디바이스, Azure IoT 및 Windows 10 IoT용 보안 프로그램이 포함됨
2. IoT
  - **Azure IoT Hub**: Azure IoT Hub는 수백만 대의 IoT 디바이스와 솔루션 백 엔드 간의 양방향 통신을 가능하게 해주는 관리되는 서비스, 이를 이용해 에지에서 클라우드까지 수십억 대의 IoT 디바이스를 연결,관리,확장할 수 있음
  - **Azure Time Series Insights**: Azure Time Series Insights를 상ㅇ하면 IoT 디바이스에서 새성된 대량의 시계열 데이터를 저장, 시각화, 쿼리하여 실행 가능한 인사이트를 얻을 수 있음
  - **Azure Sphere**: Azure Sphere는 마이크로 컨트롤러 장치(MCU)가 탑재된 디바이스의 보안을 위한 엔드투엔드 솔루션임. Azure Sphere는 Azure Sphere 운영 체제, Azure Sphere 인증 MCU, Azure Sphere Security Service라는 세 부분으로 구성됨
      1. Azure Sphere 운영체제: IoT 디바이스용 보안 플랫폼을 만드는 안전한 OS
      2. Azure Sphere 인증 MCU는 연결 기능과 믿을 수 있는 하드웨어 신뢰 루트를 제공
      3. Azure Sphere Security Service는 디바이스-디바이스 및 디바이스-클라우드 통신을 위한 신뢰를 중개함. 이 서비스는 새로운 위협을 검색하고 보안을 갱신함
      &rarr; 따라서 Azure Sphere를 사용하면 디바이스 스택에 걸쳐 계층화된 엔드투엔드 보안 기능을 사용하여 새 디바이스를 빌드하고 기존 장비를 안전하게 연결할 수 있음
  - **Azure IoT Edge**: IoT Edge는 IoT Hub를 기반으로 구축되며, 워크로드 일부를 에지 디바이스로 이동하고 고객 환경을 개선할 수 있음
  - **IoT Hub Device Provisioning Service**: Azure IoT Hub Device Provisioning Service는 IoT Hub의 도우미 서비스임. 고객은 이 디바이스 프로비저닝 서비스를 사용하여 안전하고 확장 가능한 방식으로 수백만 대의 디바이스를 프로비저닝할 수 있음
  - **Azure Digital Twins**: Azure Digital Twins를 사용하면 사람, 공간, 디바이스 간의 관계와 상호 작용을 모델링할 수 있음. 이 기능을 통해 분석에 관심이 있는 물리적 환경의 모델을 만들 수 있음. 환경을 모델링한 후 예측 알고리즘을 실행하여 전체 시스템의 동작을 예상할 수 있음. 예를 들어, Digital Twins를 통해 사무실을 모델링하는 경우, 실제 공간을 복제하여 사용 가능한 사무실 공간을 최적화할 수 있음.
  - **Azure Maps**: Azure Maps는 웹 및 모바일 애플리케이션에 정확한 지리적 컨텍스트를 제공하기 위해 최신 매핑 데이터를 사용하는 지리 공간적 서비스의 컬렉션임. API를 사용하여 공간 분석, 매핑 서비스, 모바일 서비스를 앱에 추가할 수 있음
3. 데이터 분석
  - **Azure Machine Learning**: Azure Machine Learning은 Azure에서 기계 학습 모델을 빌드, 학습, 배포하기 위한 제품군을 대표함. Azure의 데이터 및 분석 솔루션은 IoT 데이터를 저장하고 분석하는 솔루션 집합을 대표함.
    여기에는 Azure Stream Analytics, Azure HDInsight, Azure Data Lake, Azure Cosmos DB, Azure Data Fabric이 포함됨. 또한 Azure 함수를 사용하여 시스템을 빌드하면 서버리스 애플리케이션을 사용할 수 있음. 이러한 함수를 결합하여 보다 포괄적인 애플리케이션을 만들 수 있음. 코드가 실행되는 시간에 대해서만 비용을 지불함

4. 시각화 및 통합
  - IoT 솔루션용 시각화 및 통합 솔루션에는 Microsoft Flow, Azure Logic Apps, Azure 웹앱, Notification Hubs, Azure Active Directory, Microsoft Power BI, Azure Monitor, Power Apps가 포함됨

## Azure IoT를 사용해야 하는 경우

| |질문|
|---|----|
|비즈니스 프로세스 재정렬|IoT를 사용하여 고객, 운영, 제품/자산, 직원에 걸친 조직 비즈니스 프로세스를 다시 조정할 계획이 있나요?|
|클라우드 전문 지식|팀에 최소한의 클라우드 전문 지식이 있나요?|
|IoT Edge 디바이스로 처리 오프로드|에지 디바이스에 처리를 오프로드할 계획이 있나요?|
|규모|수백만 개의 IoT 자산을 관리해야 하나요?|
|물리적 공간 모델링|센서를 사용하여 실제 공간을 모델링해야 하나요?|
|시계열 데이터|대규모 시계열 데이터가 있나요?|
|IoT 디바이스 보안|보안 디바이스를 관리해야 하나요?|

### 지침 요약

- 특정 수직 부문 내에 산업 관련 문제가 있는 경우, Azure IoT 솔루션 가속기와 같은 PaaS 솔루션을 고려
- 디바이스 모델이 적고 예측 가능한 시나리오가 있다면 SaaS 솔루션 선택
- 솔루션의 모든 측면을 유지 관리하려는 경우, Azure Sphere, Azure IoT Edge 등의 기본 PaaS 기술을 선택
