# 2주차

# 📌 인상 깊었던 내용

## **📚 마블 다이어그램 보는 방법**

> 리액티브 프로그래밍을 이해하려면 마블 다이어그램을 이해해야 한다
> 
> 📕 125p 1번째 (6장)
> 

**🧐 : 플로우 정리**

![KakaoTalk_Photo_2024-02-05-01-19-13.jpeg](https://prod-files-secure.s3.us-west-2.amazonaws.com/571c1952-01e0-464b-8ba7-04ce180d0108/95cfd188-0e67-46f2-96f5-c82cfff9e105/KakaoTalk_Photo_2024-02-05-01-19-13.jpeg)

1. 시작
2. publisher가 emit하는 데이터
3. 수직 바는 데이터의 emit이 정상적으로 끝났음을 의미(onComplete)
4. publisher의 emit된 데이터가 operator 함수의 입력으로 전달되는 것을 의미
5. publisher로부터 전달받은 데이터를 처리하는 Operator 함수
6. operator 함수에서 가공 처리한 후에 출력되는 것을 의미
7. operator 함수에서 가공 처리되어 출력으로 내보내진 데이터의 타임라인
8. operator 함수에서 가공 처리된 데이터
9. ‘X’ 표시는 에러가 발생해 데이터 처리가 종료되었음을 의미

## **📚 중간 퀴즈**

> Flux와 Mono의 특성만 숙지한다면 쉽게 맞힐 수 있을 것 같다
> 
> 1. 3번 라인의 concat() Operator에서 리턴하는 Publisher는 Mono? Flux?
> 2. 7번 라인의 collectList() Operator에서 리턴하는 Publisher는 Mone? Flux?
> 3. 8번 라인에서 최종적으로 출력되는 데이터의 형태는?
> 
> 📕 138p 1번째 (6장)
> 

```jsx
public class Example {
	public static void main(String[] args) {
		Flux.concat(
			Flux.just("Mercury", "Venus", "Earth"),
			Flux.just("Mercury", "Venus", "Earth"),
			Flux.just("Mercury", "Venus", "Earth"),
		).collectList()
		.subscribe(planets -> System.out.println(planets));
	}
}	
```

**🧐 : 정답**

1. **Flux→ concat을 이용해서 총 세 개의 Flux가 가지고 있는 아홉 개의 데이터를 연결하기 때문에 concat()의 리턴값을 Flux가 될 수 밖에 없다**
2. **Mono → collectList는 여러 개의 데이터를 하나의 List에 원소로 포함시킨다**
3. **Mono → 2번의 정답이 List이기 때문에 각각의 데이터를 반복적으로 출력하는 것이 아니라 List 객체를 그대로 출력한다**

## **📚 Cold sequence VS Hot Sequence**

> coldSequence → 구독 시점이 달라도 publisher가 데이터를 emit 하는 과정을 처음부터 다시 시작하는 데이터 흐름
> hotSequence → 구독 시점이 다르면 구독이 발생한 시점 이후에 시작하는 데이터 흐름
> 
> 📕 144p 8번째 ( 7장)
> 

**🧐 : 나중에 적용하기 좋은 개념으로 보여 우선 저장**

## **📚 Cold/Hot Sequance 응용편**

> share() : 여러 subscriber가 하나의 원본 Flux를 공유한다
> cache() : emit된 데이터를 캐시한 뒤, 구독이 발생할때마다 캐시된 데이터를 전달한다
> (결과적으로 캐시된 데이터를 전달하기 때문에 구독이 발생할 때마다 Subscriber는 동일한 데이터를 전달받게 된다)
> 
> 📕 150p 4번째 ( 7장)
> 

**🧐 : 요것두 cold/hot sequence 개념적용할때 같이 사용해보면 좋을 메서드로 보여 킵해본다**

---

# 📌 이해가 가지 않았던 내용

## **📚 Reactor에서 제공하는 BackPressure 전략 종류**

> Ignore 전략 : Backpressure를 적용하지 않는다
> ERROR 전략 : Downstream으로 전달할 데이터라 버퍼에 가득 찰 경우, Exception을 발생시키는 전략
> DROP 전략 : Downstream으로 전달할 데이터가 버퍼에 가득 찰 경우, 버퍼 밖에서 대기하는 먼저 emit된 데이터부터 Drop 시키는 전략
> LATEST 전략 : DownStream으로 전달할 데이터가 버퍼에 가득 찰 경우, 버퍼 밖에서 대기하는 가장 최근에(나중에) emit된 데이터부터 버퍼에 채우는 전략
> BUFFER 전략 : DownStream으로 전달할 데이터가 버퍼에 가득 찰 경우, 버퍼 안에 있는 데이터부터 Drop 시키는 전략(DROP이나 LATEST는 버퍼 밖에 있는 데이터를 제거하는데, 버퍼 전략은 버퍼 내부의 데이터를 소멸시킨다)
> 
> 📕 164p 1번째 ( 8장)
> 

**🧐 : 이 전략들을 실제로 사용할까..? 실무에서는 크리티컬 할것 같은데…**

---

# 📌 논의해보고 싶었던 내용

## **📚 Sinks**

> 일반적으로 generator() Operator나 create() Operator는 싱글스레드 기반에서 Signal을 전송하는 데 사용하는 반면, Sinks는 멀티스레드 방식으로 Signal을 전송해도 스레드 안전성을 보장하기 때문에 예기치 않은 동작으로 이어지는 것을 방지해준다
> 
> 📕 188p 15번째 ( 9장)
> 

**🧐 : 흠.. 무조건 Sinks로 구현하는게 좋은게 아닐까..?**
