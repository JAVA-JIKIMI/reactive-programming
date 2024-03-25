### ServerWebExchange 
#### ServerWebExchange는 HTTP 요청,응답, request attributes, sessionattributes를 모두 포함하고 있는 컨테이너

### DispatcherHandler
#### WebHandler 인터페이스의 구현체 
#### Spring MVC에서 Front Controller 패턴이 적용된 DispatcherServlet처럼 중앙에서 먼저 요청을 전달받은 후에 다른 컴포넌트에 요청 처리를 위임
#### DispatcherHandler 자체가 Spring Bean으로 등록되도록 설계되어있고, ApplicationContext에서 HandlerMapping, HandlerAdapter, HandlerResultHandler 등의 요청 처리를 위한 위임 컴포넌트를 검색

| No | 처리 흐름 정리 |
| -- | -- |
| 1 | 클라이언트 요청 |
| 2 | HttpHandler가 요청을 받고 ServerWebExchange(ServerhttpRequest, ServerHttpResponse)를 생성 후 |
| 3 | WebFilter 체인으로 전달 |
| 4 | DispatcherHandler에게 전달 |
| 6 |  HandlerMapping List를 원본 Flux의 소스로 전달받음 | 
| 7 | HandlerMapping에서 ServerWebExchange을 처리할 핸들러 조회 |
| 8 | HandlerAdapter가 요청을 위임받음 |
| 9 | 실제 핸들러 호출 |
| 10 | HandlerResultHandler을 거쳐 response 반환 |
