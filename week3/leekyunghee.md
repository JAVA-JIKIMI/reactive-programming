
### Context란?
*	어떤 것을 이해하는 데 도움이 될 만한 관련 정보나 이벤트, 상황
*	어떠한 상황에서 그 상황을 처리하기 위해 필요한 정보
###	Reactor에서 Context의 정의
* Operator 같은 Reactor 구성요소 간에 전파되는 key/value 형태의 저장소
###	전파: Downstream에서 Upstream 체인상의 각 Operator가 해당 Context 정보를 동일하게 이용할 수 있음
### 정보를 공유하기 위한 목적을 가지는 것으로 이해함. 
*	Reactor의 Context는 Subscriber와 매핑됨
*	즉, 구독이 발생할 때마다 해당 구독과 연결된 하나의 Context가 생김
*	Reactor에서는 Operator 체인상의 서로 다른 스레드들이 Context의 저장된 데이터에 손쉽게 접근할 수 있음

### 자주 사용되는 Context 관련 API
#### Context API (쓰기)
* put(key, value) : key/value 형태로 Context에 값을 쓴다.
* of(key1, value1, key2, value2, ...) : key/value 형태로 Context에 여러 개의 값을 쓴다.
* pulAll(ContextView) : 현재 Context와 파라미터로 입력된 ContextView를 merge한다.
* delete(key) : Context에서 key에 해당하는 value를 삭제한다.
#### ContextView API (읽기)
* get(key) : ContextView에서 key에 해당하는 value를 반환한다.
* getOrEmpty(key) : ContextView에서 key에 해당하는 value를 Optional로 래핑해서 반환한다.
* getOrDefault(key, default value) : ContextView에서 key에 해당하는 value를 가져온다. key에 해당하는 value가 없으면 default value를 가져온다.
* hasKey(key) : ContextView에서 특정 key가 존재하는지를 확인한다.
* isEmpty() : Context가 비어 있는지 확인한다.
* size() : Context 내에 있는 key/value의 개수를 반환한다.

#### Context의 특징
* Context는 구독이 발생할 때마다 하나의 Context가 해당 구독에 연결된다.
* Context는 Operator 체인의 아래에서 위로 전파된다.
* 모든 Operator에서 Context에 저장된 데이터를 읽어올 수 있으려면 contextWrite()을 Operator 체인의 맨 마지막에 둬야 한다.
* 동일한 키에 대한 값을 중복해서 저장하면 Operator 체인에서 가장 위쪽에 위치한 contextWrite()이 저장한 값으로 덮어쓴다.

### Reactor에서의 디버깅 방법
* 비동기, 선언형 프로그래밍 방식의 Reactor는 디버깅이 쉽지 않음
### Debug Mode를 사용한 디버깅
* Hooks.onOperatorDebug()를 통해 디버그 모드 활성화 (Example12_1)
* 체인상에서 에러가 시작된 지점부터 에러 전파 상태를 표시
* 비용이 많이 드는 동작
* 스택트레이스 캡처
* 에러가 발생한 Assembly의 스택트레이스를 원본 스택트레이스 중간에 끼워 넣음
* 운영 환경에서는 reactor.tools.agent.ReactorDebugAgent 이용
* IntelliJ에서는 [Enable Reactor Debug mode]에서 Hooks.onOperatorDebug() 또는 ReactorDebugAgent.init() 중 택1 하여 활성화 가능
### checkpoint( ) Operator를 사용한 디버깅
* checkpoint() Operator를 사용하여 특정 Operator 체인 내의 스택트레이스를 캡처
#### 각 Operator 체인에 checkpoint()를 추가한 후, 범위를 좁혀 가면서 단계적으로 에러 발생 지점을 찾는다.

##  Testing
* Flux 또는 Mono를 Reactor Sequence로 정의한 후 구독 시점에 해당 Operator 체인이 시나리오대로 동작하는지 테스트

