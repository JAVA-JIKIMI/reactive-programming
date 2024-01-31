| Chapter | Title |
| -- | -- |
| 1 | 리액티브 시스템과 리액티브 프로그래밍 |
| 2 | 리액티브 스트림즈 |
| 3 | Blocking I/O와 Non-Blocking I/O |
| 4 | 리액티브 프로그래밍을 위한 사전 지식 |

# Chapter 1 리액티브 시스템과 리액티브 프로그래밍
* 반응을 잘하는 시스템

### 리액티브 시스템 설계 원칙
![image](https://github.com/JAVA-JIKIMI/reactive-programming/assets/7133516/553cb2d8-a106-4ecb-9a45-6e6e7b9b7078)

#### MEANS
주요 통신 수단으로 비동기 메세지 기반의 통신을 사용 - 구성 요소들 간의 느슨한 결합, 격리성, 위치 투명성을 보장한다.

#### FORM 
- 메세지 기반 통신을 통해 어떠한 형태를 지니는 시스템인가? 탄력성, 회복성을 가짐

#### Resilient 
- 시스템에 장애가 발생하더라도 응답성을 유지하는 것을 의미함.
- 리액티브 시스템에서 회복성이란 시스템에 장애가 발생하더라도 응답성을 유지하는 것을 의미함.
- DR(재해 복구)을 위한 솔루션으로 대비

# 비동기 메세지 기반 통신을 통해 느슨한 결합과 격리성을 보장한다.

#### VALUE
- 비동기 메세지 기반 통신을 바탕으로 한 회복성과 예측 가능한 규모 확장 알고리즘을 통해 시스템의 처리량을 자동으로 확장하고 축소하는 탄력성을 확보

[기억할만 한 내용 또는 느낀점]
* 리액트의 설계 원칙과 특징을 정리해볼 수 있었다. 어떤 시스템의 어떤 기능, 어떤 데이터에 적합한 지 알 수 있다. 

[다시 보기]
* 38page 요약 참고하기

   
# Chapter 2 리액티브 스트림즈 구성 요소
- 데이터 스트림을 Non-Blocking 이면서 비동기적인 방식으로 처리하기 위한 리액티브 라이브러리의 표준 사양
- 구현체는 Reactor 사용
- pub/sub 패턴

[Publisher]
* 데이터를 생성하고 통지(발행, 게시, 방출)하는 역할을 한다.

[Subscriber] 
* 구독한 Publlisher로부터 통지된 데이터를 받아서 처리하는 역할을 한다.

[Subscrition]
* Publisher에 요청할 데이터의 개수를 지정하고, 데이터의 구독을 취소하는 역할을 한다. 

[Processor]
* Publisher, Subscriber의 기능을 모두 가지고 있다. Subscriber로서 다른 Publisher를 구독할 수 있고, Publisher로서 다른 Subscriber가 구독할 수 있다.

- Publisher 구현을 위한 주요 기본 규칙 중 예외에 대한 사항을 기억하자.
- Publisher는 요청된 것보다 적은 수의 onNext signal을 보내고 onComplete 또는 onError를 호출하여 구독을 종류할 수 있다. 
- Publisher가 처리할 데이터가 끊임없이 발생하는 무한 스트림의 경우, 처리 중 에러가 발생하기 전까지는 종료 자체가 없기 때문에 위 내용에 위배된다. 



# Chapter 3 Blocking I/O와 Non-Blocking I/O 
* Blocking I/O 방식의 문제점을 보완하기 위해서 멀티스레딩 기법으로 추가 스레드를 할당하여 차단된 그 시간을 효율적으로 사용할 수 있으나 컨텍스트 스위칭으로 인한 스레드 전환 비용이 발생
- Non-Blocking I/O 방식의 통신에서는 말 그대로 스레드가 차단되지 않음
- Blocking I/O 방식의 통신에서는 해당 스레드가 작업을 처리할 때까지 남아 있는 작업들은 해당 작업이 끝날 때까지 차단되어 대기합니다. 

[기억할만 한 내용 또는 느낀점]
Spring MVC의 문제점을 극복하기 위한 대안으로 등장한 Spring WebFlux 는 Non-Blocking I/O 방식의 통신을 적용
어떤 서비스에 적합한 것인가? 적합하다고 하면 우리 시스템 중 어떤 기능에 녹여볼 수 있을까? (대용량 트래픽, 스트리밍, 실시간, 마이크로 서비스 등)

# Chapter 4 리액티브 시스템과 리액티브 프로그래밍
함수형 프로그래밍과 람다 표현식



| Chapter | Title |
| -- | -- |
| 5 | Reactor 개요 |
| 6 | 마블 다이어그램 |
| 7 | Cold Sequence와 Hot Sequence |  
| 8 | Backpressure |
| 9 | Sinks |
| 10 | Scheduler |

책의 주요 내용을 살펴 보면서 궁금한 점을 파헤쳐보자
* Reactor의 구성요소는?
* Reactor에서 Publisher의 역할
  
| Reactor | Description |
| -- | -- |
| Flux |  0개부터 N개의 데이터를 방출할 수 있는 Publisher |
| Mono | 0개부터 1개의 데이터를 방출할 수 있는 Publisher |

* Cold 와 Hot의 의미란?
  
| Reactor | Description |
| -- | -- |
| Cold | 구독할 때마다 데이터 흐름이 처음부터 시작되는 Sequence |
| Hot | 구독이 발생한 시점 이전에 방출된 데이터는 받지 못하고 시점 이후에 방출된 데이터를 전달 받는 Sequence |

* 마블 다이어그램 쉽게 이해하는 방법은?
* Backpressure 처리 방식은?
* Sink란?


 
