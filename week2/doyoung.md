[notion](https://xavy.notion.site/202401-fdd758dacfef4f62b8d3bc41b94e7ba1?pvs=4)


### 마블 다이어그램

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/639e72b4-dcc1-4ad6-894c-6180a5653a03/20cdfe05-1fe8-461d-8e5b-014e99ebd0cd/Untitled.png)

126p

### Cold&Hot Sequence

- Cold sequence: subscriber의 구독 시점이 달라도 구독할 때 마다 publisher가 데이터를 emit하는 과정을 처음부터 다시 시작하는 데이터의 흐름
- Hot sequence: 구독한 시점의 타임라인부터 emit된 데이터를 받음
    - cold sequence에 .share() 함수를 이용하면 생성할 수 있음

```kotlin
Flux.fromArray(arrayOf("Metallica", "The Cure", "The Beatles"))
        .delayElements(Duration.ofSeconds(1)) // use parallel thread pool
        .share() // change to hot sequence
        .also { it.subscribe { println("Subscriber 1: $it") } }
        .also { Thread.sleep(2000) }
        .also { it.subscribe { println("Subscriber 2: $it") } }
        .also { Thread.sleep(3000) }
```

```kotlin
[parallel-1] Subscriber 1: Metallica
[parallel-2] Subscriber 1: The Cure
[parallel-2] Subscriber 2: The Cure
[parallel-3] Subscriber 1: The Beatles
[parallel-3] Subscriber 2: The Beatles
```

- hot sequence 구분
    - warm up: Subscriber의 최초 구독이 발생해야 Publisher가 데이터를 emit
    - hot: Subscriber의 구독 여부와 상관없이 데이터를 emit

157p

### Backpressure

Publisher가 emit 하는 데이터를 적절하게 제어하여 데이터 처리에 있어 과부하가 걸리지않도록 제어

- 데이터 요청 개수를 제어하는 방식
- Backpressure 전략을 사용하는 방식
    - IGNORE, ERROR, DROP, LATEST, BUFFER(DROP_LATEST, DROP_OLDEST)

186p

## Sinks

Publisher, Subscriber의 기능을 모두 지니는 Processor의 개선버전. Processor는 제거될 예정.

### mono.just vs sink.mono

```kotlin
// mono.just
Mono<String> mono = Mono.just("Hello, Mono!");

// sinks.mono
Sinks.MonoSink<String> monoSink = Sinks.mono();
monoSink.success("Hello, Sink Mono!");
```

⇒ **Mono.just**는 이미 값을 알고 있을 때 사용하고, **Sinks.mono**는 나중에 값을 동적으로 제공하고 조작해야 할 때 사용

### 그 외

- sinks는 thread safe
- flux와 대비대는 것은 Sinks.many
- Sinks.one은 일회용으로 사용할 수 있는 mono 스트림을 생성

198p

## Scheduler

### subscribeOn

- 구독 전용 쓰레드를 설정
- reactor는 기본적으로 scheduler를 변경하지 않으면 같은 쓰레드에서 실행됨
- upstream으로 signal을 전송할 때 실행되는 스레드를 제어(subscribe)

### publisherOn

- downstream으로 signal을 전송할 때 실행되는 스레드를 제어(onNext)

<aside>
💡 - Reactor진영에서는 Scheduler를 통해 로직이 실행 될 Thread 환경을 지정할 수 있다.

- 로직의 실행 Thread에 대한 주도권을 가질 땐 PublishOn, 로직을 subscribe할 때 default를 지정 할 목적이면 subscribeOn

- Webflux에서 I/O등 시간이 걸리는 작업은 scheduler를 지정하여(elastic 추천) 수행

</aside>

[출처](https://p-bear.tistory.com/68)