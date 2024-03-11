# 11. Operator

## Sequence 생성을 위한 Operator
- just, justOrEmpty : Hot publisher 로 구동 여부와 상관없이 데이터를 emit
- fromIterable, fromStream, range
- defer : 구독이 발생하기 전까지 데이터의 emit을 지연
- using : 파라미터로 전달받은 resource를 emit
- generate : 동기적으로 데이터를 하나씩 emit
- create : generate와 비슷하나, 한번에 여러 건의 데이터를 비동기적으로 emit할 수 있음

## Sequence 필터링 Operator
- filter : 조건에 일치하는 데이터만 emit
- skip : 입력받은 파라미터 숫자 만큼 건너뛴 후 emit
- take : 입력받은 파라미터 숫자 만큼만 emit
- next : 입력한 시간 내에 emit된 데이터만 emit


## Sequence 변환 Operator
- map : mapper function 을 사용하여 변환 한 후 emit
- flatMap : 내부에서 inner sequence 를 생성하여 여러 건의 데이터를 emit
- concat : 파라미터로 입력되는 sequence 를 연결. 앞선 publisher가 종료될때까지 나머지 publisher 의 sequence는 대기
- merge : 파라미터로 입력되는 publisher의 sequence에서 emit된 데이터를 인터리빙
- zip : 파라미터로 입력되는 publisher의 sequence에서 emit된 데이터를 결합
- and : 파라미터로 입력되는 publisher의 sequence에서 emit된 Complete Signal 을 결합하여 새로운 Mono<Void> 반환
- collectList : Flux에서 emit된 데이터를 모아서 List로 변환한 후 변환된 List를 emit하는 Mono 반환
- collectMap : Flux에서 emit된 데이터를 기반으로 key와 value를 생성하여 Map을 emit하는 Mono 반환

## Sequence 내부 동작을 위한 Operator
- doOnSubscribe, doOnRequest, doOnNext
- doOnComplete, doOnError, doOnCancel, doOnSuccess,
- doOnTerminate, doOnEach, doOnDiscard, doAfterTerminate
- doFirst, doFinally

## 에러 처리를 위한 Operator
- error : 지정된 error로 종료
- onErrorReturn : 에러 이벤트가 발생했을 때 downstream으로 전파하지 않고 대체 값을 emit. try-catch문의 catch와 유사함.
- onErrorResume : 에러 이벤트가 발생했을 때 downstream으로 전파하지 않고 대체 publisher를 return.
- onErrorContinue : 에러 이벤트가 발생했을 때 에러 영역 내에 있는 데이터는 제거하고 Upstream 에서 후속 데이터를 emit
- retry : 재시도. 파라미터로 입력한 횟수만큼 원본 Flux의 Sequence를 재구독

## Sequence 동작 시간 측정을 위한 Operator
- elapsed : emit된 데이터 사이의 경과 시간 측정

## Flux Sequence 분할을 위한 Operator
- window : Upstream에서 emit되는 첫번째 데이터부터 maxSize만큼의 데이터를 포함하는 새로운 Flux로 분할. 이렇게 분할된 Flux를 window라고 부름.
- buffer : Upstream에서 emit되는 첫번째 데이터부터 maxSize만큼의 데이터를 `List 버퍼`로 한번에 emit
- bufferTimeout : Upstream에서 emit되는 첫번째 데이터부터 maxSize, maxTime 내에 emit된 데이터를 `List 버퍼` 로 한번에 emit
- groupBy : 그룹별 작업 수행시 사용. emit된 데이터를 keyMapper로 생성한 key를 기준으로 그룹화한 `GroupedFlux` 리턴

## 다수의 Subscriber에게 Flux를 멀티캐스팅 하기 위한 Operator
- publish : 구독 시점이 아닌 connect() 시험에 데이터를 emit함. 늦게 구독하면 뒤의 sequence만 받음.
- autoConnect : 지정한 숫자만큼의 구독이 발생하는 시점에 connect()가 자동 호출.
- refCount : 지정한 숫자만큼의 구독이 발생하는 시점에 connect()가 자동 호출. 모든 구독이 취소되거나 Upstream의 데이터 emit이 종료되면 연결이 해제.
