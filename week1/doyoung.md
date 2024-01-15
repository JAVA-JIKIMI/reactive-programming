# [202401] 스프링으로 시작하는 리액티브 프로그래밍

# 리액티브 시스템과 리액티브 프로그래밍

- MEANS: 비동기 메시지 기반의 통신을 통해 구성요소들 간의 느슨한 결합, 격리성, 위치 투명성을 보장
- FORM: 비동기 메시지 통신 기반하에 탄력성과 회복성을 가져야한다는 것을 보여줌
    - 탄력성: 시스템에 유입되는 입력이 많던 적던 시스템에서 요구하는 응답성을 일정하게 유지
    - 회복성: 장애가 발생하더라도 응답성을 유지

<aside>
💡 리액티브 시스템: 빠른 응답성을 바탕으로 유지보수와 확장이 용이한 시스템
리액티브 프로그래밍: Reactive programming is a **declarative programming paradigm** concerned with **data streams** and the **propagation of change**

</aside>

28p

# 리액티브 스트림즈

데이터 스트림을 non-blocking 이면서 비동기적인 방식으로 처리하기 위한 리액티브 라이브러리의 표준 사양

### 리액티브 스트림즈의 pub/sub과 카프카의 pub/sub 차이

카프카는 중간에 브로커를 통해 서로를 바라보는 것이 아닌 특정 토픽을 발행 및 구독하는 느슨한 결합 구조를 이루어짐

반면 리액티브 스트림즈는 개념상으로 Subscriber가 Producer를 구독하는 게 맞지만, 실제 코드상으로는 Publisher가 subscribe 메소드의 파라미터인 Subscriber를 등록하는 형태로 구독이 이루어 짐

```kotlin
interface Publisher<T> {
	fun subscribe(s: Subscriber<in T> s)
}
```

### Publisher 구현을 위한 주요 기본 규칙

1. Publisher가 Subscriber에게 보내는 onNext signal의 총 개수는 항상 해당 Subscriber의 구독을 통해 요청된 데이터의 총 개수보다 더작거나 같아야 한다.
2. Publisher는 요청된 것보다 적은 수의 onNext signal을 보내고 onComplete 또는 onError를 호출하여 구독을 종료할 수 있다.
3. Publisher의 데이터 처리가 실패하면 onError signal을 보내야 한다.
4. Publisher의 데이터 처리가 성공적으로 종료되면 onComplete signal을 보내야 한다.
5. Publisher가 Subscriber에게 onError 또는 onComplete signal을 보내는 경우 해당 Subscriber의 구독은 취소된 것으로 간주해야 한다.
6. 일단 종료 상태 signal을 받으면(onError, onComplete) 더 이상 signal이 발생되지 않아야 한다.
7. 구독이 취소되면 Subscriber는 결국 signal을 받는 것을 중지해야 한다.

### Subscriber 구현을 위한 주요 기본 규칙

1. Subscriber는 Publisher로부터 onNext signal을 수신하기 위해 Subscription.request(n)을 통해 Demand signal을 Publisher에게 보내야 한다.
2. Subscriber.onComplete() 및 Subscriber.onError(Throwable t)는 Subscription 또는 Publisher의 메소드를 호출해서는 안된다.
3. Subscriber.onComplete() 및 Subscriber.onError(Throwable t)는 signal을 수신한 후 구독이 취소된 것으로 간주해야 한다.
4. 구독이 더 이상 필요하지 않은 경우 Subscriber는 Subscription.cancel()을 호출해야 한다.
5. Subscriber.onSubscriber()는 지정된 Subscriber에 대해 최대 한 번만 호출되어야 한다.

### Subscription 구현을 위한 주요 기본 규칙

1. 구독은 Subscriber가 onNext 또는 onSubscribe 내에서 동기적으로 Subscription.request를 호출하도록 허용해야 한다.
2. 구독이 취소된 후 추가적으로 호출되는 Subscription.request(long n)은 효력이 없어야 한다.
3. 구독이 취소된 후 추가적으로 호출되는 Subscription.cancel()은 효력이 없어야 한다.
4. 구독이 취소되지 않은 동안 Subscription.request(long n)의 매개변수가 0보다 작거나 같은면 java.lang.illegalArgumentException과 함께 onError signal을 보내야 한다.
5. 구독이 취소되지 않은 동안 Subscription.cancel()은 Publisher가 Subscriber에게 보내는 signal을 결국 중지하도록 요청해야 한다.
6. 구독이 취소되지 않은 동안 Subscription.cancel()은 Publisher에게 해당 구독자에 대한 참조를 결국 삭제하도록 요청해야 한다.
7. Subscription.cancel(), Subscription.request() 호출에 대한 응답으로 예외를 던지는 것을 허용하지 않는다.
8. 구독은 무제한 수의 request 호출을 지원해야 하고 최대 2^63-1 개의 Demand를 지원해야 한다.