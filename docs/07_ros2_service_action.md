# ROS2 Service and Action

## 목적

Publisher / Subscriber 구조만으로 해결할 수 없는 문제를 이해하고, ROS2의 Service와 Action 개념을 학습한다.

---

## Topic의 한계

Topic은 방송 시스템과 비슷하다.

```text
Publisher

↓

Topic

↓

Subscriber
```

Publisher는 데이터를 계속 발행한다.

Subscriber는 관심 있는 데이터를 계속 수신한다.

---

하지만 다음과 같은 경우에는 Topic만으로 부족하다.

예시

```text
현재 배터리 잔량 알려줘
```

이 경우에는

```text
요청

↓

응답
```

이 필요하다.

---

## Service란?

Service는 Request / Response 구조를 제공한다.

웹 서비스의 API 호출과 가장 비슷한 개념이다.

---

## 구조

```text
Client

↓

Request

↓

Service Server

↓

Response
```

---

## 예시

```text
배터리 상태 알려줘
```

---

Request

```json
{
  "request": "battery_status"
}
```

---

Response

```json
{
  "battery": 87
}
```

---

## 웹 서비스와 비교

FastAPI

```python
POST /battery
```

↓

응답

```json
{
  "battery":87
}
```

---

ROS2 Service

```text
Request

↓

Response
```

---

철학은 거의 동일하다.

---

## 언제 Service를 사용할까?

짧고 빠르게 끝나는 작업

예시

```text
현재 위치 알려줘

현재 배터리 알려줘

현재 온도 알려줘
```

---

## Service의 한계

문제가 하나 있다.

작업 시간이 오래 걸리는 경우.

예시

```text
주방 가서 컵 가져와
```

---

이 작업은

```text
5초

10초

30초
```

걸릴 수 있다.

---

Service는

```text
요청

↓

기다림

↓

응답
```

구조이므로 적합하지 않다.

---

## Action이란?

Action은 오래 걸리는 작업을 처리하기 위한 구조이다.

---

## 구조

```text
Goal

↓

실행

↓

진행상황(Feedback)

↓

결과(Result)
```

---

## 예시

사용자

```text
주방 가서 컵 가져와
```

---

Goal

```json
{
  "target":"cup"
}
```

---

Feedback

```text
주방 이동 중

컵 탐색 중

컵 집는 중
```

---

Result

```text
컵 전달 완료
```

---

## 왜 Action이 필요한가?

Service

```text
요청

↓

기다림

↓

결과
```

---

Action

```text
요청

↓

진행상황 확인 가능

↓

완료
```

---

로봇에서는 대부분의 행동이 시간이 걸린다.

그래서 Action 사용 빈도가 높다.

---

## 휴머노이드 예시

사용자

```text
"물 좀 가져와"
```

---

LLM

↓

작업 분해

↓

ROS2 Action

↓

Navigation

↓

Object Detection

↓

Arm Controller

↓

결과

---

## Topic / Service / Action 비교

### Topic

목적

데이터 스트리밍

예시

```text
카메라 영상

센서 데이터
```

---

구조

```text
Publish

↓

Subscribe
```

---

### Service

목적

짧은 요청/응답

예시

```text
배터리 상태 조회

현재 위치 조회
```

---

구조

```text
Request

↓

Response
```

---

### Action

목적

오래 걸리는 작업

예시

```text
컵 가져오기

방 이동하기

문 열기
```

---

구조

```text
Goal

↓

Feedback

↓

Result
```

---

## 내가 이해한 비유

Topic

```text
유튜브 방송
```

---

Service

```text
전화 통화
```

---

Action

```text
배달 주문
```

---

주문

↓

배달 출발

↓

배달 중

↓

배달 완료

---

## My Thoughts

ROS2는 단순히 Publisher / Subscriber만 있는 시스템이 아니다.

상황에 따라 Topic, Service, Action을 구분해서 사용한다.

특히 휴머노이드 로봇에서는 Action이 매우 중요해 보인다.

왜냐하면 대부분의 행동이 수 초 이상 걸리며, 중간 진행 상황을 확인할 필요가 있기 때문이다.

현재 내가 이해한 바로는

* Topic = 데이터 스트림
* Service = API 호출
* Action = 장기 실행 작업

으로 정리할 수 있다.
