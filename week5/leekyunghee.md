
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

### 15.4 Spring WebFlux 의 핵심 컴포넌트 
#### HttpHandler 
* HttpHandler는 다른 유형의 HTTP 서버 API로부터 request, response를 처리하기 위한 단 하나의 추상 메서드를 갖는다.
```java
public interface HttpHandler {

	/**
	 * Handle the given request and write to the response.
	 * @param request current request
	 * @param response current response
	 * @return indicates completion of request handling
	 */
	Mono<Void> handle(ServerHttpRequest request, ServerHttpResponse response);

}
```

* HttpHandler.java의 구현체인 HttpWebHandlerAdapter.java
```java
public class HttpWebHandlerAdapter extends WebHandlerDecorator implements HttpHandler {

	...
    
    @Override
	public Mono<Void> handle(ServerHttpRequest request, ServerHttpResponse response) {
		if (this.forwardedHeaderTransformer != null) {
			try {
				request = this.forwardedHeaderTransformer.apply(request);
			}
			catch (Throwable ex) {
				if (logger.isDebugEnabled()) {
					logger.debug("Failed to apply forwarded headers to " + formatRequest(request), ex);
				}
				response.setStatusCode(HttpStatus.BAD_REQUEST);
				return response.setComplete();
			}
		}
		ServerWebExchange exchange = createExchange(request, response);

		LogFormatUtils.traceDebug(logger, traceOn ->
				exchange.getLogPrefix() + formatRequest(exchange.getRequest()) +
						(traceOn ? ", headers=" + formatHeaders(exchange.getRequest().getHeaders()) : ""));

		return getDelegate().handle(exchange)
				.doOnSuccess(aVoid -> logResponse(exchange))
				.onErrorResume(ex -> handleUnresolvedError(exchange, ex))
				.then(Mono.defer(response::setComplete));
	}
    
    ...
}
```
#### 1. ServerWebExchange를 생성

### handler() 메서드의 파라미터로 전달받은 ServerHttpRequest, ServerHttpResponse로 ServerWebExchange를 생성한다.
```java
ServerWebExchange exchange = createExchange(request, response);
 ```

#### 2. WebHandler를 호출한다.
```java
return getDelegate().handle(exchange)
				.doOnSuccess(aVoid -> logResponse(exchange))
				.onErrorResume(ex -> handleUnresolvedError(exchange, ex))
				.then(Mono.defer(response::setComplete));
                
// HttpWebHandlerAdapter.java
public WebHandler getDelegate() {
    return this.delegate;
}
```

#### WebFilter
### 핸들러가 요청을 처리하기 전에 전처리 작업을 수행할 수 있도록 해준다. 애플리케이션 내에 정의된 모든 핸들러에 공통으로 동작한다. 
```java
public interface WebFilter {

	Mono<Void> filter(ServerWebExchange exchange, WebFilterChain chain);

}
``` 
### 파라미터로 받은 WebFilterChain을 통해 필터 체인을 형성하여 원하는 만큼의 WebFilter를 추가할 수 있다.

#### DispatcherHandler
### WebHanler 인터페이스의 구현체다. Spring MVC에서 Front Controller 패턴이 적용된 DispatcherServlet처럼 중앙에서 먼저 요청을 전달받은 후에 다른 컴포넌트에 요청 처리를 위임
### DispatcherHandler 자체가 Spring Bean으로 등록되도록 설계되어있고 ApplicationContext에서 HandlerMapping, HandlerAdapter, HandlerResultHandler 등의 요청 처리를 위한 위임 컴포넌트를 검색

#### HandlerAdapter
handler를 실제로 실행하기 위해 필요한 bean. HandlerAdapter는 HandlerMapping을 통해 얻은 핸들러를 직접적으로 호출하며, 응답 결과로 Mono<HandlerResult>를 리턴받는다.

 
```java
public interface HandlerAdapter {

	boolean supports(Object handler);

	Mono<HandlerResult> handle(ServerWebExchange exchange, Object handler);

}
``` 

#### boolean supports(Object handler) 파라미터로 전달받은 handler object를 지원하는지 체크
#### Mono<HandlerResult> handle(ServerWebExchange exchange, Object handler) 파라미터로 전달받은 handler object를 통해 핸들러 메서드를 호출한다.


#### HandlerResultHandler
### Handler 결과값을 바탕으로 response를 처리하기 위해 필요한 bean.


## 15.5 Spring WebFlux의 Non-Blocking 프로세스 구조를 유념하자
### MVC와 WebFlux 는 동시성 모델과 스레드에 대한 기본 전략에서 차이점을 보임
### 이벤트 루프 방식을 사용하여 스레드 차단없이 더 많은 요청을 처리할 수 있음

### 16.6 SpringMCV와 유사한 애노테이션 기반
#### @ResponseBody, ResponseEntity 타입 지원
#### 리액티브 타입을 이용해 데이터를 비동기적으로 responsebody에 렌더링 되도록 한다는 것
