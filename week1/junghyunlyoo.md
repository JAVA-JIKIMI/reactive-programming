# 31p

두번째 문장(반면에 ~ 되겠습니다)을 보고, 이 문단이 마치 Non-Blocking이지 않을까? 싶은 생각이 듦. 어쩌면 Non-blocking + Async인 예시일지도?
```
만약 이 문장이 Blocking + Sync라면 문장이 어떻게 쓰여져야 했을까? 
->
다음 부분을 이해하기 위해선 3장에 대한 이해가 무조건 필요합니다.(sync)
그리고 이 책은 앞선 부분들의 이해가 완료되기 전에는 다음 부분으로의 진행을 허락하지 않습니다.(blocking)
따라서 독자는 3장을 이해하기 전까지는 더이상 책을 읽을 수 없습니다.

만약 이 문장이 Blocking + Async라면 문장이 어떻게 쓰여져야 했을까? 
->
다음 부분을 이해하기 위해서 3장에 대한 이해가 꼭 필요하진 않습니다.(Async)
그리고 이 책은 앞선 부분들의 이해가 완료되기 전에는 다음 부분으로의 진행을 허락하지 않습니다.(blocking)
따라서 독자는 3장을 이해하기 전까지는 더이상 책을 읽을 수 없습니다.
(이해를 안해도 되는데 더이상 책을 읽을 수 없는 이상한(?) 상황)

만약 이 문장이 Non-Blocking + Sync라면 문장이 어떻게 쓰여져야 했을까? 
->
다음 부분을 이해하기 위해선 3장에 대한 이해가 무조건 필요합니다.(sync)
그리고 이 책은 앞선 부분들의 이해가 완료되지 않더라도 다음 부분으로의 진행을 허락합니다.(Non-blocking)
따라서 독자는 3장을 이해해하지 않더라도 책읽기를 계속 진행할 수 있습니다. (이해가 필수인데 챡울 읽어버리게 되는 이상한(?) 상황)

만약 이 문장이 Non-Blocking + Async라면 문장이 어떻게 쓰여져야 했을까? 
->
다음 부분을 이해하기 위해서 3장에 대한 이해가 꼭 필요하진 않습니다.(Async)
그리고 이 책은 앞선 부분들의 이해가 완료되지 않더라도 다음 부분으로의 진행을 허락합니다.(Non-blocking)
따라서 독자는 3장을 이해해하지 않더라도 책읽기를 계속 진행할 수 있습니다.
```

# Reactive Streams 간단 구현체

```java
public class SimplePublisher implements Publisher<Integer> {
    private final List<Integer> data = Arrays.asList(1, 2, 3, 4, 5); // 데이터 스트림

    @Override
    public void subscribe(Subscriber<? super Integer> subscriber) {
        subscriber.onSubscribe(new Subscription() {
            private int index = 0;

            @Override
            public void request(long n) {
                while (n > 0 && index < data.size()) {
                    subscriber.onNext(data.get(index++));
                    n--;
                }
                if (index == data.size()) {
                    subscriber.onComplete();
                }
            }

            @Override
            public void cancel() {
                // 구독 취소 시 필요한 로직 처리
            }
        });
    }
}
```
```java

public class SimpleSubscriber implements Subscriber<Integer> {
    private Subscription subscription;
    private int count = 0;
    private final int receiveLimit = 3; // 받고자 하는 데이터의 최대 개수

    @Override
    public void onSubscribe(Subscription subscription) {
        this.subscription = subscription;
        subscription.request(1); // 첫 데이터 요청
    }

    @Override
    public void onNext(Integer item) {
        System.out.println("Received: " + item);
        count++;

        if (count >= receiveLimit) {
            System.out.println("Receive limit reached, cancelling subscription");
            subscription.cancel(); // 최대 수신 개수 도달 시 구독 취소
        } else {
            subscription.request(1); // 다음 데이터 요청
        }
    }

    @Override
    public void onError(Throwable t) {
        t.printStackTrace();
    }

    @Override
    public void onComplete() {
        System.out.println("Completed");
    }
}
```

# 63p Blocking과 non blocking 예시 비교

Blocking과 non blocking을 좀 더 적절히 비교하기 위해선, Blocking 예시에서도 외부 호출이 2개여야 했을 것 같다. 왜냐면 만약 Non blocking 예시에서의 외부 호출이 1개라면 사실 blocking 방식과 non blocking 방식의 차이가 없었을 것이기 때문.

# 81p Mono의 subscribe

Mono가 Publish interface의 구현체인데 어떻게 람다 표현식으로 Subscriber 객체를 전달할 수가 있나 했더니, overloading을 이용한 것이었다

```java
	public final Disposable subscribe(Consumer<? super T> consumer) {
		Objects.requireNonNull(consumer, "consumer");
		return subscribe(consumer, null, null);
	}
```
