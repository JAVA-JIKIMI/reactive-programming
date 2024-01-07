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

## Kafka와의 차이점
Kafka와 다르게 리액티브 스트림즈는 Publisher가 subscribe 메서드를 구현.
- Kafka는 Message Broker가 있지만 리액티브 스트림즈는 없음.

```java
public interface Publisher<T> {
    public void subscribe(Subscriber<? super T> s);
}
```
```java
public interface Subscriber<T> {
    public void onSubscribe(Subscription s);
    public void onNext(T t);
    public void onError(Throwable t);
    public void onComplete();
}
```
```java
public interface Subscription {
    public void request(long n);
    public void cancel();
}
```
** p.45 의 내용 다시 읽어보기
```java
public interface Processor<T, R> extends Subscriber<T>, Publisher<R> {
}
```

### 리액티브 스트림즈 용어 정의
- Signal : 신호. P와 S간에 주고받는 상호작용. (ex: onSubscribe, onNext, onComplete, onError, request, cancel,...)
- Demand : 데이터 요청
- Emit : 데이터 방출(제공)
- Upstream/Downstream : Flux로 구성된 메서드 체인 중 상하위 체인의 데이터
- Sequence : Publisher가 emit하는 데이터의 연속적인 흐름. 다양한 Operator로 데이터의 연속적인 흐름을 정의
- Operator : 연산자. just, filter, map같은 메서드를 지칭
- Source : (=Original) 최초에 생성된 무언가. 원본.

### 리액티브 스트림즈 구현 규칙
pass

### 리액티브 스트림즈 구현체
어떤 구현제를 사용하든 핵심 동작 원리는 같다.

** 사용상의 장단점을 googling하면 좋을거 같음
#### RxJava
- .NET 환경의 리액티브 확장 라이브러리를 넷플릭스에서 Java로 포팅. 
- 1.0 : Reactive Streams 사양 미지원.
- 2.0 : 리액티브 스트림즈 사양 지원. 때문에 2버전에서는 1.0과 2.0이 혼재. Java 9 지원.
- 3.0 : Reactive Streams 1.0.3 버전을 준수. 2버전에서 (최신 버전 3.1.8) 

### (Project) Reactor
- Spring Framework 팀 개발.
- 최신 버전 3.6.1. Spring Framework 5점대부터 지원.

### Akka Streams
- JVM상에서 동시성과 분산 어플리케이션을 단순화해 주는 오픈소스 툴킷
- Actor : Akka의 핵심. 
- Akka라는 Actor 기반의 동시성 모델을 사용하는 툴킷 위에 리액티브 스트림즈를 구현한 것이 Akka Streams

### Java Flow API
- Java 9부터 Flow API를 사용하여 리액티브 스트림즈 지원
- 다른 구현제들과의 차이점 : Java API에서 구현체가 아니라 SPI(Service Provider Interface)로만 정의되어 있음.

### 그 외 리액티브 Extension
- Android 플랫폼의 RxAndroid, Javascript의 RxJs, ...


