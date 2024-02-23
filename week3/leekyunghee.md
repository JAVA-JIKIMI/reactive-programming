
## Context란?
•	어떤 것을 이해하는 데 도움이 될 만한 관련 정보나 이벤트, 상황
•	어떠한 상황에서 그 상황을 처리하기 위해 필요한 정보
•	Reactor에서 Context의 정의
Operator 같은 Reactor 구성요소 간에 전파되는 key/value 형태의 저장소
•	전파: Downstream에서 Upstream 체인상의 각 Operator가 해당 Context 정보를 동일하게 이용할 수 있음
#### 정보를 공유하기 위한 목적을 가지는 것으로 이해함. 
•	Reactor의 Context는 Subscriber와 매핑됨
o	즉, 구독이 발생할 때마다 해당 구독과 연결된 하나의 Context가 생김
o	Reactor에서는 Operator 체인상의 서로 다른 스레드들이 Context의 저장된 데이터에 손쉽게 접근할 수 있음
