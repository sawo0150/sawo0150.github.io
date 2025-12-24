---
layout: single
title: "VRIS: 대규모 무인 자동화 시스템 모니터링 & 원격 개입 VR 관제 시스템"
# categories: Projects
tags: [Projects, VR, ROS2, WebRTC, Teleoperation, SLAM, Unity]
excerpt: "VR 기반 가상 관제실에서 다수 로봇을 모니터링하고, Edge Case에서 사람이 VR로 원격 개입(Intervention)하는 시스템(VRIS) 구현"
author_profile: true
toc: true
toc_label: "Table of Contents"
toc_icon: "list-alt"
header:
  overlay_image: /assets/images/portfolio-header.jpg
  overlay_filter: rgba(0, 0, 0, 0.5)
  caption: "VRIS (VR Intervention System)"
  actions:
    - label: "GitHub Repo"
      url: "https://github.com/sawo0150/VR-InterventionSystem"
    - label: "GitHub Repo - Robot"
      url: "https://github.com/sawo0150/VR-InterventionSystem_Robot"
classes: "text-white"
---

# 팀 프로젝트 - VRIS (VR Intervention System) 🕹️🤖

> **한 명(또는 소수)의 오퍼레이터가 VR 관제실에서 다수 로봇을 모니터링**하고,  
> 자율주행이 해결하기 어려운 **예외 상황(Edge Case)** 에서만 **VR로 제어권을 넘겨받아 개입(Intervention)** 하는 시스템을 구현했습니다.  
> *(시뮬레이션 + 실제 로봇 연동 모두 구현)* 

{% include video id="u10mQcBseCA" provider="youtube" %}

{% include video id="f3-wrwbEFRE" provider="youtube" %}

---

## 소개

물류센터/배달로봇/로보택시/드론과 같이 **대규모 무인 자동화 시스템**이 확산되면서, “군집 관제”가 중요해지고 있습니다.  
하지만 자율주행은 아직 모든 상황을 완벽히 처리하기 어렵고(시야 제한, 돌발 장애물, 보행자/어린이 등), 기존 2D 관제 화면만으로는 상황 이해에 한계가 있습니다.

VRIS는 이를 해결하기 위해:

- **VR 기반 가상 관제실(Virtual Control Room)** 에서 여러 로봇을 직관적으로 모니터링하고
- 특정 로봇에서 문제가 발생하면 **1인칭(로봇 POV) 제어실로 전환**하여
- **오퍼레이터가 직접 원격 개입** 후 다시 자율주행으로 복귀시키는 흐름을 제공합니다.

---

## 기간 / 인원 / 역할

- **기간**: 2025년 2학기 VR/AR의 실습 및 개론 수업 프로젝트
- **인원**: 6인 팀 프로젝트 (VRIS Brothers)
- **내 역할 (팀장)**: **통신 및 로봇 소프트웨어 개발**  
  - ROS2 기반 로봇 시스템 구성/연동
  - WebRTC + TCP Socket 하이브리드 통신 구조 구현
  - VR ↔ Robot 제어/상태 동기화, 모드 전환(중재) 로직 설계

---

## Tech Stack

### Software
- **ROS2 (Humble)** 기반 로봇 노드 구성 (SLAM/제어/상태 송수신)
- **Unity (VR)**: Monitoring Room / Control Room UI 및 인터랙션
- **WebRTC (P2P)**: 360° 영상 초저지연 스트리밍
- **TCP Socket + Signaling/Relay Server**: 제어/상태 데이터 안정 동기화(JSON)

### Hardware (Real Robot)
  <img src="../images/2025-12-23-VR-intervention_system/tmp172D-1766575993685.jpg" alt="그림입니다.  원본 그림의 이름: CLP00003ab80001.bmp  원본 그림의 크기: 가로 2081pixel, 세로 1365pixel" style="zoom:50%;" />  

- **Computing**: Laptop (RTX 3070, Ubuntu 22.04, ROS2 Humble)
- **MCU**: Arduino UNO (모터 제어)
- **Sensors**: RPLidar A3M1, HFI-A9 IMU, Insta360 (360 카메라)
- **Mobile Platform**: 로봇청소기 기반 이동 플랫폼 개조

---

## 시스템 아키텍처

아키텍처는 크게 **VR(관제/개입) – 통신 – 로봇(ROS2)** 로 구성됩니다.

### 1) 로봇 시스템(ROS2) 구성

- **SLAM (Mapping & Localization)**: LiDAR + IMU 기반 실시간 지도 생성 및 위치 추정
- **Mode Manager (제어권 중재)**  
  - 평상시: 자율주행(시나리오) 신호  
  - 비상/개입 시: VR 조이스틱 기반 수동 제어 신호  
  - 두 제어 입력을 안전하게 중재하여 “자율 ↔ 개입” 전환을 가능하게 함

> ![그림입니다.  원본 그림의 이름: CLP000022944a70.bmp  원본 그림의 크기: 가로 1405pixel, 세로 649pixel](../images/2025-12-23-VR-intervention_system/tmp9F4F.jpg)
>
>   ![그림입니다.  원본 그림의 이름: CLP000022940002.bmp  원본 그림의 크기: 가로 3286pixel, 세로 1080pixel](../images/2025-12-23-VR-intervention_system/tmpA8F4.jpg)  

### 2) 하이브리드 통신 시스템 (영상 vs 데이터 분리)

VR 원격 조작에서 중요한 것은 **(1) 멀미를 줄이는 초저지연 영상**과 **(2) 정확한 제어/상태 데이터 전달**입니다.  
그래서 트랙을 2개로 분리했습니다.

- **Track 1 (Video / WebRTC P2P)**  
  - 360° 고해상도 영상 **P2P 직접 전송**
  - 목표: **Latency 200ms 미만**으로 원격 조작 시 인지 부조화(멀미) 최소화
- **Track 2 (Data / TCP Socket)**  
  - 제어(Control) & 로봇 상태(State) **손실 없는 동기화**
  - Signaling/Relay Server를 통해 JSON 메시지 중계  
  - Robot→VR: SLAM pose/상태/오류  
  - VR→Robot: 조이스틱 입력(cmd), 모드 전환 신호

>   ![그림입니다.  원본 그림의 이름: CLP000022940001.bmp  원본 그림의 크기: 가로 761pixel, 세로 683pixel](../images/2025-12-23-VR-intervention_system/tmp97CE.jpg)  
>

---

## 주요 기능

### 1) Monitoring Room (통합 관제실)

![image-20251224203728321](../images/2025-12-23-VR-intervention_system/image-20251224203728321.png)

- **미니맵**에서 여러 로봇의 위치/상태를 한눈에 확인
- **로봇 카메라 뷰**를 목록으로 제공하고, 클릭 시 개입(Intervention) 진입
- 로봇에서 에러 발생 시 **Alert(경고) + 에러 위치 표시 + 텔레포트 트리거** 제

### 2) Control Room (1인칭 제어실)

![image-20251224203743241](../images/2025-12-23-VR-intervention_system/image-20251224203743241.png)

- 로봇 카메라를 기반으로 **360° 파노라마 뷰** 제공(Insta360 투영)
- VR 컨트롤러(오른손 조이스틱)로 로봇을 직접 조작  
- 개입 종료 후 자율주행 모드로 복귀

---

## 시뮬레이션 시나리오 (Edge Case 이벤트)

시뮬레이션은 “자율주행이 취약한 상황에서 Human-in-the-loop가 왜 필요한가?”를 보여주기 위한 이벤트 중심으로 구성했습니다.

### Event 1: Undefined Roads (비정형 도로/환경 변수)
- 자율주행 경로가 정의되지 않은 도로에서 에러 발생 → 사용자 개입 요청
- 장애물:
  1) 낙석(움직이는 장애물) 회피  
  2) 안개/구름으로 시야 제한 구간 통과  
  3) 야생동물 돌발 출현 회피

### Event 2: Unexpected Road Block (돌발 경로 차단/우회)

- 쓰러진 나무로 도로가 차단 → 로봇이 에러 감지
- 사용자가 VR로 상황 확인 후 **Off-road 우회** 등 유연한 판단으로 목적지 도달 

### Event 3: Children Crossing Area (어린이 보호구역)
- 스쿨존 진입 시 자동 주행을 멈추고 사용자 개입 요청
- 어린이의 불규칙한 움직임을 고려해 사람이 직접 서행/안전 주행

---

## 실제 로봇(Real-World) 구현

- 로봇청소기 기반 플랫폼을 개조하여 2륜 구동/지지대를 구성
- 전면부에 LiDAR/IMU 배치(SLAM), 최상단에 Insta360 배치(시야 확보)
- ROS2에서 SLAM 매핑 및 모드 매니저 노드가 통합 동작
- VR 관제 시스템과의 **P2P 통신 연결이 안정적으로 유지됨**을 로그/구동 화면으로 확인

---

## 결과 & 의의

- **시뮬레이션 + 실제 로봇 연동** 모두에서 “모니터링 → 에러 감지 → VR 개입 → 복귀” 흐름을 구현
- 360° 영상은 WebRTC P2P로 **200ms 미만 지연 목표**의 초저지연 전송 구조를 적용
- 제어/상태는 TCP 기반으로 안정적 동기화를 구성하여 원격 조작의 신뢰성을 확보

---

## 내가 담당한 핵심 포인트 (강조)

- **하이브리드 통신 설계**: 영상(P2P WebRTC)과 데이터(TCP Socket)를 분리해 “저지연 + 신뢰성”을 동시에 확보
- **ROS2 로봇 소프트웨어 통합**: SLAM/상태/제어 흐름을 모듈화하고, **Mode Manager로 제어권 전환 안전성**을 확보
- **VR ↔ Robot 인터페이스 정의**: VR 입력(cmd)과 로봇 상태(pose/error)를 JSON 메시지로 동기화하여 end-to-end 제어 루프 구현

---

## 아쉬운 점 & 다음 확장 아이디어

- 이번 구현은 로컬 네트워크 기반 프로토타입 → 향후 광역 통신망 환경에서의 안정성/보안/지연 최적화가 필요
- 다중 로봇 규모 확장 시, 관제 UI의 정보 밀도(미니맵/경고/우선순위) 설계가 더 중요
- 적용 분야 확장: 무인 지게차, 군집 드론, 스마트팜, 위험 지역 작업 로봇 등

---

## 첨부 자료

- 최종 결과보고서(PDF): 목표/구현/결과/회고 정리 : [Link](/assets/files/VRInterventsionSystem_report.pdf)

{% include video id="u10mQcBseCA" provider="youtube" %}

{% include video id="f3-wrwbEFRE" provider="youtube" %}

---
