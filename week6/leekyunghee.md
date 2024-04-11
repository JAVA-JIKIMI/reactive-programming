# Spring Data R2DBC
### 리액티브 API 제공
### 클라이언트의 요청부터 데이터 베이스 액세스까지 완전한 Non-Blocking 애플리케이션 구현
### 직접 테이블 스크립트를 작성해야함

# 예외처리
### onErrorResume() Operator 
### 이벤트가 발생했을 때  에러 이벤트를 Downstream으로 전파하지 않고 대체 Publisher를 통해 에러 이벤트에 대한 대체 값을 emit 하거나 발생한 에러 이벤트를 래핑한 후에 다시 에러 이벤트를 발생시키는 역할을 함

### 에러가 발생하는 Observable을 설정함. 먼저, 예외가 발생할 수 있는 Observable을 설정. 이는 주로 네트워크 호출이나 파일 시스템 액세스 등의 비동기 작업을 수행하는 Observable임
### onErrorResume()을 사용하여 대체 Observable을 지정: onErrorResume() 오퍼레이터를 사용하여 예외가 발생했을 때 대체할 Observable을 지정. 이 대체 Observable은 원래 Observable에서 예외가 발생했을 때 실행됨
### subscribe()를 사용하여 결과를 처리: 마지막으로, Observable을 구독하여 결과를 처리합니다. 이 때 대체 Observable이나 원래 Observable 중 하나의 결과가 전달됩니다.



### Spring WebFlux를 사용하여 Reactive 프로그래밍을 구현하는 예제 
### 아래 예제는 Spring WebFlux의 onErrorResume() 메서드를 사용하여 예외를 처리하는 방법을 보여줌

import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

public class Main {
    public static void main(String[] args) {
        WebClient webClient = WebClient.create("https://api.example.com");

        Mono<String> resultMono = webClient.get()
                .uri("/resource")
                .retrieve()
                .bodyToMono(String.class)
                .onErrorResume(error -> {
                    System.err.println("에러 발생: " + error.getMessage());
                    return Mono.just("에러가 발생하여 대체 데이터를 반환합니다.");
                });

        resultMono.subscribe(
                result -> System.out.println("결과: " + result),
                error -> System.err.println("구독 중 에러 발생: " + error.getMessage())
        );
    }
}

###  이 예제는 WebClient를 사용하여 외부 API로부터 리소스를 가져오는 작업을 수행함 
### 그러나 만약 요청이 실패하면 대체 데이터를 반환하도록 onErrorResume()을 사용 그 결과 에러가 발생할 경우 에러 메시지를 출력하고 대체 데이터를 반환
### 주의할 점은 Spring WebFlux의 WebClient는 Reactive 스트림을 반환하기 때문에 Mono나 Flux와 같은 리액티브 타입을 사용하여 비동기 작업을 처리해야 함.


