# 📌 인상 깊었던 내용

## **📚 Spring WebFlux의 요청 처리 흐름**

> 1. 최초 클라이언트로부터 요청이 들어오면 Netty 등의 서버 엔진을 거쳐 HttpHandler가 들어오는 요청을 전달받는다. HttpHandler는 SeverHttpRequest와 ServerHttpResponse를 포함하는 ServerWebExchange를 생성한 후, WebFilter 체인으로 전달한다
> 2. ServerWebExchange는 WebFilter 체인에서 전처리 과정을 거친 후, WebHandler 인터페이스의 구현체인 DispatcherHandler에게 전달된다
> 3. Spring MVC의 DispatcherServlet과 유사한 역할을 하는 DispatcherHandler에서는 HandlerMapping List를 원본 Flux의 소스로 전달받는다. 
> 4. ServerWebExchange를 처리할 핸들러를 조회한다
> 5. 조회한 핸들러의 호출을 HandlerAdapter에게 위임한다
> 6. HandlerAdapter는 ServerWebExchange를 처리할 핸들러를 호출한다
> 7. Controller 또는 HandlerFunction 형태의 핸들러에서 요청을 처리한 후, 응답 데이터를 리턴한다
> 8. 핸들러로부터 리턴받은 응답 데이터를 처리할 HandlerResultHandler를 조회한다
> 9. 조회한 HandlerResultHandler가 응답 데이터를 적절하게 처리한 후, response로 리턴한다
> 
> 📕 424p 7번째 (15장)
> 

### **🧐 : 요청 처리 흐름 숙지하자!!**

# 📌 이해가 가지 않았던 내용

## **📚 embadded tomcat**

> Spring MVC는 서블릿 기반의 Blocking I/O 방식이기 때문에 하나의 요청을 처리하기 위해 하나의 스레드를 사용하고, 해당 스레드의 작업이 끝나기 전까지는 스레드가 차단된다
> 
> 📕 421p 3번째 (15장)
> 

### **🧐 : 보통 Spring MVC와 Web Flux의 기준을 BIO(Blocking IO), NIO(Non-Blocking IO)를 차이로 두는데 톰캣 버전이 8.0부터는 NIO Connector가 기본으로 설정되었고, 9.0부터는 BIO Connector가 deprecated되었다. 그래서 현재는 트래픽에 따른 성능 이점이 그렇게 있을까 싶다..**

# 📌 논의해보고 싶었던 내용
