# 📌 인상 깊었던 내용

**📚 Context의 정의**

> Context : 어떠한 상황에서 그 상황을 처리하기 위해 필요한 정보
> - ServletContext : Servlet이 Servlet Container와 통신하기 위해 필요한 정보를 제공하는 인터페이스
> - ApplicationContext(Spring Framework) : 애플리케이션의 정보를 제공하는 인터페이스
> - SecurityContextHolder(Spring Security) : SecurityContext를 관리하는 주체로, SecurityContext는 애플리케이션 사용자의 인증 정보를 제공하는 인터페이스
> 
> - Reactor에서의 Context
>   : Operator 체인에 전파되는 키와 값 형태의 저장소
>   : Subscriber와 매핑되어 구독이 발생할 때마다 해당 구독과 연결된 하나의 Context가 생김
>   : contextWriter()를 사용해서 Context에 데이터 쓰기 작업을 할 수 있다
>   : deferContextual()을 사용해서 원본 데이터 소스 레벨에서 Context의 데이터를 읽는다
>   : transformDeferredContextual()을 사용해서 Operator 체인의 중간에서 데이터를 읽는다
> 
> 📕 237p 11번째 (11장)
>

**🧐 : Context라는 단어를 자주 사용하는데 용어에 대한 정의를 알기에 용이하다**

**📚 디버깅 방법**

> - Hooks.onOperatorDebug() : 디버그 모드 활성화
> - checkpoint() : 실제 에러가 발생한 assembly 지점 또는 에러가 전파된 assembly 지점의 traceback이 추가된다(forceStackTrace = true로 하면, description, Traceback을 모두 출력할 수 있다)
> - log() : Reactor Sequence의 동작을 로그로 출력하는데, 이 로그를 통해 디버깅이 가능하다
> - Intellij의 Reactor Sequence 디버깅 지원 : Reactive Streams를 선택해서 Hoos.onOperatorDebug()를 선택한 후, OK를 클릭한다
> 
> 📕 274p 1번째 (12장)
>

**🧐 : 디버깅 활용방안 숙지해두자**

**📚 StepVerifier**

```jsx
public class StepVerifierGeneralExample01Test {
	@Test
	public void sayHelloReactorTest() {
		StepVerifier
			.create(Mono.just("Hello Reactor") // 테스트 대상 Sequence 생성
			.expectNext("Hello Reactor") // emit된 데이터 기댓값 평가
			.expectComplete() // onComplete Signal 기댓값 평가
			.verify(); // 검증 실행
	}
}
```

> StepVerifier는 구독 시점에 해당 Operator 체인이 시나리오대로 동작하는지를 테스트한다
>
> 📕 276p 1번째 (13장)

**🧐 : 나중에 유용할 듯..?**



**📚 TestPublisher**

```jsx
public void divideByTwoTest() {
    TestPublisher<Integer> source = TestPublisher.create(); // TestPublisher 생성
    
    StepVerifier
        .create(GeneralTestExample.divideByTwo(source.flux())) // 파라미터를 Flux로 전달하기 위함
        .expectSubscription()
        .then(() -> source.emit(2, 4, 6, 8, 10)) // 테스트에 필요한 데이터를 emit 한다
        .expectNext(1, 2, 3, 4)
        .expectError()
        .verify();
    }
}
```

> TestPublisher를 사용하면, 개발자가 직접 프로그래밍 방식으로 Signal을 발생시키면서 원하는 상황을 미세하게 재연하며 테스트를 진행할 수 있다
> 
> 📕 300p 4번째 (13장)
>

**🧐 : 데이터를 만들때 유용하게 사용할수 있을것 같다**

---

# 📌 이해가 가지 않았던 내용

**📚 Context 흐름**

```jsx
public static void main(String[] args) throws InterruptedException {
    String key1 = "company";
    String key2 = "name";

    Mono
        .deferContextual(ctx ->
            Mono.just(ctx.get(key1))
        )
        .publishOn(Schedulers.parallel())
        .contextWrite(context -> context.put(key2, "Bill"))
        .transformDeferredContextual((mono, ctx) ->
                mono.map(data -> data + ", " + ctx.getOrDefault(key2, "Steve"))
        )
        .contextWrite(context -> context.put(key1, "Apple"))
        .subscribe(data -> log.info("# onNext: {}", data));

    Thread.sleep(100L);
}

// 출력
# onNext: Apple, Steve
```

> Context의 경우, Operator 체인상의 아래에서 위로 전파되는 특징이 있다
> 이런 결과가 나오는 이유는 Context의 경우, Operator 체인상의 아래에서 위로 전파되는 특징이 있다.
> ContextView 객체를 통해 ‘name’이라는 키로 값을 읽어 오고 있지만 이 시점에는 ‘name’ 키에 해당되는 값이 Context에 없다. 따라서 getOrDefault() API의 디폴트 값으로 지정한 ‘Steve’가 Subscriber에게 전달되는 것이다
> 만약 14번 라인의 getOrDefault()를 get()으로 바꿔서 조회하게 되면 Context에 ‘name’ 키가 존재하지 않기 때문에 NoSuchElementException이 발생하게 된다
> 
> 📕 250p 9번째 (11장)
>

**🧐 : 아래에서 위로 흐르면 ‘Steve’로 설정되고 ‘Bill’로 업데이트되어야 하는게 아닐까..?**

---

# 📌 논의해보고 싶었던 내용

**📚 PublisherProbe**

```jsx
public class PublisherProbeTestExample {
    public static Mono<String> processTask(Mono<String> main, Mono<String> standby) {
        return main
                .flatMap(massage -> Mono.just(massage))
                .switchIfEmpty(standby);
    }

    public static Mono<String> supplyMainPower() {
        return Mono.empty();
    }

    public static Mono supplyStandbyPower() {
        return Mono.just("# supply Standby Power");
    }
}

@Test
public void publisherProbeTest() {
    PublisherProbe<String> probe =
            PublisherProbe.of(PublisherProbeTestExample.supplyStandbyPower());

    StepVerifier
            .create(PublisherProbeTestExample
                    .processTask(
                            PublisherProbeTestExample.supplyMainPower(),
                            probe.mono())
            )
            .expectNextCount(1)
            .verifyComplete();

    probe.assertWasSubscribed();
    probe.assertWasRequested();
    probe.assertWasNotCancelled();
}
```

> PublisherProbe를 이용해 Sequence의 실행 경로를 테스트할 수 있다
> 주 목적은 Sequence의 Signal 이벤트뿐만 아니라 switchEmpty() Operator로 인해 Sequence가 분기되는 상황에서 실제로 어느 Publisher가 동작하는지 해당 Publisher의 실행 경로를 테스트하는 것이다
> Publisher가 구독을 했는지, 요청을 했는지, 중간에 취소가 되지 않았는지를 Assertion함으로써 검증한다
> 
> PublisherProbeTestExample 클래스의 supplyMainPower() 메서드가 Mono.empty()를 리턴하기 때문에 processTask() 메서드에서는 최종적으로 Mono<String> standby가 동작하게 된다
> 
> 📕 306p 1번째 (13장)
>

**🧐 : 흠.. 확실히 어떻게 동작하는지 이해가 잘 안간다…ㅜㅜㅜ**
