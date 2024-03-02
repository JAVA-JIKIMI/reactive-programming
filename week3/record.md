### context라는 용어에 대한 의미를 명확한게 인지할수 있어서 좋았다
- spring security context holder를 사용 하는 사례가 있었는데, 기본적인 로그인 기능을 사용하면 단순히 config에 api matcher만으로 로그인한 사용자의 정보를 spring security context holder에 로그인한 정보를 저장할수 있게 된다
- 그런데 간혹, 예제나 gpt를 사용하다보면 기본 로그인 정책만 사용하는데도 spring security context holder에 set하는 경우가 있었다. 그래서 contextholder에 용어도 중요한데 사용하는 방법또한 잘 알고 사용하는게 중요하다고 생각한다.

### 디버깅 모드
- 비동기로 로깅 기능이 가능할까?
    - 실제 웹플럭스에서 logBack의 AsyncAppender를 적용하게 되면 OOM이 발생하는 경우가 발생한 사례를 본적이 있었다 → https://junho85.pe.kr/m/1777
    - log4j2에서는 비동기 로직이 많이 개선되어 괜찮을것으로 보여진다 → https://logging.apache.org/log4j/2.x/manual/async.html

### 왜 리액티브 프로그래밍은 역순으로 진행될까? → 테넷 equals 리액티브 프로그래밍
- subscriber에서 읽는 시점에서 데이터를 읽기 때문에 subscriber → publisher로 올라가는 구조가 당연한 구조라고 볼수 있다
- 그렇기 때문에 context에 저장되는 데이터도 subsciber 입장에서는 자연스러운 흐름인것 같다. context는 subscriber 소유의 공간인것 같다
- 출처
    - [https://www.inflearn.com/questions/1085489/contextwrite의-실행시점](https://www.inflearn.com/questions/1085489/contextwrite%EC%9D%98-%EC%8B%A4%ED%96%89%EC%8B%9C%EC%A0%90)
    - https://projectreactor.io/docs/core/release/reference/#context.write
