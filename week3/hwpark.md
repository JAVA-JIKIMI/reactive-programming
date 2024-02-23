# 10. Scheduler


## Reactor에서 사용되는 Scheduler 
- Reactor Sequence에서 사용되는 스레드를 관리해 주는 관리자 역할
- Scheduler를 사용하여 어떤 스레드에서 무엇을 처리할지 제어 가능
- Scheduler를 사용하면 코드가 간결해지고 개발자가 스레드를 직접 제어해야하는 부담에서 벗어남


▶ 물리적인 스레드  
- 물리 스레드 = 4코어 8스레드의 스레드 = 논리적인 코어
- 물리 스레드는 병렬성(Parallelism)과 관련이 있다.
- 물리적인 스레드가 실제로 동시에 실행되기 때문에 여러 작업을 동시에 처리함

▶ 논리적인 스레드
- 소프트웨어적으로 생성되는 스레드 의미
- 동시성(Concurrency)와 관련이 있다.
- 동시성은 동시에 실행되는 것처럼 보이는 것

### Scheduler를 위한 전용 Operator
Reactor에서 Scheduler는 Scheduler 전용 Operator를 통해 사용할 수 있다.

### 사견
리액티브 프로그래밍은 실행할 장비의 CPU 물리 스레드 개수와 연관되어 있다.

## Scheduler의 종류
- Schedulers.immediate() : 별도의 스레드를 추가적으로 생성하지 않고, 현재 스레드에서 작업을 처리
- Schedulers.single() : 스레드 하나만 생성해서 Scheduler가 제거되기 전까지 재사용
- Schedulers.newSingle() : 호출할때마다 매번 새로운 스레드 하나를 생성
- Schedulers.boundedElastic() : Blocking I/O 작업에 최적화 
- Schedulers.parallel() : Non-Blocking I/O 작업에 최적화. CPU 코어 수만큼 스레드 생성
- Schedulers.newXXX() : 해당 메서드 사용하여 새로운 Scheduler 인스턴스를 생성 가능

# 11. Context
Context : '어떤 것을 이해하는데 도움이 될만한 관련 정보나 이벤트, 상황'
Context는 어떠한 상황에서 그 상황을 처리하기 위해 필요한 정보

1) ServletContext
   1) Servlet이 Setvlet Container와 통신하기 위해서 필요한 정보를 제공하는 인터페이스
2) Spring Framework에서 ApplicationContext
   1) 애플리케이션의 정보를 제공하는 인터페이스
3) Spring Security에서 SecurityContextHolder
   1) SecurityContext를 관리하는 주체인데, 여기서 SecurityContext는 애플리케이션 사용자의 인증 정보를 제공하는 인터페이스

즉, 프로그래밍 세계에서의 Context는 어떠한 상황을 처리하거나 해결하기 위해 "필요한 정보를 제공"하는 어떤 것이다.

### Reactor에서의 Context
```
A key/value store that is propagated between components such as operators via the context protocol
```
Operator 같은, Reactor 구성요소 간에 전파되는 key/value 형태의 저장소

- Context는 구독이 발생할 때마다 하나의 Context가 해당 구독에 연결된다.
- Context는 Operator 체인의 아래에서 위로 전파된다.
- 동일한 키에 대한 값을 중복해서 저장하면 Operator 체인의 위쪽의 값으로 Override된다.
- Inner Sequence 내부에서는 외부 Context에 저장된 데이터를 읽을 수 있다. 반대는 불가능.
- Context는 인증 정보 같은 직교성(독립성)을 가지는 정보를 전송하는데 적합하다.


# 12. Dubugging

### 1. Debug Mode를 활성화
Reactor Sequence를 디버깅 가능
```java
Hooks.onOperatorDebug();
```
#### 한계 
- 디버그 모드의 활성화는 애플리케이션 내에서 비용이 많이 드는 동작 과정을 거친다. 
- 따라서 에러 원인을 추적하기 위해 처음부터 디버그 모드를 활성화하는 것은 권장하지 않는다.
1. 애플리케이션 내에 있는 모든 Operator의 스택트레이스(Stacktrace)를 캡처한다.
2. 에러가 발생하면 캡처한 정보를 기반으로 에러가 발생한 Assembly(리턴하는 새로운 Mono 또는 Flux가 선언된 지점)의 스택트레이스를 원본 스택트레이스 중간에 끼워넣는다.  

로그 파일을 쌓는것도 I/O이고 blocking이 걸려서 운영환경에서는 로그 쌓는것도 조심스럽다.

### 2. checkpoint() Operator
특정 Operator 체인 내의 스택트레이스만 캡처하는 방식
- checkpoint()를 사용하면 실제 에러가 발생한 assembly 지점 또는 에러가 전파된 assembly 지점의 traceback이 추가된다.
- description도 출력 가능. checkpoint(String description, boolean forceStackTrace)
```java
@Slf4j
public class Example12_2 {
    public static void main(String[] args) {
        Flux
            .just(2, 4, 6, 8)
            .zipWith(Flux.just(1, 2, 3, 0), (x, y) -> x/y)
            .map(num -> num + 2)
            .checkpoint()
            .subscribe(
                    data -> log.info("# onNext: {}", data),
                    error -> log.error("# onError:", error)
            );
    }
}
```

```
...
Error has been observed at the following site(s):
	*__checkpoint() ⇢ at chapter12.Example12_2.main(Example12_2.java:17)
Original Stack Trace:
		at chapter12.Example12_2.lambda$main$0(Example12_2.java:15)
		at reactor.core.publisher.FluxZip$PairwiseZipper.apply(FluxZip.java:982)
...
```

### 3. log() Operator
Reactor Sequence 동작을 로그로 출력하여 디버깅
추가한 지점의 Reactor Signal을 출력
```java
.log()
.log("Fruit.Substring", Level.FINE) // FINE은 DEBUG를 의미
```

# Testing
io.projectreactor:reactor-test 모듈 활용

### StepVerifier
Reactor Sequence에서 발생하는 Signal 이벤트를 테스트 가능

### PublisherProve
Sequence의 실행이 분기되는 상황에서 Publisher가 어느 경로로 실행되는지 테스트 가능
- assertWasSubcribed()
- assertWasRequested()
- assertWasNotCancelled()
