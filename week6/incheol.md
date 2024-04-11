# 6주차 (파이널)

# 📌 인상 깊었던 내용

## **📚 예외처리**

```jsx
// 1. onErrorResume
public Mono<ServerResponse> createBook(ServerRequest request) {
  return request.bodyToMono(BookDto.Post.class)
          .doOnNext(post -> validator.validate(post))
          .flatMap(post -> bookService.createBook(mapper.bookPostToBook(post)))
          .flatMap(book -> ServerResponse
                  .created(URI.create("/v9/books/" + book.getBookId()))
                  .build())
          .onErrorResume(BusinessLogicException.class, error -> ServerResponse
                      .badRequest()
                      .bodyValue(new ErrorResponse(HttpStatus.BAD_REQUEST,
                                                      error.getMessage())))
          .onErrorResume(Exception.class, error ->
                  ServerResponse
                          .unprocessableEntity()
                          .bodyValue(
                              new ErrorResponse(HttpStatus.INTERNAL_SERVER_ERROR,
                                                  error.getMessage())));
                                                  
// 2. ErrorWebExceptionHandler
public class GlobalWebExceptionHandler implements ErrorWebExceptionHandler {
		...
		
		private Mono<Void> handleException(ServerWebExchange serverWebExchange,
                                       Throwable throwable) {
        ErrorResponse errorResponse = null;
        DataBuffer dataBuffer = null;

        DataBufferFactory bufferFactory =
                                serverWebExchange.getResponse().bufferFactory();
        serverWebExchange.getResponse().getHeaders()
                                        .setContentType(MediaType.APPLICATION_JSON);

        if (throwable instanceof BusinessLogicException) {
            BusinessLogicException ex = (BusinessLogicException) throwable;
            ExceptionCode exceptionCode = ex.getExceptionCode();
            errorResponse = ErrorResponse.of(exceptionCode.getStatus(),
                                                exceptionCode.getMessage());
            serverWebExchange.getResponse()
                        .setStatusCode(HttpStatus.valueOf(exceptionCode.getStatus()));
        } else {
            errorResponse = ErrorResponse.of(HttpStatus.INTERNAL_SERVER_ERROR.value(),
                                                            throwable.getMessage());
            serverWebExchange.getResponse()
                                    .setStatusCode(HttpStatus.INTERNAL_SERVER_ERROR);
        }

        try {
            dataBuffer =
                    bufferFactory.wrap(objectMapper.writeValueAsBytes(errorResponse));
        } catch (JsonProcessingException e) {
            bufferFactory.wrap("".getBytes());
        }

        return serverWebExchange.getResponse().writeWith(Mono.just(dataBuffer));
    }
}
```

> onErrorResume() - onErrorResume() Operator는 에러 이벤트가 발생했을 때 에러 이벤트를 Downstream으로 전파하지 않고, 대체 Publisher를 통해 에러 이벤트에 대한 대체 값을 emit하거나 발생한 에러 이벤트를 래필한후에 다시 에러 이벤트를 발생시킨다(try ~ catch와 비슷한 동작)
> ErrorWebExceptionHandler() - 매번 인라인으로 처리하기 힘들기 때문에 모듈로 사용할 수 있는 에러 핸들러를 정의할 수 있다(ControllerAdvice와 비슷한 개념)
> 
> 📕 490p 1번째 (19장)
> 

### **🧐 :**

# 📌 이해가 가지 않았던 내용

## **📚 Mono<List<Book>> vs Flux<Book>**

```jsx
// r2dbcTemplate
public Mono<List<Book>> findBooks() {
	return template.select(Book.class).all().collectList();
}

// r2dbc pagination
public interface BookRepository extends ReactiveCrudRepository<Book, Long> {
	Mono<Book> findByIsbn(String isbn)
	Flux<Book> findAllBy(Pageable pageable)
}

@Repository("bookRepositoryV7")
public interface BookRepository extends ReactiveCrudRepository<Book, Long> {
    Mono<Book> findByIsbn(String isbn);
    Mono<List<Book>> findByAuthor(String author); // 이게 맞을까?
    Flux<Book> findByAuthor(String author); // 이게 맞을까?
    Flux<Book> findAllBy(Pageable pageable);
}
```

> 📕 482p 1번째 (18장)
> 

### **🧐 : 같은 목록인데… template으로 목록을 리턴할때는 Mono 이고, 페이징 결과는 왜 Flux일까..?**

# 📌 논의해보고 싶었던 내용

## **📚WebClient**

> WebClient는 Spring 5부터 지원하는 Non-Blocking HTTP request를 위한 리액티브 웹 클라이언트로서 함수형 기반의 향상된 API를 제공한다
> 
> 출처 : https://digma.ai/restclient-vs-webclient-vs-resttemplate/
> 
> 출처 : https://docs.spring.io/spring-framework/reference/integration/rest-clients.html
> 
> 📕 498p 1번째 (20장)
> 

### **🧐 : WebClient를 많이들 사용하는가..? 혹시 최근에 나온 RestClient를 사용하지는 않는지?**

## **📚 SSE(Server-Sent Events)**

```jsx
@Bean
public RouterFunction<?> routeStreamingBook(BookService bookService,
                                            BookMapper mapper) {
  return route(RequestPredicates.GET("/v11/streaming-books"),
          request -> ServerResponse
                  .ok()
                  .contentType(MediaType.TEXT_EVENT_STREAM)
                  .body(bookService
                                  .streamingBooks()
                                  .map(book -> mapper.bookToResponse(book))
                          ,
                          BookDto.Response.class));
}
```

> SSE는 클라이언트가 HTTP 연결을 통해 서버로부터 전송되는 업데이트 데이터를 지속적으로 수신할 수 있는 단방향 서버 푸시 기술이다
> 
> 출처 : https://en.wikipedia.org/wiki/Server-sent_events
> 
> 출처 : [https://conanmoon.medium.com/프로그래밍-일기-sse-vs-websocket-62d4b7c90fc7](https://conanmoon.medium.com/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%EC%9D%BC%EA%B8%B0-sse-vs-websocket-62d4b7c90fc7)
> 
> 📕 498p 1번째 (20장)
> 

### **🧐 : 실시간 스트리밍 연결은 소켓으로 해결한다고 생각했는데 SSE가 대안이 될수 있을까?**
