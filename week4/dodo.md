[원문](https://xavy.notion.site/202401-fdd758dacfef4f62b8d3bc41b94e7ba1?pvs=4)

## Operators

308p

### Mono

- [JustOrEmpty](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html#justOrEmpty-T-)
- [Defer](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html#defer-java.util.function.Supplier-)
- [And](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html#and-org.reactivestreams.Publisher-)

### Flux

- [FromIterable](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#fromIterable-java.lang.Iterable-)
- [FromStream](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#fromStream-java.util.stream.Stream-)
- [Range](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#range-int-int-)
- [Using](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#using-java.util.concurrent.Callable-java.util.function.Function-)
- [Generate](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#generate-java.util.concurrent.Callable-java.util.function.BiFunction-)
- [Create](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#create-java.util.function.Consumer-)
- [Filter](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#filter-java.util.function.Predicate-)
- [Skip](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#skip-long-)
- [Take](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#take-long-)
- [Next](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#next--)
- [Map](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#map-java.util.function.Function-)
- [FlatMap](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#flatMap-java.util.function.Function-)
- [Concat](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#concat-java.lang.Iterable-)
- [Merge](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#merge-int-org.reactivestreams.Publisher...-)
- [Zip](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#zip-java.util.function.Function-int-org.reactivestreams.Publisher...-)
- [CollectList](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#collectList--)
- [CollectMap](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#collectMap-java.util.function.Function-)
- [Error](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#error-java.util.function.Supplier-)
- [onErrorReturn](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#onErrorReturn-java.lang.Class-T-)
- [onErrorResume](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#onErrorResume-java.lang.Class-java.util.function.Function-)
- [onErrorContinue](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#onErrorContinue-java.util.function.BiConsumer-)
- [Retry](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#retry--)
- [Elapsed](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#elapsed--)
- [Window](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#window-java.time.Duration-)
- [Buffer](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#buffer--)
- [BufferTimeout](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#bufferTimeout-int-java.time.Duration-)
- [GroupBy](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#groupBy-java.util.function.Function-)
- [Publish](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Flux.html#publish--)

### **ConnectableFlux**

- [autoConnect](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/ConnectableFlux.html#autoConnect--)
- [refCount](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/ConnectableFlux.html#refCount--)

### doOn~()

데이터의 변경없이 부수효과만을 수행하는 operator