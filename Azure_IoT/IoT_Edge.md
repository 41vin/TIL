# Iot Edge란 무엇인가?

Azure IoT Edge는 클라우드에서 실행되는 클라우드 서비스와 디바이스에서 실행되는 런타임의 조합임. 런타임은 디바이스에서 시작되고 워크플로를 관리함. 워크프롤는 특정 순서로 연결되어 End-to-end 시나리오를 만드는 컨테이너 집합으로 구성됨. IoT Edge는 IoT Hub에 의해 관리됨.
Azure IoT Edge를 사용하면 클라우드 서비스를 사용하여 개발되는 워크로드를 에지 디바이스에서 실행할 수 있음. 워크로드는 docker 호환 컨테이너를 사용하여 배포되는 모듈임. 모듈은 인공 지능 애플리케이션이거나 Azure 및 타사 서비스이거나 비즈니스 logic일 수 있음.

IoT Edge의 기능

1. **로컬 변경에 거의 실시간으로 응답**: 일부 기능을 에지에서 구현할 수 있으므로 디바이스가 클라우드와 통신하는 데 사용하는 시간이 줄어듦. 따라서 데이터를 디바이스에서 처리하고 대기 시간을 줄일 수 있어 디바이스가 로컬 변경에 신속히 응답 가능. 또한 로컬로 실행되는 모듈은 필드 프로그래머블 게이터 어레이 같은 전문 하드웨어를 사용할 수 있음
2. **에지 디바이스 관리**: IoT Edge는 클라우드 인터페이스와 런타임 모듈을 제공하므로 IoT Hub를 통해 원격으로 워크로드를 관리하고 에지 디바이스에 배포가 가능함
3. **인증된 보안 하드웨어를 사용하여 배포**: IoT Edge를 사용하면 컨테이너 엔진을 지원하는 Linux 또는 Windows 디바이스에서 인증된 IoT Edge 하드웨어에 대한 액세스가 가능합니다.

4. **AI 및 분석 워크로드를 에지에 배포**: IoT Edge를 사용하면 클라우드에서 빌드 및 학습된 모델을 배포하고 에지 디바이스에서 실행할 수 있습니다. IoT Edge는 모델을 사용하여 로컬로 데이터를 처리하고 이벤트에 신속히 응답합니다.

5. **기존 개발자 기술 세트 및 코드 사용**: IoT Edge 코드는 C, C#, Java, Node.js, Python 등의 언어를 지원합니다.

6. **데이터를 관리하여 비용 절감**: IoT Edge 디바이스는 많은 양의 데이터를 캡처하지만 일반적으로 추가 분석에는 이 데이터 중 적은 부분만 필요합니다. 모든 데이터를 클라우드로 전송하면 사용자에게 전송 및 스토리지 비용이 발생합니다. IoT Edge는 필요에 따라 데이터 일부만 전송할 수 있기 때문에 비용을 줄일 수 있습니다. 집계된 데이터를 클라우드로 전송할 수도 있습니다. 집계된 데이터를 클라우드로 전송하면 대역폭과 스토리지 비용이 줄어들어 전체 데이터 관리 및 데이터 전송 비용이 줄어듭니다.

7. **오프라인 또는 일시적 모드에서 안정적으로 작동**: 클라우드와의 연결이 일시적이거나 오프라인 상태에서 IoT 디바이스가 작동해야 하는 경우가 많습니다. IoT Edge 디바이스 관리 기능은 클라우드에 다시 연결되면 원활한 작동을 위해 디바이스의 최신 상태를 자동으로 동기화합니다.

8. **에지 배포를 위한 보안 제공**: IoT Edge는 여러 가지 방법으로 보안을 제공합니다. IoT Hub를 사용하면 적절한 디바이스만 서로 통신하고 디바이스에 적절한 소프트웨어가 설치되도록 할 수 있습니다. IoT Edge는 클라우드용 Microsoft Defender와 통합하여 추가 보안을 제공할 수 있습니다. 또한 IoT Edge는 기밀 컴퓨팅을 위한 강력한 인증 연결 제공에 하드웨어 보안 모듈을 사용할 수 있습니다.

9. **IoT Edge 배포의 개인 정보 보호**: IoT Edge는 개인과 관련된 데이터를 보호할 수 있습니다. 데이터를 클라우드로 보내기 전에 개인 관련 데이터를 정리하여 개인 정보 보호를 개선할 수 있습니다. 온-프레미스에 데이터를 저장하면 보안, 기밀성, 개인 정보 규정 준수를 개선할 수 있습니다.

10. **게이트웨이 역할**: IoT Edge는 프로토콜 게이트웨이 역할을 할 수 있으므로 이러한 기능이 없는 IoT 디바이스에 연결 및 에지 분석을 제공합니다.

11. **타사 모듈의 가용성**: Azure Marketplace의 타사 모듈을 사용하여 출시 시간을 단축하고 에지에서 소프트웨어 솔루션의 견고성을 향상할 수 있습니다.