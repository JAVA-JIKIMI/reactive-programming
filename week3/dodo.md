[원문](https://xavy.notion.site/202401-fdd758dacfef4f62b8d3bc41b94e7ba1?pvs=4)

# Context

<aside>
💡 어떤 것을 이해하는데 도움이 될만한 관련 정보나 이벤트, 상황

</aside>

- 구독이 발생할 때마다 해당 구독과 연결된 하나의 context가 생긴다.

238p

### Context는 Operator 체인의 아래에서 위로 전파된다.

- 늦게 선언된 context가 사용된다.

> Context는 Subscriber의 구독 메커니즘과 연결이 되어 있고, 구독이 발생했다는 signal(subscription signal)이 upstream으로 이동하는것과 같은 방식으로 Context에 wirte을 할 수 있다라는 설명이 예시를 통해 나와 있습니다.
>
- [답변 출처](https://www.inflearn.com/questions/1085489/contextwrite%EC%9D%98-%EC%8B%A4%ED%96%89%EC%8B%9C%EC%A0%90)
- [공식 문서](https://projectreactor.io/docs/core/release/reference/#context.write), [문서2](https://projectreactor.io/docs/core/release/reference/#_simple_context_examples)

250p