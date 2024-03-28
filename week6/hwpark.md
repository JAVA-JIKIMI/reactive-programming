# 18. Spring Data R2DBC

R2DBC : Reactive Relational Database Connectivity

관계형 데이터베이스에 리액티브 프로그래밍 API를 제공하기 위한 개방형 Specification.
드라이버 벤더가 구현하고 클라이언트가 사용하기 위한 SPI(Service Provider Interface).

### Spring Data R2DBC
- R2DBC 기반 Repository를 좀 더 쉽게 구현하게 해 주는 Spring Data Family 프로젝트의 일부.
- JPA같은 ORM 프레임워크에서 제공하는 캐싱, 지연 로딩, 과 같은 특징들이 제거되어 단순한 방법으로 사용할 수 있음.
- 데이터 액세스 계층의 보일러플레이트 코드의 양을 대폭 줄일 수 있음.

### R2DBC Repository
기존 Repository와 다른 점.
- 리액티브를 지원하는 ReactiveCrudRepository를 지원
- 메서드의 리턴 타입이 Mono 또는 Flux.


# 19. 예외 처리
@ExceptionHandler, @ControllerAdvice를 제외한 Spring WebFlux 전용 예외 처리 기법

### onErrorResume
에러 이벤트가 발생했을 때, DownStream에 전파하지 않고, 
대체 Publisher를 통해 에러 이벤트에 대한 대체 값을 emit 하거나 
발생한 에러 이벤트를 래핑한 후에 다시 에러 이벤트를 발생시키는 역할을 함.

### ErrorWebExceptionHandler 활용 글로벌 예외처리

# 20. WebClient
Non-Blocking HTTP request를 위한 리액티브 웹 클라이언트

근데, FeignClient도 reactive를 지원한다.  
(https://github.com/OpenFeign/feign/tree/master/reactive)

FeignClient는 webclient와 달리 호출결과를 caching하는 기능이 있어서 캐싱이 필요할 땐 reactive feign을 사용하면 좋을듯 