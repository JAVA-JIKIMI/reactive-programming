

| Chapter | Title |
| -- | -- |
| 5 | Reactor 개요 |
| 6 | 마블 다이어그램 |
| 7 | Cold Sequence와 Hot Sequence |  
| 8 | Backpressure |
| 9 | Sinks |
| 10 | Scheduler |

책의 주요 내용을 살펴 보면서 궁금한 점을 파헤쳐보자
# Reactor의 구성요소는?
* Reactor에서 Publisher의 역할
  
| Reactor | Description |
| -- | -- |
| Flux |  0개부터 N개의 데이터를 방출할 수 있는 Publisher |
| Mono | 0개부터 1개의 데이터를 방출할 수 있는 Publisher |

# Cold 와 Hot의 의미란?
  
| Reactor | Description |
| -- | -- |
| Cold | 구독할 때마다 데이터 흐름이 처음부터 시작되는 Sequence |
| Hot | 구독이 발생한 시점 이전에 방출된 데이터는 받지 못하고 시점 이후에 방출된 데이터를 전달 받는 Sequence |

# 마블 다이어그램 쉽게 이해해보자 
#### 마블은 구슬 모양의 도형으로 데이터 처리 흐름을 의미한다.

## Timeline (left to right)
#### 타임라인은 왼쪽에서 오른쪽 방향으로 읽고 최근 발생한 내역은 오른쪽에 가까움

## Element
#### 타임 라인을 지나는 도형을 element라고 하며 마블의 모양과 색과는 관계없이 타임라인 위의 도형은 Mono 또는 Flux를 사용하면 다루게 될 데이터의 요소 
#### Mono는 0개 혹은 1개
#### Flux는 0개 혹은 그 이상의 Element를 다룬다. 
#### 이 때 타임라인은 왼쪽에서 오른쪽으로 이벤트가 발생하는 것을 표시하기 때문에 Reactor에서 데이터는 시간에 따라 연속적으로 발생할 수 있으며 한 개의 데이터 단위를 요소(Element)라고 부른다. 


## onNext
element를 구독(subscribe)하고 나서 다음 element를 읽기 위해서는 onNext()를 사용해서 다음 element를 읽는다.
![image](https://github.com/JAVA-JIKIMI/reactive-programming/assets/7133516/727271e9-2c87-4c7d-89ff-3e40ecc439de)

#### https://projectreactor.io/docs/core/release/reference/#howtoReadMarbles 

# Backpressure 처리 방식은?
publisher가 끊임없이 emit하는 무수히 많은 데이터를 적절하게 제어하여 데이터 처리에 과부하가 걸리지 않도록 제어하는 것

| Category | Description |
| -- | -- |
| IGNORE | Backpressure를 적용하지 않음 |
| ERROR | Downstream으로 전달할 데이터가 버퍼에 가득 찰 경우 Exception을 발생시킴 |
| DROP | Downstream으로 전달할 데이터가 버퍼에 가득 찰 경우 버퍼 밖에서 대기하는 먼저 emit된 데이터부터 Drop 시킴 |
| LATEST | Downstream으로 전달할 데이터가 버퍼에 가득 찰 경우, 버퍼 밖에서 대기하는 가장 최근에 emit된 데이터부터 버퍼에 채움 |
| BUFFER | Downstream으로 전달할 데이터가 버퍼에 가득 찰 경우, 버퍼 안에 있는 데이터부터 Drop 시킴 |

# Sink란?
* Sinks는 Publisher와 Subscriber의 기능을 모두 지닌 Processor의 향상된 기능을 제공
* Reactive Streams에서 발생하는 signal을 프로그래밍적으로 push할 수 있는 기능을 가지고 있는 Publisher의 일종
* Sinks는 Sinks.Many 또는 Sinks.One interface를 사용해서 Thread-Safe하게 signal을 발생시킴
* 멀티 스레드 방식으로 Signal을 전송해도 스레드 안정성을 보장하기 때문에 예기치 않은 동작으로 이어지는 것을 방지해 줌

# Reactor의 Scheduler
* 비동기 프로그래밍을 위해 사용되는 스레드를 관리해주는 역할을 함
* Scheduler를 사용하여 어떤 스레드에서 무엇을 처리할지 제어함
* Scheduler를 위한 전용 Operator

| Operator | Description |
| -- | -- |
| subscribeOn | 구독이 발생한 직후에 실행될 스레드를 지정하는 Operator |
| publishOn | Downstream으로 Signal을 전송할 때 실행되는 스레드를 제어하는 역할을 하는 Operator |
| parallel | Round robin 방식으로 CPU 코어 개수만큼의 스레드를 병렬로 실행 |
