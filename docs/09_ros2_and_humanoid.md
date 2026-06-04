# ROS2 and Humanoid

## 목적

휴머노이드 로봇 내부에서 ROS2가 어떤 역할을 하는지 이해한다.

---

## 처음 가졌던 생각

처음에는 휴머노이드가 GPT나 Claude 같은 LLM 하나로 동작한다고 생각했다.

예시

```text
사용자

↓

GPT

↓

로봇 행동
```

---

하지만 실제 구조는 훨씬 복잡하다.

---

## 휴머노이드 내부 구조

```text
사용자

↓

음성

↓

Speech Recognition

↓

LLM

↓

Task Planning

↓

ROS2

↓

Controller

↓

Motor

↓

실제 행동
```

---

ROS2는 로봇의 두뇌가 아니다.

각 기능을 연결하는 신경계 역할에 가깝다.

---

## Camera 입력

예시

```text
사용자

↓

"컵 가져와"
```

---

로봇은 먼저 주변 환경을 확인해야 한다.

```text
Camera

↓

VLM

↓

컵 위치 인식
```

---

VLM은

Vision Language Model

의 약자이다.

---

역할

```text
무엇이 보이는가?
```

판단.

---

예시

```text
컵이 테이블 위에 있다.
```

---

## LLM의 역할

LLM은 행동 계획을 만든다.

예시

```text
컵 가져와
```

↓

```text
1. 컵 찾기

2. 컵 위치 이동

3. 컵 집기

4. 사용자에게 전달
```

---

즉

LLM은 행동 자체가 아니라 계획을 담당한다.

---

## ROS2의 역할

계획이 만들어지면

ROS2가 실제 시스템을 연결한다.

---

예시

```text
Planner Node

↓

Navigation Node

↓

Arm Controller Node

↓

Gripper Node
```

---

ROS2는

각 Node들이 서로 통신하도록 만든다.

---

## Controller의 역할

Controller는 실제 모터를 움직인다.

예시

```text
팔 각도 변경

손가락 닫기

보행
```

---

이 영역은 보통 C++ 비중이 높다.

---

## 휴머노이드 전체 구조

```text
Camera

↓

VLM

↓

LLM

↓

Task Planner

↓

ROS2

↓

Controller

↓

Motor
```

---

## GPT와 Claude는 어디에 위치할까?

GPT, Claude 같은 모델은

상위 계획자(High-Level Planner) 역할을 수행할 수 있다.

---

예시

```text
사용자

↓

GPT

↓

작업 분해

↓

ROS2
```

---

하지만 GPT가 직접 모터를 제어하지는 않는다.

---

## 왜 GPT만으로는 부족할까?

이유

```text
실시간성

지연시간

안전성
```

---

예시

```text
컵 잡기
```

과 같은 작업은

수 ms 단위 제어가 필요하다.

---

GPT API 호출만으로는 어렵다.

---

## 내가 잘 맞을 가능성이 높은 영역

현재 나의 강점

```text
LLM

NLP

시스템 설계

서비스 개발

Task Planning
```

---

부족한 영역

```text
ROS2

Robot Control

Motion Planning

C++
```

---

따라서 현재 전략은

기존 AI 역량을 버리는 것이 아니라,

ROS2와 Robotics Layer를 추가하는 방향이 적절하다.

---

## 간호 스케줄링과의 공통점

간호 스케줄링 엔진도

```text
현재 상태

↓

제약조건

↓

계획

↓

행동 생성
```

구조였다.

---

휴머노이드도

```text
현재 상태

↓

목표

↓

계획

↓

행동
```

구조를 가진다.

---

생각보다 두 문제는 비슷한 철학을 공유한다.

---

## 내가 이해한 핵심

휴머노이드는 단순히 AI 모델 하나가 움직이는 시스템이 아니다.

여러 개의 전문 Node가 협력하는 분산 시스템이다.

ROS2는 그 Node들을 연결하는 신경계 역할을 수행한다.

현재 나의 강점은 Planning과 AI 영역에 있으며,

앞으로 학습해야 할 영역은 ROS2와 Robot Control 영역이다.
