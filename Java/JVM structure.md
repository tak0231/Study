## [JVM의 구성]
- 크게 3부분으로 나뉨

![image](https://user-images.githubusercontent.com/80720210/175872974-b7a6d59b-f453-4121-91fb-391abb1f46ff.png)

### 클래스 로더(Class Loader)
- class file 및 libraries를 읽어들여 클래스 객체를 생성

- Compile Time이 아닌 RunTime에 Class를 로딩할 수 있게 해주는 기술
- 런타임시에 모든 코드가 JVM에 링크, 로딩됨
- 자바 -> java.lang.ClassLoader를 제공

- 동적으로 클래스를 로딩한다 = JVM은 클래스에 대한 어떤 정보도 가지고 있지 않다는 것을 의미
- RunTime에 메소드, 필드, 상속관계 등을 "동적으로" 확인, 검증함

- 클래스 로딩 방식 2가지 : load-time dynmic loading / runtime dynamic loading



### 데이터 영역(Runtime data area)
- 로딩된 Class를 수행하기 위해 할당받은 메모리 공간
- 힙 / 스택 / 스태틱 영역

### 실행 엔진(Excution engine)
- 로딩된 클래스를 실행


<br/>

### 참고
- https://joomn11.tistory.com/11?category=854732
