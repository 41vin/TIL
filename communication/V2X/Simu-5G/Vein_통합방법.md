# Veins를 Simu5G 프로젝트에 통합하는 방법입니다.

## SUMO 다운로드

1. SUMO v1.11.0을 다운로드합니다.
2. 압축 파일의 내용을 디렉토리에 추출합니다 (예: PATH_TO_SUMO/sumo-1.11.0).
3. README 파일에 포함된 지침에 따라 SUMO를 빌드합니다.

## Veins 다운로드

1. Veins 5.2를 다운로드합니다.
2. 압축 파일의 내용을 Simu5G와 inet4.4 디렉토리와 함께 워크스페이스에 추출합니다.
3. IDE를 사용하여 Veins를 워크스페이스에 가져오기

## OMNeT++ IDE를 시작하고 Simu5G 워크스페이스를 엽니다.

1. File | Import | General | Existing projects into Workspace를 선택합니다.
2. 워크스페이스 디렉토리를 선택하고 (Simu5G, inet4.4 및 Veins 폴더를 포함하는 디렉토리), "Search for nested projects" 상자를 선택합니다.
3. "veins" 및 "veins_inet" 프로젝트를 선택하고 마침을 클릭합니다.
4. 프로젝트 탐색기에서 "Simu5G" 폴더를 마우스 오른쪽 클릭하고 속성 | Project References를 선택합니다. "veins_inet" 상자를 선택한 후 확인을 클릭합니다.
5. 프로젝트 탐색기에서 "Simu5G" 폴더를 마우스 오른쪽 클릭하고 속성 | OMNeT++ | Project Features를 선택합니다. "Simu5G Cars" 상자를 선택한 후 확인을 클릭합니다.

## 프로젝트 빌드하기

1. CTRL-B를 눌러 프로젝트를 빌드합니다 (또는 Project | Build all).

## 시뮬레이션 예제 실행하기

1. 터미널에서 "veins/bin" 폴더로 이동합니다.
2. 다음 명령을 사용하여 SUMO를 실행합니다: ./veins-launchd.py -vv -c PATH_TO_SUMO/sumo-1.11.0/bin/sumo (PATH_TO_SUMO를 실제로 SUMO를 추출한 디렉토리로 대체합니다).
3. OMNeT++ IDE에서 'Simu5G/simulations/NR/cars' 예제 폴더를 선택합니다.
4. 툴바에서 'Run'을 클릭하여 시뮬레이션을 시작합니다.
