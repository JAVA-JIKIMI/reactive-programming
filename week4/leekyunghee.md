# 자주 사용하는 Operator
### Sequence 생성을 위한 Operator

| Operator | Description |
| -- | -- |
| justOrEmpty |  emit할 데이터가 null일 경우 onComplete Signal을 전송하거나 null이 아닐 경우 emit하는 Mono를 생성 |
| fromIterable | Iterable에 포함된 데이터를 emit하는 Flux를 생성, 리스트에 포함된 데이터를 Subscriber에서 전달받아 출력 |
| fromStream | Stream에 포함된 데이터를 emit하는 Flux를 생성 |
| range | n부터 1씩 증가한 연속된 수를 m개 emit 하는 Flux를 생성 |
| defer | 구독하는 시점에 emmit 하는 Flux 또는 Mono를 생성 |
| using | 파라미터로 전달받은 resource를 emit하는 Flux를 생성 |

### 잘 이해가 가지 않았던 내용
### defer
* 데이터 emit을 지연시키기 때문에 꼭 필요한 시점에 데이터를 emit하여 불필요한 프로세스를 줄일 수 있음
### deferMono와 justMono 차이
* justMono: 이미 정의된 값을 사용하여 Mono를 즉시 생성
* deferMono: Mono를 생성하는 Supplier를 지연 실행하며, Mono가 실제로 구독될 때마다 Supplier 코드가 실행
* justMono의 경우 출력된 현재시간 데이터가 지연 시간과는 상관없이 동일한 시간을 출력하는 이유는?
