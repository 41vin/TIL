# Quickstart: Send telemetry from a device to an IoT hub and monitor it with the Azure CLI
<출처 https://learn.microsoft.com/en-us/azure/iot-hub/quickstart-send-telemetry-cli>
<br>

한국어 번역이 없어 직접 번역하여 정리하고 있습니다. <br>


## 2개의 CLI 세션을 이용하여 진행
- CLI 세션  1은 IoT hub와 통신하고 있는 IoT 디바이스를 시뮬레이션
-  CLI 세션  2는 CLI 세션  1에 있는 IoT 디바이스의 모니터링, 디바이스에게 메세지, 커맨드, property 업데이트를 수행
<br><br>

1. CLI 세션  1에서 **az extension add** 커맨드를 실행.
  - __az extension command__: Microsoft Azure IoT Extension for Azure CLI를 CLI shell에 추가하는 역할

```bash
az extension add --name azure -iot
```
이 과정은 다시 실행할 때 할 필요 없음
<br>

2. CLI 세션  2를 open한다.

<br><br><br>

## IoT hub 생성하기
- Azure resource group: Azure의 리소스들이 배치되고 관리되는 logical container
- IoT hub: IoT application과 디바이스들 간의 쌍방향 통신의 central message hub
<br>

1. **CLI 세션  1**에서 __az group create__ 커맨드를 이용해 resource group을 생성한다. 
```bash
az group create --name MyResourceGroup --location koreacentral
```

2. **CLI 세션  1**에서 __az powershell module iot hub create__ 커맨드를 이용하여 IoT Hub를 생성. 
   - 결과가 나올 때 까지 시간이 좀 걸리는 과정임
```bash
az iot hub create --resource-group MyResourceGroup --name {YourIoTHubName}
```
<br><br><br>

## 디바이스 생성 및 모니터링
이 섹션에서는 CLI 세션  1에서 시뮬레이션 디바이스를 만들 수 있음. 시뮬레이션 디바이스는 IoT Hub에게 Telemetry를 송신함. 또한 CLI 세션  2에서 telemetry와 events 모니터링 가능.

1. CLI 세션  1에서 시뮬레이션 디바이스의 identity를 생성하기 위해 __az iot hub device-identity create__ 커맨드 실행
```bash
az iot hub device-identity create -d simDevice -n {YourIoTHubName}
```

2. CLI 세션  1에서 시뮬레이션 디바이스를 구동하기 위해 __az iot device simulate__ 커맨드를 실행.
```bash
az iot device simulate -d simDevice -n {YourIoTHubName}
```
<br>
디바이스를 모니터링 하기 위해:

1. CLI 세션  2에서 지속적으로 시뮬레이션 디바이스를 모니터링하기 위해 __az iot hub monitor-events__ 커맨드 실행 <br>
   결과물은 telemetry를 보여주게 됨.
```bash
az iot hub monitor-events --output table -p all -n {YourIoTHubName}
```
2. 모니터를 중지하려면 ctrl + C를 누르면 됨.
<br><br><br>


## 메시지 송신을 위해 CLI 사용하기
본 섹션에서는 시뮬레이션 디바이스에게 메시지를 보낼 수 있음.

1. CLI 세션 1에서 시뮬레이션 디바이스가 실행중이어야 함. 만약 디바이스가 멈춰있다면 아래의 커맨드를 입력하여 다시 실행시킬 것
   
```bash
az iot device simulate -d simDevice -n {YourIoTHubName}
```

2. Cloud-to-device 메세지를 보내기 위해 CLI 세션 2에서 __az iot device c2d-message send__ 커맨드 사용
```bash
az iot device c2d-message send -d simDevice --data "Hello MCL" --props "key0=value0;key1=value1" -n {YourIoTHubName}
```

3. 위의 과정 이후 CLI 세션 1에서 수신 메시지를 확인할 수 있음
<br><br><br>

## Device method 호출을 위해 CLI 사용하기

1. 마찬가지로 CLI 세션 1에서 디바이스가 시뮬레이션 중인지 확인하기
2. __az iot hub invoke-device-method__ 커맨드 실행
```bash
 az iot hub invoke-device-method --mn MySampleMethod -d simDevice -n {YourIoTHubName}
```
3. CLI 세션 1에서 method call 결과물 출력 확인

<br><br><br>

## 디바이스 properties를 업데이트 하기 위해 CLI 사용하기
1. CLI 세션 1에서 디바이스가 시뮬레이션 중인지 확인하기
2. CLI 세션 2에서 시뮬레이션 디바이스 twin의 속성을 원하는 상태로 업데이트하기 위해 __az iot hub device-twin update__ 커맨드 실행, 본 예제에선 온도 조건을 설정
```bash
az iot hub device-twin update -d simDevice --desired '{"conditions":{"temperature":{"warning":98, "critical":107}}}' -n {YourIoTHubName}
```
```bash
az iot hub device-twin update -d simDevice --desired '{\"conditions\":{\"temperature\":{\"warning\":98, \"critical\":107}}}' -n {YourIoTHubName}
```

3. CLI 세션 1에서 시뮬레이션 디바이스의 결과물을 바로 확인할 수 있음
4. CLI 세션 2에서 디바이스 속성의 변경을 리포트하는 __az iot hub device-twin show__ 커맨드를 입력
```
az iot hub device-twin show -d simDevice --query properties.reported -n {YourIoTHubName}
```

### 포털에서 message metrics 확인은 생략하였습니다!
<br><br><br>

## 리소스 정리하기
불필요한 요금을 피하기 위해 리소스를 정리해야 함!
1. __az group delete__ 커맨드 실행. 이 커맨드를 입력해 resource group, IoT hub, 그리고 device registration을 지울 수 있음.
```bash
az group delete --name MyResourceGroup
```
2. Resource group 삭제를 다시 한번 확인하기 위해 __az group list__ 커맨드 입력
```
az group list
```
