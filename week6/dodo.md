## WebClient 사용 경험 

### 동기
비동기 방식의 client를 사용해야 함. 게시글 등록 시점에 외부 MS의 API 호출 및 링크 유효성 검증(ogtag parser) 등 호출해야할 것이 많음

### WebClient 사용

처음 감잡기에 약간 어려웠으나 사용하다보니 익숙해짐. 
* 특히 map은 그냥 객체(우리가 아는 그 맵)를 응답하고 flatMap은 Mono나 Flux를 응답. 따라서 비동기적인 행위를 할때는 자연스럽게 flatMap을 쓰게되어서 생각보다 신묘한 설계에 감탄.
* OnErrorResume으로 예외처리도 꽤 편리함.
* 하지만 timeout 설정이 매우 까다롭고(책에 있는 것만으로 안됨) 내부 connection pool 관리도 어려움 <[요기내용](https://yangbongsoo.tistory.com/30)>
* reactor 의 오퍼레이션 체인의 영향은 업스트림으로 전파된다는 것을 알고 개발하니 이곳저곳 큰 도움이 되었다.
* 사용추천!