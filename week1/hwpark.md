# 1. 리액티브 시스템과 리액티브 프로그래밍

## 리액티브 시스템의 정의
Reactive : 반응을 잘하는
Reactive System : 반응을 잘하는 시스템.
클라이언트의 요청에 즉각적으로 대응함으로써 지연 시간을 최소화.

## 리액티브 선언문(Reactive Manifesto) 설계 원칙

```빠른 응답성을 바탕으로 유지보수와 확장이 용이한 시스템```

![image](https://reactivemanifesto.org/images/reactive-traits.svg)
(참고 : https://reactivemanifesto.org/)

Means : 수단. 리액티브 시스템의 주요 통신 수단으로, 비동기 메시지 기반의 통신 활용

Form :
- Elastic : 탄력성. 작업량 변화에 따라 탄력적으로 자원을 확대,축소할 수 있음. -> 응답성을 일정하게 유지
- Resilient : 회복성. 장애가 발생해도 응답성을 유지. 느슨한 결합, 격리성으로 장애가 발생해도 발생한 부분만 복구하면 되도록.

Value : 응답성. 리액티브 시스템의 핵심 가치.

## 리액티브 프로그래밍 개념
리액티브 시스템을 구축하는데 필요한 프로그래밍 모델
비동기 Non-Blocking 통신을 위한 프로그래밍 모델.

## 리액티브 프로그래밍의 특징
```
In computing, reactive programming is a 
declarative programming paradigm concerened with 
data streams and the propagation of change.
```

- declarative programming : 선언형.
- data streams : 데이터가 지속적으로 발생
- the propagation of change : 지속적으로 데이터가 발생할 때마다 이것을 변화하는 이벤트로 보고,
  이 이벤트를 발생시키면서 데이터를 계속적으로 전달

## 명령형 프로그래밍 vs 선언형 프로그래밍
선언형 프로그래밍은 동작을 구체적으로 명시하지 않고 목표만 선언.
코드가 간결해지고 가독성이 좋아짐.

## 리액티브 프로그래밍 코드 구성

- Publisher : 발행인, 발행자. 입력으로 들어오는 데이터를 제공하는 역할.
- Subscriber : 구독자. Publisher가 제공한 데이터를 전달받아서 사용하는 주체.
- Data Source : Publisher의 입력으로 들어오는 데이터. (= Data Stream)
- Operator : Publisher, Subscriber 사이에서 데이터를 가공. 데이터 생성, 필터링, 변환 등..


![image](https://github.com/JAVA-JIKIMI/reactive-programming/assets/42948301/d63f50b4-c5d3-4c2f-b1ba-028a8247485f)
