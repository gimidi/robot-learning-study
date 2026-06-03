# ROS2 Architecture Notes

## 1. ROS2는 Python 라이브러리가 아니다

ROS2는 `pip install torch`처럼 설치해서 쓰는 단순 Python 라이브러리가 아니다.

ROS2는 로봇 시스템을 만들기 위한 미들웨어/플랫폼에 가깝다.

비유하면 Python 라이브러리보다는 Docker, Kubernetes처럼 하나의 실행 환경과 생태계를 가진 도구에 더 가깝다.

---

## 2. ROS2의 핵심 구현은 C++에 가깝다

ROS2는 Python으로만 만들어진 시스템이 아니다.

핵심 구현은 C++ 기반이고, Python에서는 `rclpy`라는 클라이언트 라이브러리를 통해 ROS2 기능을 사용할 수 있다.

PyTorch도 Python API를 사용하지만 내부 연산은 C++/CUDA에서 수행되는 것처럼, ROS2도 Python API 뒤쪽에는 C++ 기반의 ROS2 Core와 DDS가 있다.

---

## 3. rclpy와 rclcpp

`rclpy`는 ROS Client Library for Python의 의미이다.

Python에서 ROS2 Node, Topic, Service, Action 등을 사용하기 위한 라이브러리다.

C++ 버전은 `rclcpp`라고 부른다.

* `rclpy` = Python용 ROS2 클라이언트 라이브러리
* `rclcpp` = C++용 ROS2 클라이언트 라이브러리

---

## 4. ROS2는 HTTP 기반 통신이 아니다

웹 서비스에서는 보통 HTTP, WebSocket, REST API를 사용한다.

하지만 로봇 내부에서는 카메라, 센서, 제어기, AI 모델 등이 매우 빠르게 데이터를 주고받아야 한다.

예를 들어 카메라는 초당 30프레임 이상 이미지를 보내고, IMU나 모터 제어는 훨씬 더 높은 주기로 동작할 수 있다.

이런 통신을 HTTP 요청/응답 구조로 처리하면 비효율적이다.

그래서 ROS2는 Topic, Service, Action 같은 통신 방식을 사용한다.

---

## 5. DDS란?

DDS는 Data Distribution Service의 약자이다.

ROS2 아래에서 노드 간 통신을 담당하는 미들웨어 역할을 한다.

ROS2의 Publisher/Subscriber 구조는 DDS를 통해 동작한다.

비유하면 Kafka의 Producer/Consumer 구조와 비슷하지만, DDS는 로봇처럼 실시간성과 낮은 지연시간이 중요한 환경에 더 적합하다.

---

## 6. Publisher / Subscriber 구조

ROS2에서는 데이터를 보내는 쪽을 Publisher, 데이터를 받는 쪽을 Subscriber라고 한다.

예시:

```text
Camera Node
↓ publish
/camera/image
↓ subscribe
Object Detection Node
```

Camera Node는 이미지 데이터를 발행하고, Object Detection Node는 해당 Topic을 구독해서 이미지를 받는다.

---

## 7. ROS2와 Kafka의 차이

Kafka는 보통 Broker라는 중앙 서버를 통해 메시지를 전달한다.

ROS2의 DDS 기반 통신은 노드들이 같은 네트워크 안에서 서로를 자동으로 발견하고 통신할 수 있다.

즉 ROS2에서는 항상 HTTP 서버 주소나 포트를 직접 지정하는 방식이 아니다.

---

## 8. LAN과 WAN

LAN은 Local Area Network의 약자로, 집, 회사, 연구실, 로봇 내부 네트워크처럼 좁은 범위의 네트워크를 의미한다.

WAN은 Wide Area Network의 약자로, 인터넷처럼 넓은 범위의 네트워크를 의미한다.

DDS는 주로 같은 로봇 내부나 연구실 네트워크 같은 LAN 환경에 강하다.

HTTP는 인터넷 전체를 대상으로 하는 WAN 환경에 강하다.

따라서 DDS가 HTTP의 상위호환인 것은 아니다.

둘은 해결하는 문제가 다르다.

* HTTP = 인터넷 서비스에 적합
* DDS = 로봇 내부 실시간 통신에 적합

---

## 9. ROS2는 HTTP의 다음 세대인가?

ROS2/DDS가 HTTP의 상위호환은 아니다.

HTTP는 웹, 앱, 클라우드 서비스에 매우 적합하다.

ROS2/DDS는 로봇 내부의 센서, AI, 제어기 간 통신에 적합하다.

즉 둘은 경쟁 관계라기보다 사용 영역이 다르다.

로봇 시스템에서도 외부 클라우드 API를 호출할 때는 HTTP를 사용할 수 있고, 로봇 내부 통신에는 ROS2를 사용할 수 있다.

---

## 10. LLM API와 로봇

휴머노이드 로봇에서도 GPT, Claude 같은 LLM API는 고수준 계획자로 사용할 수 있다.

예를 들어 사용자가 “주방에 가서 빨간 컵 가져와”라고 말하면 LLM은 이를 다음과 같은 작업으로 분해할 수 있다.

```text
1. 주방으로 이동
2. 빨간 컵 찾기
3. 컵 집기
4. 사용자에게 전달
```

하지만 LLM API가 직접 모터를 제어하는 것은 아니다.

실제 행동은 ROS2, VLA, Motion Planning, Controller가 담당한다.

---

## 11. 로봇 시스템에서 각 계층의 역할

```text
사용자 명령
↓
LLM / GPT / Claude
: 고수준 계획, 작업 분해
↓
VLM / VLA
: 시각 이해, 행동 정책
↓
ROS2
: 노드 간 통신, 실행 연결
↓
Controller
: 실제 로봇팔, 모터, 이동 제어
```

---

## 12. 내가 관심 있는 방향

내가 잘 맞을 가능성이 높은 영역은 모터 제어 자체보다는 Robot Brain, Task Planning, VLM/VLA, LLM 기반 로봇 계획 쪽이다.

기존에 재미있게 했던 간호 스케줄링 엔진도 구조적으로 보면 상태, 제약조건, 계획, 행동을 만드는 문제였다.

로봇의 Task Planning도 비슷하게 상태를 이해하고 목표를 달성하기 위한 행동을 계획하는 문제이다.

---

## 13. 현재 이해한 핵심

ROS2는 로봇의 두뇌라기보다 신경계에 가깝다.

AI 모델이 판단과 계획을 담당한다면, ROS2는 각 기능들이 서로 통신하고 실행될 수 있도록 연결한다.

내가 앞으로 배워야 할 것은 다음과 같다.

* ROS2 기본 구조
* Node / Topic / Service / Action
* rclpy
* rclcpp
* DDS
* Robot Control 기초
* VLA / Task Planning
