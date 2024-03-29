# 1주차 정인철



# 📌 인상 깊었던 내용

## **📚** 리액티브 시스템 설계 원칙

> 투명성 : 비동기 메시지 기반의 통신을 통해서 구성요소들 간의 느슨한 결합, 격리성, 위치 투명성을 보장한다
> 
> 탄력성 : 시스템으로 유입되는 입력량이 변화하더라도 응답성을 일정하게 유지해야 한다. 이는 입력을 처리하기 위한 시스템 자원을 실시간으로 추가하거나 감소시켜서 작업량의 변화에 대응해야 한다
> 
> 회복성 : 리액티브 시스템의 구성요소들은 비동기 메시지 기반 통신을 통해 느슨한 결합과 격리성을 보장해야 한다
> 
> 📕 29p 5번째 (1장)
>

**🧐 : 리액티브 프로그래밍을 특징을 잘 설명한 부분인것 같다**

## **📚 선언형 프로그래밍 vs 명령형 프로그래밍**

> 1. 선언형 프로그래밍 방식에서는 동작을 구체적으로 명시하지 않고목표만 선언한다.
> 2. 선언형 프로그래밍에서는 여러 가지 동작을 각각 별도의코드로 분리하지 않고 각 동작에 대해서 메서드 체인을 형성해서한 문장으로 된 코드로 구성한다.
> 3. 선언형 프로그래밍 방식은 함수형 프로그래밍으로 구성된다.
> 
> 📕 35p 16번째 (1장)
>

**🧐 : 용어에 대한 차이를 알아보면 이해하고 있으면 좋을것 같다**

## **📚 리액티브 프로그래밍의 특징**

> - 리액티브 프로그래밍은선언형 프로그래밍 방식이다. 그렇기 때문에 실행할 동작을 구체적으로 명시하지 않고 목표만 선언한다.
> - 데이터 소스의 변경이 있을 때마다 데이터를 전파한다
> - 리액티브 프로그래밍 코드는 코드의 간결함과 가독성에 유리한 메서드 체인의 형태로 표현된다.
> - 리액티브 프로그래밍 코드에서 파라미터를 가지는 메서드는 함수형 프로그래밍 방식의 코드 형태의 파라미터를 가진다
> 
> 📕 38p 10번째 (1장)
>

**🧐 : 리액티브 프로그래밍 특징 잘 알고 있자**

---

# 📌 이해가 가지 않았던 내용

## **📚 논블럭킹 I/O**

> 블럭킹 I/O 방식의 통신에서는 해당 스레드가 처리할 때까지 남아 있는 작업들은 해당 작업이 끝날 때까지 차단되어 대기한다. 그렇기 때문에 뒤에 남아 있는 작업들을 대기 없이 처리하려면 별도의추가 스레드를 할당해야 해서 그만큼의 비용이 더 들게 된다.
> 논블럭킹 I/O 통신에서는 말 그대로 스레드가 차단되지 않는다.
> 
> 📕 31p 1번째 (1장)
>

**🧐 : 블럭킹과 논블럭킹의 차이는 동일한 리소스로 성능을 최대한 끌어들일수 있는 차이라고 생각했는데… 비용적으로는 차이가 있을까..?
→ 65p에 나오는데 프로세스의 정보를 PCB에 저장하고 reload하는 시간 동안에는 CPU가 다른 작업을 하지 못하고 대기하게 된다. 당연히 컨텍스트 스위칭이 많으면 많을수록 CPU의 전체 대기 시간은 길어지기 때문에 성능이 저하된다고 볼수 있다**

---

# 📌 논의해보고 싶었던 내용