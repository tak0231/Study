##
```
>> docker run -itd -p 19090:8080 -p 19050:50000 -v /data/jenkins-volume:/var/jenkins_home -e TZ=Asia/Seoul --name jenkins -u root jenkins/jenkins:lts
```
- TimeZone 설정, slave 통신용 50000포트 추가

### 설치 상세
- docker pull jenkins/jeknins:lts
- port, volume 설정
	- 컨테이너 내부 volume 경로 : /var/jenkins_home
	
- 초기 접근시
	- 초기 비밀번호 : /var/jenkins_home/secrets/initialAdminPassword
	- 나머지는 추천 설정으로

### 참고자료
- jenkins 설치 : https://narup.tistory.com/202
