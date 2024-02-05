# spring reactor에서 buffer가 뭘까

publisher가 emit(방출)하는 데이터들을 그룹화해놓는 공간. 즉 spring reactor에서의 buffer는 publisher의 영역에 속함

# sinks? thread safe?

processor는 backpressure를 지원하지 않음. 오버플로우 발생 가능성 존재. 그리고 processor가 thread safe하지 않는다는 것은 '데이터 무결성이 해쳐지는 것' 보다 '데이터 처리 순서가 보장되지 않는다는 것'을 의미.
