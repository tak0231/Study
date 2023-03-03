
![image](https://user-images.githubusercontent.com/80720210/176351722-f0edf33e-c073-4428-8daa-ed79915848cd.png)

## JDBC
- 참고 : https://velog.io/@sloools/Spring-JDBC-Datasource-%EA%B8%B0%EC%B4%88-%EC%A0%95%EB%A6%AC
- 참고 : https://chojpsh1.tistory.com/114


- DB에 접근할 수 있도록 자바에서 제공하는 API
- JAVA 추상화의 대표적인 예
- 어떤 DB서버든 같은 메소드로 접근함
- (환경의 변화와 관계없이 일관된 방식의 기술로 접근하는 구조)

### JDBC Template
- Spring JDBC 접근 방법 중 하나
- plain JDBC에서 수행하는 반복적인 저수준 작업을 내부에서 처리함
- 개발자는 connection 연결, 종료 등의 세부적인 작업을 spring에 위임함

- JDBC를 사용하기 위해서는 "DataSource"가 필요함

## DataSource
- Connection Pool을 관리하는 목적으로 사요되는 객체
- connectino pool을 설정, 관리하는 "인터페이스"

### Connection pool
- connection을 모아두는 장소

### Connection
- DB에 접근할 때마다 connection을 맺고 끊어주는 작업 필요
- 이 작업을 줄이기 위해서 이 connection을 미리 생성해 둠
- DB에 접근할 떄, 생성해 둔 conecction을 제공하고 돌려받음

### Datasource 역할
1. DB 서버와 연결
2. DB Connection polling
> 일정량의 Connectiond르 미리 생성시켜 저장소에 저장해둔후
> DB 접근 요청이 있을시 저장소에서 Connection을 제공하는 기법
> 자바에서 DB 연결은 시간이 많이 걸림
> 때문에 Connection Polling 사용하면 부하를 줄일 수 있음
3. 트랜잭션 관리

## MyBatis
- 참고 : https://jung-story.tistory.com/121
- 참고 : https://linked2ev.github.io/mybatis/2019/09/08/MyBatis-1-MyBatis-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EA%B5%AC%EC%A1%B0/


- 자바의 JDBC를 이용한 Persistance framework(퍼시스턴스 프레임워크)
  - SQL, 동적쿼리, 저장 프로시저, 고급 매핑 들을 지원하는 SQL mapper
  - SQL쿼리들을 따로 XML 파일로 작성하여 프로그램 코드, SQL 코드관리에 용이함

- 기존의 JDBC만을 이용할 경우 -> SQL문 처리하고 rs.next()등으로 하나씩 받아와야 했음
  - Mybatis에서는 상당부분의 코드, 파라미터 설정 및 ResultSet 결과를 대신 해줌
  - 코드 중복, 무의미한 코드 작성 생랴 가능 & xml 파일 -> 변환 자유롭고 가독성 좋아짐


### MyBatis 특징
![image](https://user-images.githubusercontent.com/80720210/176353571-0013fef5-4a6c-406b-893f-259eac3d4494.png)
(Database Access 구조

1. Mybatis3와 Spring 연동 라이브러리 제공
2. 싱글톤 패턴으로 스프링빈에 등록, 주입 하여 쉽게 사용 가능함
3. 파라미터와 겨로가를 객체(DTO, map 등)로 자동 매핑을 지원함
4. 스프링 연동 모듈을 제공 -> 스프링 설정이 간단
5. 트랜잭션을 관리해줌 -> 쉽게 설정 가능



## 주요 구성요소
| 구성요소/구성파일 | 설명 |
|:---|:---|
|Mybatis configuration file</br>(=xml or java 파일)| MyBatis의 작업 설정을 설명하는 XML 파일.</br></br> 데이터베이스의 연결 대상, 매핑 파일의 경로, Mybatis의 작업 설정 등과 같은 세부 사항을 결정하는 파일</br></br>Mybatis3의 기본 작업을 변경 or 확장할 떄 설정하는데 사용|
|org.apache.ibatis.session.SqlSessionFactoryBuilder</br>(=SqlSesseionFactoryBuilder)|Mybatis3 구성 파일을 읽고 생성하는 SqlSessionFactory 구성 요소</br></br>이 구성요소는 Spring과 통합되어 사용할 떄 애플리케이션 클래스에서 직접 처리하지 않음|
|org.apache.ibatis.session.SqlSessionFactory</br>(=SqlSesseionFactory)|SqlSession을 생성하는 구성요소</br></br> 이 구성요소는 Spring과 통합되어 사용할 떄 애플리케이션 클래스에서 직접 처리하지 않음|
|org.apache.ibatis.session.SqlSession</br>(=SqlSesseion)|SQL 실행 및 트랜젝션 제어를 위한 API를 제공하는 구성요소.</br></br> Mybatis를 사용하여 데이터베이스에 엑세스할 떄 가장 중요한 역할을 하는 구성요소.</br></br>이 구성요소를 Spring과 통합하여 사용할 경우 애플리케이션 클래스에서 직접 처리하지않음|
|Mapper interface</br>(=@Mapper 붙는 파일)|tyesafe안에서 Mapping file에 정의된 SQL을 호출하는 인터페이스</br></br>MyBatis3는 Mapper 인터페이스에 대한 구현 클래스를 자동으로 생성함 -> 개발자는 인터페이스만 생성하면 됨|
|Mapping file</br>(=XXXMapper.xml)|SQL 및 O/R매핑 설정을 설명하는 XML 파일|


### typesafe
- 타입에 안정적인 것을 의미함
  - typesafe 하다 = 타입을 판별(type check)할 수 있어 Runtime시가 아닌 컴파일시 문제를 잡을 수 있다는 것을 의미
  - 타입이 불안정하다 = 타입을 판별하지 못해 Runtime시 타입으로 인한 문제가 발생한다는 것

- 어떠한 오퍼레이션(또는 연산)도 정의되지 앟는 결과를 내놓지 않는 것
  - 즉, 예측 불가능한 결과를 내지 않는 것을 의미

- ex) 1+"1"
  - 문자열에 숫자 1 할당하는 것 -> 비논리적
  - 이런 연산, 조작에 있어 논리적이지 않은 것에 대해 엄격히 체크하여 Runtime시 이로인한 오류가 발생하지 않도록 하는 것이 typesafe 하단 것


### O/R Mapping = ORM (Object Relational Mapping)
- 참고 : https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=3546love&logNo=120008321307
- 객체지향언어(Java)에서 객체와 RDB(관계형 데이터베이스)의 레코드를 매핑하는 것

### 싱글톤 패턴
- 참고 : https://velog.io/@jaeeunxo1/spring-singleton
- 참고 : https://jeong-pro.tistory.com/86
- 객체의 인스턴스가 오직 1개만 생성되는 패턴


## Mabatis 주요 구성요소가 Database Access 하는 순서
![image](https://user-images.githubusercontent.com/80720210/176357151-4459df95-973e-4a1d-94fd-f4725db1e6cd.png)

### 1~3 : 응용 프로그램(=Application) 시작시 수행되는 프로세스
- Applicaiton 컴파일 시 스프링에서 Mybatis의 빈 생성 흐름
1. 응용프로그램(=Application)은 SqlSessionFactorBuilder에 SqlSessionFactory를 빌드하도록 요청함
2. SqlSessionFactoryBuilder는 SqlSessionFactory를 생성하기 위한 Mybatis 구성파일(config)을 읽어옴
3. SqlSessionFactoryBuilder는 Mybatis 구성 파일의 정의에 따라 SqlSessionFactory를 생성함

- 즉, 응용프로그램 요청 -> FactoryBuilder가 config파일을 일거와서 Factory를 생성


### 4-10 : 클라이언트의 각 요청에 대해 수행되는 프로세스
- 어플리케이션 내에서 개발자가 구현한 사용자의 데이터 요청 흐름
4. 클라이언트가 응용프로그램에 대한 프로세스를 요청함
5. 응용프로그램 -> SqlSessionFactoryBuilder를 사용하여 빌드된 SqlSessionFactory에서 SqlSession을 가져옴
6. SqlSessionFactory는 SqlSession을 생성하고 이를 애플리케이션에 반환함
7. 응용프로그램이 SqlSession에서 Mapper 인터페이스의 구현 객체(implementation object)를 가져옴
8. 응용프로그램이 Mapper 인터페이스 메서드를 호출함
9. Mapper 인터페이스의 구현 객체가 SqlSession 메서드를 호출하고 SQL 실행을 요청함
10. SqlSession은 Mapping File(XXXMapper.xml)에서 실행할 SQL을 가져와 실행함

- 빌드된 SqlSessionFactory 안에 SqlSession이 존재함
- 클라이언트의 요청에 의해 응용프로그램이 실행되면, 이걸(SqlSession) 생성해서 응용프로그램에 다시 돌려줌
- 그럼 응용프로그램은 전달받은 SqlSession은 구현 객체를 가져오고
- 요청받은 기능에 맞춰 Mapper interface에 명시된 메서드를 불러와서
- 그 요청과 일치하는 Mapper 구현 객체를 가져온 SqlSession의 Method와, 안에 적힌 SQL의 호출을 요청
- 그리고 SqlSession이 mapper.xml(매핑파일)에서 sql을 가져와서 실행시킴


## Mybatis-Spring의 컴포넌트 구조
|구성요소/구성파일|설명|
|:--|:--|
|org.mybatis.spring.SqlSesionFactoryBean</br>(=SqlSessionFactoryBean)|SqlSessionFactory를 작성하고 Spring DI 컨테이너에 개체를 저장하는 구성요소</br></br>표준 Mybatis에서 SqlSessionFactory는 Mybatis 구성 파일에 정의된 정보를 기반으로 함</br></br>but SqlSessionFactoryBean을 사용하면 Mybatis 구성파일(Config) 없이도 SqlSessionFactory를 빌드할 수 있음|
|org.mybatis.spring.mapper.MapperFactoryBean</br>(=MapperFactoryBean)|싱글톤 Mapper 객체를 만들고 SpringDI 컨테이너에 객체를 저장하는 구성요소(즉, Mapper 객체를 Spring Bean 등록)</br></br>Mybatis 표준 매커니즘에 의해 생성된 Mapper객체는 스레드가 안전하지 않음(not thread safe). 때문에 각 스레드에 대한(스레드별로) 인스턴스를 할당해야 했음.</br></br>Mybatis-Spring 구성 요소에 의해 생성된 Mapper 객체는 안전한 Mapper객체를 생성할 수 있음. 따라서 Service등 싱글톤 구성요소에 DI를 적용할 수 있음|
|org.mybatis.spring.SqlSessionTemplate</br>(=SqlSessionTemplate)|SqlSession 인터페이스를 구현하는 싱글톤 버전의 SqlSession 구성요소.</br></br>Mybatis 표준 메커니즘에 의해 생성된 SqlSession 객체는 안전하지 않음. 마찬가지로 각 스레드에 대한 인스턴스를 할닫해야 했음</br></br>Mybatis-Spring 구성요소에서 생성된 SqlSession 객체는 안전한 스레드 SqlSession 객체를 생성할 수 있음. 따라서 Service 등 싱글톤 구성요소에 DI를 적용할 수 있|

### thread safe (thread safety 안전한 스레드, 스레드 안정성)
- 참고 : https://gompangs.tistory.com/entry/OS-Thread-Safe%EB%9E%80
- 멀티 스레드 프로그래밍에서 어떤 함수, 변수, 객체가 여러 스레드로부터 동시에 접근이 이루어져도 프로그램의 실행에 문제가 없는 것을 의미함
- => 하나의 함수가 한 스레드로부터 호출되어 실행중일 때, 다른 스레드가 그 함수를 호출하여 동시에 함께 실행되더라도 각 스레드에서의 수행결과가 올바르게 나오는 것

![image](https://user-images.githubusercontent.com/80720210/176365199-ae49aea4-267f-46e4-af2f-2fb7d3ab4074.png)



### 1~4 : 응용 프로그램(=Application) 시작시 수행되는 프로세스
- Applicaiton 컴파일 시 스프링에서 Mybatis의 빈 생성 흐름
1. SqlSessionFactoryBean은 SqlSessionFactoryBuilder에 SqlSessionFactory 빌드를 요청함
2. SqlSessionFactoryBuilder는 SqlSessionFactory 생성을 위한 Mybatis 구성파일(Config)을 읽어옴
3. SqlSessionFactoryBuilder는 MyBatis 구성 파일의 정의에 따라 SqlSEssion Factory를 생성함
   </br> 생성된 SqlSessionFactorys는 Spring DI 컨테이너에 의해 저장됨
4. MapperFactoryBean은 스레드 안전(thread safe) SqlSession(SqlSessionTemplate)과 스레드 안전 Mapper 객체를 생성함
   </br> 이렇게 생성된 Mapper 객체는 Spring DI 컨테이너에 저장되고, DI는 Service 클래스 등에 적용됨
   </br> Mapper 객체는 스레드 안전 SqlSession(SqlSessionTemplate)을 사용하여 스레드 안전 구현을 제공함

### 5-11 : 클라이언트의 각 요청에 대해 수행되는 프로세스

- 어플리케이션 내에서 개발자가 구현한 사용자의 데이터 요청 흐름

5. 클라이언트는 응용프로그램에 대한 프로세스를 요청함
6. Application(Service)는 DI 컨테이너에 의해 주입된 Mapper Interface 구현체의 메서드를 호출함
7. Mapper 객체는 호출된 메서드에 해당하는 SqlSession(SqlSessionTemplate) 메서드를 호출함
8. SqlSession(SqlSessionTemplate)은 프록시 사용 및 스레드 안전 SqlSession 메서드를 호출함
9. 프록시 사용 및 스레드 안전 SqlSession은 트랜잭션에 할당 된 MyBatis3 표준 SqlSession을 사용 함
10. SqlSessionFactory는 MyBatis 표준  Sqlsession을 반환함.
    </br> 반환된 SqlSession은 트랜잭션에 할당 되었으므로 동일한 트랜잭션 내에 있으면 새 트래잭션을 만들지 않고 동일한 SqlSession이 사용(꽁유됨)
11. MyBatis3 표준 SqlSession은 매핑 파일에서 실행할 SQL을 가져와서 SQL을 실행함



## 커넥션 풀
- 참고 : https://devbox.tistory.com/entry/JSP-%EC%BB%A4%EB%84%A5%EC%85%98-%ED%92%80-1
