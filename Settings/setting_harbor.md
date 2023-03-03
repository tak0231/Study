## 인증서 등록 시
- 우분투는 updata-ca-trust 대신에 update-ca-certificates 명령어 사용해야 함
- ca 위치 경로도 다음과 같음
	- /usr/local/share/ca-certificates

## port 관련
- An attempt was made to access a socket in a way forbidden by its access permissions 에러 발생 시
	- 참고자료의 "소켓 관련 에러" 부분 참고
	- powershell에서 포트 제외 목록에 추가해야 함
	- ubuntu, docker desktop 실행 이전에 해야 함
	- powershell에서 관리자 권한으로 실행 후
		- ```
			netsh interface ipv4 show excludedportrange protocol=tcp
		  ```
			- 현재 예약된 포트 확인
			
		- ```
			netsh int ipv4 add excludedportrange protocol=tcp startport=5432 numberofports=1 store=persistent
		  ```
			- 포트 예약
			- startport = 내가 사용할 포트
			- numberofports = startport 부터 이후로 몇개의 포트 사용할지
			
## 도메인 관련
- harbor 설치 후 ubuntu의 hostname으로 접근 가능
	- 혹은 localhost
	- 추가적으로 hosts name으로 등록 가능
	
### 참고자료
- Harbor 설치 1 : https://ikcoo.tistory.com/229
- Harbor 설치 2 : https://mightytedkim.tistory.com/23
- Harbor 설치 3 : https://kyh0703.github.io/install/post-install-harbor/
- CSR 신청 데이터 : https://blusky10.tistory.com/352
- 데비안 update-ca-trust 대신 : https://zetawiki.com/wiki/%EB%8D%B0%EB%B9%84%EC%95%88_update-ca-certificates
- 우분투 CA 인증서 추가 : https://zetawiki.com/wiki/%EC%9A%B0%EB%B6%84%ED%88%AC_CA_%EC%9D%B8%EC%A6%9D%EC%84%9C_%EC%B6%94%EA%B0%80
- 소켓 관련 에러 : https://velog.io/@dom_hxrdy/Cannot-start-service-backend-Ports-are-not-available-listen-tcp-0.0.0.03001-bind-An-attempt-was-made-to-access-a-socket-in-a-way-forbidden-by-its-access-permissions.-에러-해결하기
