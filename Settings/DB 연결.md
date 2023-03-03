- 참고 : https://araikuma.tistory.com/484
- 참고(경로) : https://itcoolly.tistory.com/148
- 참고(구조) : https://chojpsh1.tistory.com/114
- 참고(application.properties 관련) : https://awse2050.tistory.com/65

## Mariadb
- 참고 : https://blogger.pe.kr/885
- 참고 : https://damin8.github.io/spring/2020/07/21/Spring-Boot+MariaDB+MyBatis(1)/
- 참고 : https://docs.3rdeyesys.com/database/ncloud_database_mariadb_access_from_remote_ubuntu.html

- (계정생성) -> 스키마(DB) 생성 -> 테이블 생성


### 권한 부여
- grant all privileges on 'DB명'.'테이블명' to '계정명'@'접속위치' identified by 비밀번호;
  - db명, 테이블명의 * => 모든 db
  - 접속위치의 % => 외부 접속도 허용한다는의미
    - 내부접속만 허용시 => 'root'@'내부IP주소'
  
- flush privileges
  - 권한 적용

```
> grant all privileges on *.* to 'root'@'%' identified by '내계정비밀번호';
> flush privileges
```

- sudo vi /etc/mysql/mariadb.conf.d/50-server.cnf
  - 해당 파일에서 bind-address  부분을 주석처리하기

- sudo systemctl restart mariadb
  - 이후 mariadb 재시작

> - 참고 : https://postitforhooney.tistory.com/entry/MySql-Mariadb-MYsql-%EC%82%AC%EC%9A%A9%EC%9E%90-%EA%B6%8C%ED%95%9C%EC%A3%BC%EA%B8%B0-%EB%B0%8F-%ED%99%95%EC%9D%B8
> - 참고 : https://postitforhooney.tistory.com/entry/MySql-Mariadb-MYsql-%EC%82%AC%EC%9A%A9%EC%9E%90-%EA%B6%8C%ED%95%9C%EC%A3%BC%EA%B8%B0-%EB%B0%8F-%ED%99%95%EC%9D%B8
> - 참고 : https://dukkoong.tistory.com/32
> - 참고 : https://prod.velog.io/@sheltonwon/Ubuntu%EC%97%90-MariaDB-%EC%84%A4%EC%B9%98-%ED%9B%84-%EC%99%B8%EB%B6%80-%EC%A0%91%EC%86%8D


## DBeaver
> - 참고 : https://jhhan009.tistory.com/75
> - 참고 : https://computer-science-student.tistory.com/506

- 특정 DB에 접근하는 경우에만 초기 접속 설정시 Databases에 적어주면되고, 아니면 비워두면 됨


## springboot - mysql - mybatis 연동
- 참고 : https://limjunho.github.io/2021/08/11/spring-mysql.html
- 참고 : https://devlog-wjdrbs96.tistory.com/200

## springboot - mariadb - mybatis 연동
- 참고 : https://rangsub.tistory.com/99
- 참고 : https://www.devkcj.com/gatsby-springMVC-2/
- 참고 : https://www.holaxprogramming.com/2015/10/18/spring-boot-with-mybatis/


### DataSource Mybatis 관련
- https://www.wrapuppro.com/programing/view/4yO1xLOCovPwa4R
