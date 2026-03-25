# RTSP 스트리밍 기반 OpenCV와 MediaPipe를 이용한 실시간 손 제스처 및 PTZ 카메라 제어 시스템 구현

* OpenCV와 MediaPipe를 이용하여 RTSP 스트리밍 영상 속의 손동작을 인식하고, 이를 통해 Tapo PTZ 카메라를 물리적으로 실시간 제어하는 프로젝트입니다.
* 본 프로젝트 목표는 C++20 기능, 컴퓨터 비전 파이프라인 학습입니다.
* 본 프로젝트는 구상, 설계, 구현 단계에서 구글 제미나이 프롬프트의 도움을 받았으나, 코딩 에이전트는 사용하지 않고 모두 리눅스 터미널에서 VIM으로 손코딩하였습니다.

---

## 프로젝트 개요 (Overview)
본 프로젝트는 네트워크 카메라(Tapo)의 RTSP 스트림을 분석하여 사용자의 손바닥을 추적(Auto-tracking)하고, 특정 제스처를 통해 카메라의 Pan-Tilt 동작을 원격으로 제어하는 인터랙티브 시스템을 구현합니다.

## 주요 기능 (Key Features)
* **Real-time Ingestion:** OpenCV를 이용한 고성능 RTSP 스트리밍 수신.
* **Hand Tracking:** MediaPipe Hands를 이용한 21개 랜드마크 실시간 추출.
* **Auto-Follow:** 화면 중앙과 손바닥의 오차를 계산하여 카메라가 자동으로 손을 따라가는 추적 로직.
* **PTZ Control:** ONVIF 프로토콜을 통한 카메라의 물리적 Pan/Tilt 제어.

## 기술 스택 (Tech Stack)
* **Language:** C++20, Python 3.10
* **Library:** OpenCV 4.x, MediaPipe, ONVIF (python-onvif-zeep)
* **Environment:** Ubuntu 22.04 LTS (VirtualBox), CMake
* **Protocol:** RTSP (Streaming), SOAP/ONVIF (Control)

---

## 시스템 아키텍처 (Architecture)

1. **Producer Thread:** 카메라로부터 RTSP 패킷을 받아 가장 최신 프레임만 뮤텍스로 보호된 버퍼에 갱신합니다.
2. **Consumer (Main) Thread:** 최신 프레임을 가져와 MediaPipe 연산을 수행하고 좌우 반전(Flip) 처리를 합니다.
3. **Controller:** 계산된 좌표 오차를 바탕으로 ONVIF 명령을 생성하여 카메라에 전송합니다.

---

## 개발 환경 준비
### Ubuntu 개발 환경 구축
```bash
sudo apt update
sudo apt install -y build-essential cmake git pkg-config
sudo apt install -y g++-11
```
### OpenCV 설치
```bash
sudo apt install -y libopencv-dev
```

---

## 빌드
```bash
mkdir -p build
cd build
cmake ..
make
```

## 실행
* onvif 제어가 가능한 네트워크 카메라가 준비되어야 함.
* 네트워크 카메라의 onvif 연결 계정과 주소는 소스 코드에서 변경 가능함.
* build 디렉토리에서 아래와 같이 실행함.
```bash
./GestureControl
```
