# ROS2 Publisher / Subscriber

## 목적

ROS2에서 가장 핵심적인 통신 방식인 Publisher / Subscriber 구조를 이해한다.

---

## 왜 필요한가?

로봇은 여러 기능이 동시에 동작한다.

예를 들어 휴머노이드 로봇은

* 카메라
* 음성 인식
* 물체 인식
* 경로 계획
* 로봇팔 제어

등이 동시에 실행된다.

이러한 기능들은 서로 데이터를 주고받아야 한다.

ROS2는 Publisher / Subscriber 구조를 통해 이를 해결한다.

---

## Node

ROS2에서 Node는 하나의 기능을 수행하는 프로그램이다.

예시

* Camera Node
* VLM Node
* Planner Node
* Robot Arm Controller Node

웹 개발 관점에서는 하나의 서비스(Microservice)와 비슷하게 생각할 수 있다.

---

## Publisher

Publisher는 데이터를 발행(Publish)하는 Node이다.

예시

```text
Camera Node
```

카메라는 계속 이미지를 생성한다.

```text
Image

Image

Image

Image
```

이 데이터를 Topic으로 발행한다.

---

## Subscriber

Subscriber는 데이터를 구독(Subscribe)하는 Node이다.

예시

```text
Object Detection Node
```

Object Detection Node는 Camera가 보내는 이미지를 받아서 물체를 인식한다.

---

## Topic

Topic은 Publisher와 Subscriber를 연결하는 통신 채널이다.

예시

```text
/camera/image
```

Camera Node는 해당 Topic에 이미지를 발행하고,

Object Detection Node는 해당 Topic을 구독한다.

---

## 전체 구조

```text
Camera Node

(Publisher)

↓

/camera/image

↓

Object Detection Node

(Subscriber)
```

---

## Subscriber는 여러 개가 가능하다

ROS2의 중요한 특징 중 하나는 하나의 Publisher에 여러 Subscriber가 동시에 연결될 수 있다는 점이다.

예시

```text
Camera Node

↓

/camera/image

↙      ↓      ↘

VLM   Planner   Recorder
```

Camera는 이미지를 한 번만 발행한다.

하지만 여러 Node가 동시에 같은 데이터를 받을 수 있다.

---

## 웹 서비스와의 차이

### 웹 서비스

```text
Client

↓

Request

↓

Server

↓

Response
```

요청 후 응답을 기다리는 구조이다.

---

### ROS2 Topic

```text
Publisher

↓

Topic

↓

Subscriber
```

Publisher는 데이터를 계속 발행한다.

Subscriber는 관심 있는 Topic을 구독한다.

응답을 기다리는 구조가 아니다.

---

## 방송 시스템 비유

ROS2 Topic은 방송 시스템과 비슷하다.

```text
유튜브 방송

↓

채널

↓

구독자들
```

방송자는 구독자가 몇 명인지 몰라도 방송을 계속한다.

구독자는 관심 있는 채널만 구독한다.

ROS2 Topic도 같은 철학을 가진다.

---

## RabbitMQ와의 비교

RabbitMQ를 사용해본 경험 기준으로 보면

```text
Producer
≈
Publisher

Consumer
≈
Subscriber
```

와 유사한 개념으로 이해할 수 있다.

다만 ROS2는 로봇 시스템에 특화되어 있으며, DDS 기반 통신을 사용한다.

---

## 왜 이름이 Topic일까?

처음에는 Topic이라는 이름이 직관적으로 느껴지지 않았다.

Service는 요청/응답 서비스,

Action은 작업(Action)

이라는 의미가 바로 이해되는데, Topic은 왜 Topic인지 궁금했다.

---

ROS2의 Topic은 "대화 주제(Topic)"라는 의미에 가깝다.

예를 들어 사람들이 대화한다고 가정하자.

```text
Topic = 날씨

민지
영희
철수
```

날씨라는 주제에 관심 있는 사람들만 해당 대화를 듣는다.

---

ROS2도 동일한 철학을 가진다.

예를 들어

```text
/camera/image
```

라는 Topic이 있다면

```text
Object Detection Node

VLM Node

Recorder Node
```

등이 해당 Topic을 구독할 수 있다.

---

즉 Topic은

```text
데이터의 주제
```

를 의미한다.

---

예시

```text
/camera/image

/robot_pose

/battery_status

/imu
```

이들은 모두 특정 데이터 종류를 나타낸다.

---

반면 Service와 Action은 데이터 자체보다 동작에 가깝다.

### Topic

무슨 데이터가 흐르는가?

예시

```text
/camera/image
/battery_status
```

---

### Service

무슨 요청을 하는가?

예시

```text
현재 배터리 상태 알려줘
현재 위치 알려줘
```

---

### Action

무슨 작업을 수행하는가?

예시

```text
컵 가져오기
방 이동하기
문 열기
```

---

따라서

```text
Publisher
Subscriber
Topic
```

을 합치면

"특정 주제(Topic)에 대해 데이터를 발행(Publish)하고 구독(Subscribe)한다."

라는 의미가 된다.

---

## 내가 이해한 비유

Publisher

```text
방송국
```

---

Topic

```text
방송 채널
```

---

Subscriber

```text
시청자
```

---

예시

```text
Camera Node

↓

/camera/image

↓

Object Detection Node

VLM Node

Recorder Node
```

Camera Node는 카메라 영상이라는 주제에 대해 방송을 하고,

다른 Node들은 해당 주제를 구독하여 데이터를 받는다.

---

### My Thoughts

웹 서비스에서는 API Endpoint를 중심으로 생각했기 때문에 처음에는 Topic이라는 이름이 낯설었다.

하지만 Topic은 "행동"이 아니라 "데이터 주제"를 의미한다는 점을 이해하니 Publisher / Subscriber 구조가 훨씬 자연스럽게 느껴졌다.

---

## 로봇에서의 예시

```text
Camera Node

↓

/camera/image

↓

VLM Node

↓

/object_info

↓

Planner Node

↓

/robot_action

↓

Robot Arm Controller Node
```

각 Node는 자신의 역할에만 집중한다.

ROS2는 이 Node들을 연결하는 역할을 수행한다.

---

## 내가 이해한 핵심

ROS2의 Publisher / Subscriber 구조는 웹 서비스의 Request / Response 구조와 다르다.

웹 서비스가 질문과 답변이라면,

ROS2 Topic은 방송과 구독에 가깝다.

Publisher는 데이터를 계속 발행하고,

Subscriber는 관심 있는 Topic을 구독한다.

ROS2는 이러한 구조를 통해 여러 기능을 느슨하게 연결하고 확장 가능하게 만든다.

---

## My Thoughts

ROS2는 생각보다 웹 서비스와 완전히 다른 세계는 아니다.

오히려 여러 컴포넌트를 연결하는 메시지 기반 아키텍처라는 점에서 RabbitMQ와 비슷한 철학을 가진다.

다만 ROS2는 실시간성과 로봇 제어를 고려하여 설계된 플랫폼이며, Publisher / Subscriber는 그 핵심 통신 모델이다.
