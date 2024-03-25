
## Spring Webflux 기술 스택
![img.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FC2Foi%2FbtsdG4BHH22%2F09JxqIVTF3YkrxiGJwe1J0%2Fimg.png)


## Spring webFlux의 요청 처리 흐름
![img_1.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn9Euu%2FbtsdGABjBdD%2FGHHfAKh0VyVVWNPgN5yx5K%2Fimg.png)

1. 최초에 클라이언트로부터 요청이 들어오면 Netty 등의 서버 엔진을 거쳐, HttpHandler가 들어오는 요청을 받는다.
각 서버의 ServerhttpRequest, ServerHttpResponse를 포함하는 ServerWebExchange를 생성한후, WebFliter 체인으로 전달한다.
2. 전달받은 ServerWebExchange는 전처리 과정을 거친 후, WebHandler 인터페이스의 구현체인 DispatcherHandler에게 전달된다.
3. DispatcherHandler에서는 HandlerMapping List를 원본 Flux의 소스로 전달받는다.
4. ServerWebExchange를 처리할 핸들러를 조회한다.
5. 조회한 핸들러의 호출을 HandlerAdapter에게 위임한다.
6. HandlerAdapter는 ServerWebExchange를 처리할 핸들러를 호출한다.
7. Controller 또는 HandlerFunction 형태의 핸들러에서 요청을 처리한 후, 응답 데이터를 리턴받는다.
-> 핸들러에서 리턴한다고 표현했지만, 실제 핸들러에서 리턴되는 것은 응답 데이터를 포함하고 있는 Flux 또는 Mono Sequence 이다. 따라서 즉시 어떤 작업을 수행한다는 의미가 아니다.
8. 핸들러로부터 리턴받은 응답 데이터를 처리할 HandlerResultHandler를 조회한다.
9. 조회한 HandlerResultHandler가 응답 데이터를 적절하게 처리한 후, response로 리턴한다.
