# ROS2 vs AI Stack

## 목적

AI 서비스 개발 경험이 로봇 시스템 안에서 어떤 역할을 하는지 이해하기 위한 문서.

---

## 기존 AI 서비스 구조

사용자

↓

Frontend

↓

Backend API (FastAPI)

↓

LLM / ML Model

↓

Database

↓

결과 반환

---

## 로봇 시스템 구조

사용자

↓

Camera / Microphone / Sensors

↓

Perception

(VLM, Object Detection, Speech Recognition)

↓

Planner

(LLM, VLA)

↓

ROS2

↓

Robot Control

↓

Motor / Robot Arm / Locomotion

↓

실제 행동

---

## 기술 스택 비교

### PyTorch

역할

* 모델 학습
* 추론

로봇에서의 위치

* VLM
* LLM
* VLA
* Detection Model

---

### FastAPI

역할

* 서비스 API 제공

로봇에서의 대응 개념

* ROS2 Service
* ROS2 Action

차이점

FastAPI는 HTTP 요청을 처리한다.

ROS2는 로봇 내부 구성 요소 간 통신을 담당한다.

---

### Kafka

역할

* 비동기 메시지 전달

로봇에서의 대응 개념

* ROS2 Topic

예시

Camera Node

↓

/camera/image

↓

Object Detection Node

---

### Docker

역할

* 서비스 배포

로봇에서의 대응 개념

* ROS2 Node 분리
* 컨테이너 기반 로봇 배포

---

### Database

역할

* 데이터 저장

로봇에서의 대응 개념

* Robot Memory
* Knowledge Base
* Map Data

---

## 내가 가진 강점

### AI

* NLP
* LLM
* BERT
* 이상탐지
* 시계열 분석

### Software

* Python
* API 개발
* 서비스 배포
* Docker

### System Design

* 대규모 서비스 설계
* 모델 운영
* 데이터 파이프라인

---

## 내가 부족한 영역

### Robotics

* ROS2
* Robot Control
* Motion Planning
* Sensor Integration

---

## 현재 나의 위치

나는 로봇 개발 경험은 없지만,

AI 모델 개발과 서비스 개발 경험은 이미 보유하고 있다.

따라서 처음부터 로봇 엔지니어가 되기보다 AI 엔지니어의 강점을 유지하면서 Robotics 영역을 확장하는 전략이 적합하다.

---

## 정리

AI 엔지니어는 로봇의 두뇌를 만든다.

ROS2는 두뇌와 몸을 연결하는 신경계 역할을 한다.

내가 앞으로 학습해야 하는 핵심은 새로운 AI 기술보다 ROS2와 Robot Control 영역이며, 기존 AI 역량을 로봇 시스템에 연결하는 것이 목표이다.
