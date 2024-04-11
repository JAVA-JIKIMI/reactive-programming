# Spring Data R2DBC
### 리액티브 API 제공
### 클라이언트의 요청부터 데이터 베이스 액세스까지 완전한 Non-Blocking 애플리케이션 구현
### 직접 테이블 스크립트를 작성해야함

# 예외처리
### onErrorResume() Operator 
### 이벤트가 발생했을 때  에러 이벤트를 Downstream으로 전파하지 않고 대체 Publisher를 통해 에러 이벤트에 대한 대체 값을 emit 하거나 발생한 에러 이벤트를 래핑한 후에 다시 에러 이벤트를 발생시키는 역할을 함

### 에러가 발생하는 Observable을 설정함. 먼저, 예외가 발생할 수 있는 Observable을 설정. 이는 주로 네트워크 호출이나 파일 시스템 액세스 등의 비동기 작업을 수행하는 Observable임
### onErrorResume()을 사용하여 대체 Observable을 지정: onErrorResume() 오퍼레이터를 사용하여 예외가 발생했을 때 대체할 Observable을 지정. 이 대체 Observable은 원래 Observable에서 예외가 발생했을 때 실행됨
### subscribe()를 사용하여 결과를 처리: 마지막으로, Observable을 구독하여 결과를 처리합니다. 이 때 대체 Observable이나 원래 Observable 중 하나의 결과가 전달됩니다.






