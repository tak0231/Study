### 바인딩 (Binding)
- 메서드 호출과 메서드 본문의 연결을 의미
- 또는 프로그래밍에서 각종 값들이 확정되어 더 이상 변경할 수 없는 구속(bind) 상태가 되는 것
- 프로그램 내에서 식별자(identifier)가 그 대상인 메모리 주소, 데이터형 또는 실제 값으로 배정되는 것
- 프로그램에 사용된 구성 요소의 실제 값 또는 프로퍼티를 결정짓는 행위를 의미

- 즉, 프로그래머가 코딩을 해서 컴파일 하면 프로그래머가 값을 변경할 수 없는 상태가 되는데 이 상태가 "바인딩"

- 실행(Runtime) 이전에 값이 확정 되는 것 = 정적 바인딩 / 실행 이후 = 동적 바인딩

### 정적 바인딩(Static Binding)
- 컴파일 시간(CompileTime) 혹은 링크 시에 확정되는 바인딩
- 컴파일 : 코딩 -> 기계어 번역하는 과정

- 컴파일 시간에 성격이 결정 됨
- 변수의 타입이 수퍼 클래스 -> 수퍼클래스의 메소드를 호출함

** 정적 바인딩인 경우 **
- 오버로딩의 경우 정적 바인딩
- private, final, statc이 붙은 메서드


### 동적 바인딩(Dynamic Binding)
- 프로그램이 실행되는 과정에서 바인딩 되는 것

- 런타임 시점에 형성된 객체 타입을 기준으로 실행될 함수를 결정하고 호출한다는 의미
- 실행시간(Runtime), 즉 파일을 실행하는 시점에 성격이 결정되는 것
- 다형성 사용하여 메서드 호출시 주로 발생


- Java에서 다형성, 상속이 가능한 이유

- ex) Animal 클래스를 상속받는 Dog라는 클래스가 있다
- 이 때, Dog는 Animal 클래스에 존재하는 메서드 howl()을 재정의 하였다 (Overriding)
- 그럼 main에서 Animal a = new Dog(); a.howl(); 입력시 Dog서 정의된 내용이 출력된다

- 즉, 컴파일 시점에서는 Anima의 메서드가 출력되는 것이 맞지만,
- 런타임 시점에서는 new 연산자 통해 Dog 객체 생성되고, 오버라이딩 된 howl을 호출하게 되는 것
- => 실제 참조하는 객체는 서브클래스(Dog) -> 때문에 서브 클래스의 메소드

- 예외 : static 메소드 -> 컴파일 시점에서 결정됨 => 동적 바인딩이 발생 X

** 동적 바인딩인 경우 **
- 오버라이딩

### Static Method Overriding
- static 메소드는 오버라이딩 할 수 없음
- static 메소드 -> 컴파일 시 메모리에 올라감 => 메소드 영역에 존재하게 됨
- 즉, 객체 생성과 관련 X -> 해당 클래스로부터 모든 인스턴스가 공유됨
- static 메소드는 클래스 단위로 만들어짐 => 때문에 Overriding이 될 수 없음

<br/>

### 참고
- https://joomn11.tistory.com/12
- https://h-coding.tistory.com/37?category=945005
- https://woovictory.github.io/2020/07/05/Java-binding/
- https://sorjfkrh5078.tistory.com/87