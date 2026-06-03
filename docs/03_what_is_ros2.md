# What is ROS2?

## 한 줄 요약

ROS2(Robot Operating System 2)는 로봇을 개발하기 위한 미들웨어 프레임워크이다.

엄밀히 말하면 운영체제(OS)가 아니라 여러 프로그램(노드)들이 서로 통신할 수 있도록 도와주는 로봇 소프트웨어 플랫폼에 가깝다.

---

## 왜 필요한가?

로봇은 다양한 기능이 동시에 동작해야 한다.

예를 들어 휴머노이드 로봇은

* 카메라 영상 처리
* 음성 인식
* 물체 인식
* 경로 계획
* 팔 제어
* 보행 제어

등이 동시에 실행된다.

ROS2는 이러한 기능들을 독립적인 노드(Node)로 분리하고 서로 데이터를 주고받을 수 있도록 한다.

---

## 핵심 개념

### Node

하나의 기능을 수행하는 프로그램 단위

예시

* Camera Node
* Object Detection Node
* Navigation Node
* Robot Arm Control Node

---

### Topic

노드 간 데이터를 전달하는 통신 채널

예시

Camera Node

↓

/camera/image

↓

Object Detection Node

---

### Publisher / Subscriber

Publisher

* 데이터를 발행하는 노드

Subscriber

* 데이터를 구독하는 노드

예시

Camera Node → 이미지 발행

Object Detection Node → 이미지 수신

---

### Service

요청(Request)과 응답(Response)이 필요한 통신 방식

예시

"현재 배터리 상태 알려줘"

↓

배터리 노드 응답

---

### Action

시간이 오래 걸리는 작업을 수행하기 위한 통신 방식

예시

"컵을 가져와"

↓

작업 진행 상황 전달

↓

작업 완료

---

## AI와 ROS2의 관계

ROS2는 AI 모델이 아니다.

ROS2는 AI 모델을 로봇과 연결하는 인프라 역할을 한다.

예시

카메라

↓

VLM

↓

LLM/VLA

↓

ROS2

↓

모터 제어

↓

로봇 행동

---

## 내가 현재 가진 역량

* NLP
* LLM
* 이상탐지
* 서비스 개발
* API 개발

## 앞으로 필요한 역량

* ROS2
* Gazebo
* Robot Control
* Motion Planning
* Robot Learning

---

## 정리

ROS2는 로봇의 두뇌가 아니라 신경계에 가깝다.

AI 모델이 판단을 담당한다면 ROS2는 각 기능들이 서로 통신하고 협력하도록 연결해주는 역할을 수행한다.

로봇 개발자가 되기 위해서는 ROS2를 가장 먼저 익혀야 한다.
