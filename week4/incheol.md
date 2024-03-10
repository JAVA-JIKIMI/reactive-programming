# 📌 인상 깊었던 내용

**📚 defer**

```jsx
public class Example {
	public static void main(String[] args) throws InterruptedException {
		log.info("# start: {}", LocalDateTime.now());
		Mono<LocalDateTime> justMono = Mono.just(LocalDateTime.now());
		Mono<LocalDateTime> deferMono = mono.defer(() -> Mono.just(LocalDateTime.now()));
		
		Thread.sleep(2000);
		
		justMono.subscribe(data -> log.info("# onNext just1: {}", data);
		deferMono.subscribe(data -> log.info("# onNext defer1: {}", data);
		
		Thread.sleep(2000);
		
		justMono.subscribe(data -> log.info("# onNext just2: {}", data);
		deferMono.subscribe(data -> log.info("# onNext defer2: {}", data);
	}
}
```

> defer() Operator는 Operator를 선언한 시점에 데이터를 emit하는 것이 아니라 구독하는 시점에 데이터를 emit하는 Flux 또는 Mono를 생성한다
> 
> 위의 실행 결과를 보면 deferMono의 경우 예상했던 것처럼 emit된 현재 시간 데이터가 2초의 간격을 두고 emit되었음을 알 수 있다.
> 
> justMono의 경우 출력된 시간은 어떻게 될까?
> 
> 📕 317p 8번째 (14장)
> 

**🧐 : 정답은 시간은 동일하다.. 왜냐하면 just() Operator는 Hot Publisher이기 때문에 Subscriber의 구독 여부와는 상관없이 데이터를 emit하게 된다. 그리고 구독이 발생하면 emit된 데이터를 다시 재생(replay)해서 subscriber에게 전달한다. 따라서 justMono의 경우 출력 결과가 같다**

# 📌 이해가 가지 않았던 내용

# 📌 논의해보고 싶었던 내용
